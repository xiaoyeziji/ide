# 配置MaxCompute Reader {#concept_xtt_j1p_p2b .concept}

本文为您介绍MaxCompute Reader支持的数据类型、字段映射和数据源等参数及配置举例。

MaxCompute Reader插件实现了从MaxCompute读取数据的功能，有关MaxCompute的详细介绍请参见[MaxCompute简介](https://www.alibabacloud.com/help/doc-detail/27800.htm)。

根据您配置的源头项目/表/分区/表字段等信息，在底层实现上可通过Tunnel从MaxCompute系统中读取数据。常用的Tunnel命令请参见[Tunnel命令操作](https://www.alibabacloud.com/help/doc-detail/27833.htm)。

MaxCompute Reader支持读取分区表、非分区表，不支持读取虚拟视图。当要读取分区表时，需要指定出具体的分区配置，比如读取t0表，其分区为pt=1，ds=hangzhou，那么您需要在配置中配置该值。当读取非分区表时，您不能提供分区配置。表字段既可以依序指定全部列、部分列，也可以调整列顺序、指定常量字段和指定分区列（分区列不是表字段）。

## 支持的数据类型 {#section_yn2_gpm_q2b .section}

MaxCompute Writer支持MaxCompute中以下数据类型。

|数据类型|MaxCompute数据类型|
|:---|:-------------|
|整数类|Bigint|
|浮点类|Double和Decimal|
|字符串类|String|
|日期时间类|Datetime|
|布尔类|Boolean|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|table|读取数据表的表名称（大小写不敏感）。|是|无|
|partition|读取数据所在的分区信息，支持linux shell通配符，包括表示0个或多个字符，?代表一个字符是否存在。例如现在有分区表test，其存在pt=1/ds=hangzhou、pt=1/ds=shanghai、pt=2/ds=hangzhou和pt=2/ds=beijing 四个分区。-   如果您想读取pt=1/ds=shanghai分区的数据，那么您应该配置为`"partition":"pt=1/ds=shanghai"`。
-   如果您想读取pt=1下的所有分区，那么您应该配置为`"partition":"pt=1/ds=*"`。
-   如果您想读取整个test表的所有分区的数据，那么您应该配置为`"partition":"pt=*/ds=*"`。

|如果表为分区表，则必填。如果表为非分区表，则不能填写。|无|
|column|读取MaxCompute源头表的列信息。例如现在有表test，其字段为id、name和age。-   如果您想依次读取id、name和age，那么您应该配置为 `"column":["id","name","age"]` 或者配置为`"column":["*"]` 。

**说明：** 不推荐您配置抽取字段为\*，因为它表示依次读取表的每个字段，如果您的表字段顺序调整、类型变更或者个数增减，您的任务就会存在源头表列和目的表列不能对齐的风险，会直接导致您的任务运行结果不正确甚至运行失败。

-   如果您想依次读取name和id，那么您应该配置为`"coulumn":["name","id"]`。
-   如果您想在源头抽取的字段中添加常量字段（以适配目标表的字段顺序）。例如您想抽取的每一行数据值为age列对应的值，name列对应的值，常量日期值1988-08-08 08:08:08，id列对应的值，那么您应该配置为`"column":["age","name","'1988-08-08 08:08:08'","id"]`，即常量列首尾用符号`'` 包住即可。内部实现上识别常量是通过检查您配置的每一个字段，如果发现有字段首尾都有`'`，则认为其是常量字段，其实际值为去除`'`之后的值。

**说明：** 

    -   MaxCompute Reader抽取数据表不是通过MaxCompute的Select SQL语句，所以不能在字段上指定函数。-
    -   column必须显示指定同步的列集合，不允许为空。

 |是|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16225/15432255527759_zh-CN.png)

    配置项说明如下：

    -   数据源：即上述参数说明中的datasource，一般填写您配置的数据源名称。
    -   表：即上述参数说明中的table，选择需要同步的表。
    **说明：** 如果是指定所有的列，可以在column配置，例如"column": \[""\]。partition支持配置多个分区和通配符的配置方法。

    -   `"partition":"pt=20140501/ds=*"`代表ds中的所有的分区。
    -   `"partition":"pt=top?"`中的?代表前面的字符是否存在，指pt=top和pt=to两个分区。
    可以输入您要同步的分区列，如分区列有pt等。例如MaxCompute的分区为pt=$\{bdp.system.bizdate\}，您可以直接将您的分区的名称pt添加到源头表字段中，可能会有未识别的标志直接忽视下一步，如果要同步所有的分区将前面显示的分区值配置成为pt=$\{\*\}，如果是同步某个分区可以直接选择您要同步的时间值。

2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16225/15432255527760_zh-CN.png)

    -   同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。
    -   自动排版：可以根据相应的规律自动排版。
    -   手动编辑源表字段：请手动编辑字段，一行表示一个字段，首尾空行会被采用，其他空行会被忽略。
    添加一行的功能如下所示：

    -   可以输入常量，输入的值需要使用英文单引号，如'abc'、'123'等。
    -   可以配合调度参数使用，如$\{bizdate\}等。
    -   可以输入关系数据库支持的函数，如now\(\)、count\(1\)等。
    -   如果您输入的值无法解析，则类型显示为未识别。
3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15432255527675_zh-CN.png)

    配置项说明如下：

    -   DMU：数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
    -   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度
    -   错误记录数：错误记录数，表示脏数据的最大容忍条数。
    -   任务资源组：任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从MaxCompute抽取数据到本地的作业，详情请参见上述参数说明。

```
{
    "type":"job",
    "version":"2.0",
    "steps":[
        {
            "stepType":"odps",//插件名
            "parameter":{
                "partition":[],//读取数据所在的分区
                "isCompress":false,//是否压缩
                "datasource":"",//数据源
                "column":[//源头表的列信息
                    "id"
                ],
                "emptyAsNull":true,
                "table":""//表名
            },
            "name":"Reader",
            "category":"reader"
        },
        { //下面是关于Writer的模板，可以找相应的写插件文档
            "stepType":"stream",
            "parameter":{
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record":"0"//错误记录数
        },
        "speed":{
            "throttle":false,//false代表不限流，下面的限流的速度不生效，true代表限流
            "concurrent":1,//作业并发数
            "dmu":1//DMU值
        }
    },
    "order":{
        "hops":[
            {
                "from":"Reader",
                "to":"Writer"
            }
        ]
    }
}
```

如果您需要指定MaxCompute 的[Tunnel Endpoint](../../../../intl.zh-CN/准备工作/配置Endpoint.md#)，可通过脚本模式手动配置数据源：将上述示例中的 `"datasource":"",`替换为数据源的具体参数，示例如下：

```
"accessId":"xxxxxxxxxxxxxxxxxxxxxxxx",
 "accessKey":"*******************",
 "endpoint":"http://service.eu-central-1.maxcompute.aliyun-inc.com/api",
 "odpsServer":"http://service.eu-central-1.maxcompute.aliyun-inc.com/api", 
"tunnelServer":"http://dt.eu-central-1.maxcompute.aliyun.com", 
"project":"yyyyyyyy", 
```

