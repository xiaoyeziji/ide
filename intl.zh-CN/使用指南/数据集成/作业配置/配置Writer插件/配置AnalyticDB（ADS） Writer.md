# 配置AnalyticDB（ADS） Writer {#concept_bft_jyk_q2b .concept}

er实现了两种向Analytic DB（ADS）导入数据的模式。

-   Load Data（批量导入） 模式：数据中转落地导入方式。
    -   优点：当数据量较大（\>1KW）时，能够以较快速度进行导入。
    -   缺点：需要三方系统授权。
-   Insert Ignore（实时插入）模式：向ADS直接写入的方式。
    -   优点：小批量数据写入能够较快完成（<1KW）。
    -   缺点：大数据导入较慢，不适合大批量数据写入。

在开始配置AnalyticDB Writer插件前，请首先配置好数据源，详情请参见[配置AnalyticDB数据源](intl.zh-CN/使用指南/数据集成/数据源配置/配置AnalyticDB数据源.md#)。

AnalyticDB Writer支持AnalyticDB中以下数据类型。

|类型|AnalyticDB 数据类型|
|:-|:--------------|
|整数类|Int、Tinyint、Smallint、Int和Bigint|
|浮点类|Float和Double|
|字符串类|Varchar|
|日期时间类|Date|
|布尔类|Bool|

## 前提条件 {#section_iv2_mzk_q2b .section}

-   当只有Load Data模式导入一个来源表是MaxCompute表时，需要在MaxCompute中将表Describe和Select权限授权给AnalyticDB的导入账号。

    公共云账号为garuda\_build@aliyun.com以及garuda\_data@aliyun.com（两个都需要授权）。各个专有云的导入账号名请参见专有云的相关配置文档，一般为test1000000009@aliyun.com。

    授权命令如下所示。

    ```
    USE projectname; --表所属MaxCompute project
    ADD USER ALIYUN$xxxx@aliyun.com;--输入正确的云账号（首次添加时）。
    GRANT Describe,Select ON TABLE table_name TO USER ALIYUN$xxxx@aliyun.com;--输入需要赋权的表和正确的云账号
    ```

    另外为了保护您的数据安全，AnalyticDB目前仅允许导入操作者为Proejct Owner的MaxCompute Project的数据，或者操作者为MaxCompute表的owner。

-   ADS Writer仅支持MaxCompute数据源写入，如果其他数据源需要写入到ADS，请先写入到MaxCompute，再写入ADS。

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|url|ADS连接信息，格式为ip:port。|是|无|
|schema|ADS的schema名称。|是|无|
|username|ADS对应的username，目前就是accessId。|是|无|
|password|ADS对应的password，目前就是accessKey。|是|无|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须与添加的数据源名称保持一致。|是|无|
|table|目标表的表名称|是|无|
|partition|目标表的分区名称，当目标表为分区表，需要指定该字段。如果Reader为MaxCompute，且AnalyticDB Writer写入模式为Load模式时，MaxCompute的partition仅支持如下三种配置方式（以两级分区为例）。-   "partition":\["pt=\*, ds=\*"\] （读取表所有分区的数据）。
-   "partition":\["pt=1, ds=\*"\] （读取表下面，一级分区pt=1下面的所有二级分区）。
-   "partition":\["pt=1, ds=hangzhou"\] （读取表下面，一级分区pt=1下面，二级分区ds=hangzhou的数据）。

|否|无|
|writeMode|支持Load Data（批量导入）和Insert Ignore（实时插入）两种写入模式。如果在系统中已经有相同主键的记录，则当前INSERT IGNORE模式的语句执行能成功，但新记录将会被丢弃掉。|是|无|
|column|目的表字段列表，可以为\["\*"\]，或者具体的字段列表，例如\["a", "b", "c"\]。|是|无|
|overWrite|ADS写入是否覆盖当前写入的表，true为覆盖写入，false为不覆盖（追加）写入。当writeMode为Load时，该值才会生效。|是|无|
|lifeCycle|ADS临时表生命周期。当writeMode为Load时，该值才会生效。|是|无|
|suffix|ADS url配置项的格式为ip:port，实际在ADS数据库访问时，会变成JDBC数据库连接串，此部分为您定制的连接串，可选参数（请参见MySQL支持的JDBC控制参数）。例如配置suffix为autoReconnect=true&failOverReadOnly=false&maxReconnects=10。|否|无|
|opIndex|ADS对端存储对应的操作类型列的下标，从0开始。当writeMode为stream时，该值才会生效。|writeMode为stream时，为必选。|无|
|batchSize|ADS提交数据写的批量条数，当writeMode为insert时，该值才会生效。|writeMode为insert时，为必选。|无|
|bufferSize|DataX数据收集缓冲区大小，缓冲区的目的是攒一个较大的Buffer，源头的数据首先进入到此Buffer中进行排序，排序完成后再提交到ADS。排序是根据ADS的分区列模式进行的，排序的目的是数据顺序对ADS服务端更友好（出于性能考虑）。BufferSize缓冲区中的数据会经过batchSize批量提交到ADS中，一般如果要设置bufferSize，设置bufferSize为batchSize数量的多倍。当writeMode为insert时，该值才会生效。

|writeMode为insert时，为必选。|默认不配置不开启此功能。|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16239/15368919547998_zh-CN.png)

    配置项说明如下：

    -   数据源：即上述参数说明中的datasource，一般填写您配置的数据源名称。
    -   表：即上述参数说明中的table，选择需要同步的表。
    -   导入模式：即上述参数说明中的writeMode，支持Load Data（批量导入）和Insert Ignore（实时插入）两种模式。
2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16239/15368919548002_zh-CN.png)

    -   同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。
    -   自动排版：可以根据相应的规律自动排版。
3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15368919547675_zh-CN.png)

    配置项说明如下：

    -   DMU：数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
    -   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度
    -   错误记录数：错误记录数，表示脏数据的最大容忍条数。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

```
{
    "type":"job",
    "version":"2.0",
    "steps":[ //下面是关于Reader的模板，可以找相应的读插件文档
        {
            "stepType":"stream",
            "parameter":{
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType":"ads",//插件名
            "parameter":{
                "partition":"",//目标表的分区名称
                "datasource":"",//数据源
                "column":[//字段
                     "id"
                ],
                "writeMode":"insert",//写入模式
                "batchSize":"256",//一次性批量提交的记录数大小
                "table":"",//表名
                "overWrite":"true"//ADS 写入是否覆盖当前写入的表，true 为覆盖写入，false 为不覆盖（追加）写入。当 writeMode 为 Load 时，该值才会生效。
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

