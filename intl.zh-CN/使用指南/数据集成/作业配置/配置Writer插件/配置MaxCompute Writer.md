# 配置MaxCompute Writer {#concept_jjy_y4m_q2b .concept}

本文为您介绍MaxCompute Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

MaxCompute Writer插件用于实现向MaxCompute中插入或者更新数据，主要适用于ETL开发同学，可以将业务数据导入MaxCompute，适合于TB、GB等数量级的数据传输。

**说明：** 在开始配置MaxCompute Writer插件前，请首先配置好数据源，详情请参见[配置MaxCompute数据源](intl.zh-CN/使用指南/数据集成/数据源配置/配置MaxCompute数据源.md#)。

有关MaxCompute的详细介绍请参见[MaxCompute简介](https://www.alibabacloud.com/help/doc-detail/27800.htm)。

根据您配置的源头项目/表/分区/表字段等信息，在底层实现上可通过Tunnel将数据写入MaxCompute中。常用的Tunnel命令请参见[Tunnel命令操作](https://www.alibabacloud.com/help/doc-detail/27833.htm)。

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

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|table|写入的数据表的表名称（大小写不敏感），不支持填写多张表。|是|无|
|partition|需要写入数据表的分区信息，必须指定到最后一级分区。例如把数据写入一个三级分区表，必须配置到最后一级分区，例如pt=20150101, type＝1, biz=2。-   对于非分区表，该值务必不要填写，表示直接导入到目标表。
-   MaxCompute Writer不支持数据路由写入，对于分区表请务必保证写入数据到最末一级分区 。

|如果表为分区表，则必填。如果表为非分区表，则不能填写。|无|
|column|需要导入的字段列表，当导入全部字段时，可以配置为`"column": ["*"]`，当需要插入部分MaxCompute列，则填写部分列，例如`"column": ["id, name"]`。-   MaxCompute Writer支持列筛选、列换序，例如一张表中有a、b和c三个字段，您只同步c和b两个字段，则可以配置为`"column": ["c, b"]`，在导入过程中，字段a自动补空，设置为null。
-   column必须显示指定同步的列集合，不允许为空。

 |是|无|
|truncate|通过配置`"truncate": "true"`保证写入的幂等性，即当出现写入失败再次运行时，MaxCompute Writer将清理前述数据，并导入新数据，这样可以保证每次重跑之后的数据都保持一致 。truncate选项不是原子操作。因为利用MaxCompute SQL进行数据清理工作，SQL无法做到原子性。因此当多个任务同时向一个Table/Partition清理分区时，可能出现并发时序问题，请务必注意。

针对这类问题，建议您尽量不要多个作业DDL同时操作同一个分区，或者在多个并发作业启动前，提前创建分区。

|是|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16247/15432255998125_zh-CN.png)

    配置项说明如下：

    -   数据源：即上述参数说明中的datasource，一般填写您配置的数据源名称。
    -   表：即上述参数说明中的table，选择需要同步的表。
    -   分区信息：如果是指定所有的列，可以在column配置，例如"column": \[""\]。partition支持配置多个分区和通配符的配置方法。

        -   `"partition":"pt=20140501/ds=*"`代表ds中的所有的分区。
        -   `"partition":"pt=top?"`中的?代表前面的字符是否存在，指pt=top和pt=to两个分区。
        可以输入您要同步的分区列，如分区列有pt等。例如MaxCompute的分区为pt=$\{bdp.system.bizdate\}，您可以直接将您的分区的名称pt添加到源头表字段中，可能会有未识别的标志直接忽视下一步，如果要同步所有的分区将前面显示的分区值配置成为pt=$\{\*\}，如果是同步某个分区可以直接选择您要同步的时间值。

    -   清理规则：
        -   写入前清理已有数据：导数据之前，清空表或者分区的所有数据，相当于insert overwrite。
        -   写入前保留已有数据：导数据之前不清理任何数据，每次运行数据都是追加进去的，相当于insert into。
    -   压缩：默认选择不压缩。
    -   空字符串是否作null ：默认值为是。
2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    -   同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。
    -   自动排版：可以根据相应的规律自动排版。
3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15432255997675_zh-CN.png)

    配置项说明如下：

    -   DMU：数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
    -   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度
    -   错误记录数：错误记录数，表示脏数据的最大容忍条数。
    -   任务资源组：任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

脚本配置样例如下，详情请参见上述参数说明。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {//下面是以stream为例子，如果是其他的插件可以找对应的插件填写相应的内容
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType":"odps",//插件名
            "parameter":{
                "partition":"",//分区信息
                "truncate":true,//写入规则
                "compress":false,//是否压缩
                "datasource":"odps_first",//数据源名
                "column":[//列名
                    "*"
                ],
                "emptyAsNull":false,//空字符串是否作为null
                "table":""//表名
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record":"0"//错误记录数超过
        },
        "speed":{
            "throttle":false,//是否限流
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

## 补充说明 {#section_j3w_xrm_q2b .section}

**关于列筛选的问题**

MaxCompute本身不支持列筛选、重排序和补空等，但MaxCompute Writer完成了上述需求，支持列筛选、重排序和补空。例如需要导入的字段列表，当导入全部字段时，可以配置为`"column": ["*"]`。

MaxCompute表有a、b和c三个字段，您只同步c和b两个字段，可以将列配置为`"column": ["c","b"]`，表示会把Reader的第一列和第二列导入MaxCompute的c字段和b字段，而MaxCompute表中新插入的a字段会被置为null。

**列配置错误的处理**

为了保证写入数据的可靠性，避免多余列数据丢失造成数据质量故障。对于写入多余的列，MaxCompute Writer将报错。例如MaxCompute表字段为a、b和c，但如果MaxCompute Writer写入的字段多于3列，MaxCompute Writer将报错。

**分区配置注意事项**

MaxCompute Writer仅提供写入到最后一级分区的功能，不支持写入按照某个字段进行分区路由等功能。假设表一共有3级分区，那么在分区配置中就必须指明写入到某个三级分区，例如把数据写入一个表的第三级分区，可以配置为pt=20150101, type=1, biz=2，但不能配置为pt=20150101, type=1或者pt=20150101。

**任务重跑和failover**

MaxCompute Writer通过配置`"truncate": true`，保证写入的幂等性，即当出现写入失败再次运行时，MaxCompute Writer将清理前述数据，并导入新数据，这样可以保证每次重跑之后的数据都保持一致。如果在运行过程中因为其他的异常导致了任务中断，便不能保证数据的原子性，数据不会回滚也不会自动重跑，需要您利用幂等性这一特点重跑去确保保证数据的完整性。

**说明：** 

truncate为true的情况下，会将指定分区\\表的数据全部清理，请谨慎使用。

