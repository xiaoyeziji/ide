# 基于PAI模型在线预测服务的实践 {#concept_emy_dpc_z2b .concept}

本文将为您介绍如何基于PAI模型进行在线预测服务。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18990/154270653710837_zh-CN.png)

## 准备环境 {#section_j2c_fqc_z2b .section}

-   OSS认证配置
    -   accessId
    -   accessKey
-   PAI模型服务认证信息
    -   appKey
    -   appSecret
    -   Authorization
-   在AppStudio上创建一个项目

1.  OSS测试数据格式。

    杭州划分为 `61x61`格区域，由 `50`种观测方式（人、机构）的预测值，得到最终自己的预测值。

    例如：`4x4` 区域 `2` 人结果，得到预测值。

    |80|70|
    |90|40|

    |90|80|
    |50|100|

    |85|75|
    |45|95|

    **说明：** 我们会拿到7份数据，各包含50x61x61个观测值。

2.  确定预测服务数据格式。

    假定PAI模型服务已经准备就绪。

    输入格式（50个特征）：

    ```
    [{member_1=2.02, member_2=3.14, ..., member_50=3.14}, ...]
    ```

    输出格式：

    ```
    [{"p_label_value":6.1389000174876625}, ...]
    ```

3.  修改配置。

    在新建的工程里找到： `src/main/resources/application.properties`，这里包含外部数据服务所需的配置。

    填入下图中打码的配置：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18990/154270653710844_zh-CN.png)

    外部数据服务包括：

    -   OssService
    -   PaiApiService

做好准备工作后，便可开始进行预测服务的App开发。

## 定义接口 {#section_edv_dsc_z2b .section}

根据PAI模型的预测服务接口格式，先定义一个接口来match它的格式，测试一下连通性，然后生成代码。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18990/154270653710845_zh-CN.png)

## 实现接口 {#section_fmn_psc_z2b .section}

实现PredictService接口（PredictServiceImpl类）

1.  读取OSS特征数据。

    根据OSS公共文件中提供的数据，我们需要将数据格式做一个转置：将50份文件里的矩阵数据读取出来，汇总成一个50个特征的数组（共61x61个数组）。这里简单点，预估一下全部读取到内存中，用量也不会很多（10^2MB级别左右），可以全部读到一个三维数组中。

    首先注入OssService服务：

    ```
    // PredictServiceImpl.java
    
        private final OssService ossService;
    
        @Autowired
        public PredictServiceImpl(OssService ossService) {
            this.ossService = ossService;
        }
    ```

2.  写一段读取特征的实现，并验证的逻辑。

    ```
    // PredictServiceImpl.java
    
        private static final int FEATURE_NUM = 50;
    
        /**
         * 具体业务处理逻辑
         */   
        @Override
        public List<PredictBO> bizProcess() {
            List<PredictBO> result = new ArrayList<>();
    
            double[][][][] features = new double[7][61][61][FEATURE_NUM];  // d x i x j x k        
            // 1. 读取OSS特征
            for (int k = 0; k < FEATURE_NUM; k++) {
                for (int d = 0; d < 7; d++) {
                    String fileKey = String.format("path/to/data/forecast_%s_75%d.txt",
                        StringUtils.leftPad(String.valueOf(k + 1), 2, "0"), d);
                    List<String> lines = ossService.readOssFileAsLines("<public oss bucketname>", fileKey);
                    for (int j = 0; j < lines.size(); j++) {
                        String[] nums = lines.get(j).split(",");
                        for (int i = 0; i < nums.length; i++) {
                            features[d][i][j][k] = Double.parseDouble(nums[i]);
                        }
                    }
                }
            } // FIXME: 异常处理
    
            // 2. 收集预测结果
            // 这里首先通过一段简单的求平均来验证一下数据读取的逻辑是否正确。
            PredictBO r = new PredictBO();
            r.setP_label_value(DoubleStream.of(features[0][0][0]).average().getAsDouble());
            result.add(r);
            return result;
        }
    ```

3.  运行代码，测试`/predict`接口返回的结果。

    ```
    {
        message: null,
        code: 200,
        success: true,
        data: [
            {
                p_label_value: 3.508
            }
        ]
    }
    ```

    **说明：** 由于每次读取O特S征文件的内容是固定的，我们可以将它优化到构造函数里，起服务时就load好数据。

4.  获取预测结果。

    准备好的模型服务ApiPath，并注入PaiApiService：

    ```
    // PredictServiceImpl.java
    
        private static final String MODEL_API = "/EAPI_xxxxxxx";
    
        private static final double[][][][] features = new double[7][61][61][FEATURE_NUM];  // d x i x j x k
        private final PaiApiService paiApiService;
    
        @Autowired
        public PredictServiceImpl(OssService ossService, PaiApiService paiApiService) {
            this.paiApiService = paiApiService;
            // 1. 读取OSS特征
            //...略
        }
    ```

    根据输入参数的格式，我们需要将50个特征组织成一个Map结构，然后将输入参数转成JSON格式。

    **说明：** 输入参数可以是批量地获取预测结果，只要组织成`[{},{},...,{}]`的结构即可，直接调用`syncPredict`接口。

    具体实现：

    ```
    /**
         * 具体业务处理逻辑
         */   
        @Override
        public List<PredictBO> bizProcess() {
            List<PredictBO> result = new ArrayList<>();
    
            // 2. 收集预测结果
            for (int d = 0; d < 7; d++) {
                result.addAll(predict(d));
            }
            return result;
        }
    
        private List<PredictBO> predict(int d) {
            List<Map<String, Double>> feature = new ArrayList<>();
            for(int n = 0; n < 61 * 61; n++) {
                int i = n % 61;
                int j = n / 61;
                Map<String, Double> unit = new HashMap<>();
                for (int k = 0; k < FEATURE_NUM; k++) {
                    unit.put("member_" + (k + 1), features[d][i][j][k]);
                }
                feature.add(unit);
            }
            ApiResponse response = paiApiService.syncPredict(MODEL_API, JSON.toJSONString(feature).getBytes());
            if (response.getStatusCode() == 200) {
                return JSON.parseArray(new String(response.getBody()), PredictBO.class);
            }
            return Collections.emptyList();
        }
    ```

    启动服务检查7份数据预测的结果。

5.  展示预测结果。

    展示形式可以是多种多样的。这里选择一种比较简单的方式来展示数据。

    假设7份数据代表了某7天（可能是某一周）的降雨量（我们可以根据自己的假设来展示数据），选用了一个图表组件。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18990/154270653710847_zh-CN.png)

    根据组件数据格式要求，对返回值做些微调：

    ```
    @GetMapping(value = "predict")
        @ResponseBody
        public Result predict(){   
            List<PredictBO> predictBOList = predictService.bizProcess();
            List<int[]> result = new ArrayList<>();
            for (int i = 0; i < predictBOList.size();) {
                int[] r = new int[7];
                for (int j = 0; j < 7; j++) {
                    r[j] = predictBOList.get(i + j).getP_label_value().intValue();
                }
                result.add(r);
                i += 7;
            }
            return Result.ofSuccess(result);
        }
    ```

6.  查看展示页面。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18990/154270653710848_zh-CN.png)


