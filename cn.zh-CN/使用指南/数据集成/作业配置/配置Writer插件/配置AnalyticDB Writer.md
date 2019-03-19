# 配置AnalyticDB Writer {#concept_bft_jyk_q2b .concept}

本文将为您介绍AnalyticDB Writer支持的数据源、数据类型、字段映射、参数配置以及一个完整的导入数据操作示例。

数据集成通过实时导入的方式将数据导入AnalyticDB中，要求您必须提前在AnalyticDB中创建好实时表（普通表）。实时导入方式效率高，且流程简单。

如果数据源来源是RDS for SQLServer，详细的导入操作，请参见[使用数据集成迁移](https://help.aliyun.com/document_detail/98790.html)。

开始配置AnalyticDB Writer插件前，请首先配置好数据源，详情请参见[配置AnalyticDB数据源](cn.zh-CN/使用指南/数据集成/数据源配置/配置AnalyticDB数据源.md#)。

## AnalyticDB Writer支持的AnalyticDB数据类型 {#section_rtq_nyr_lgb .section}

|类型|AnalyticDB数据类型|
|:-|:-------------|
|整数类|int、tinyint、smallint、bigint|
|浮点类|float、double|
|字符串类|varchar|
|日期时间类|date、timestamp|
|布尔类|boolean|

## 通过向导模式实时导入数据至AnalyticDB {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的**数据来源**和**数据去向**。

    ![选择数据源](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16239/15529871967998_zh-CN.png)

    |配置项|说明|
    |:--|:-|
    |**数据源**|导入数据的数据来源。|
    |**表**|选择需要同步的表。|
    |**数据过滤**|同步数据的筛选条件，暂时不支持limit关键字过滤。|
    |**切分键**|选择数据源中的主键作为切分键。|

    |配置项|说明|
    |:--|:-|
    |**数据源**|选择ADS，系统将自动关联[配置AnalyticDB数据源](https://help.aliyun.com/document_detail/100590.html)时设置的数据源名称。|
    |**表**|选择AnalyticDB中的一张表，将Reader数据库中的数据同步至该表中。|
    |**切分键**|根据AnalyticDB中表的更新方式设置导入模式，此处为实时导入。|

2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16239/15529871968002_zh-CN.png)

    |配置项|说明|
    |:--|:-|
    |**同行映射**|自动将同一行的数据设置映射关系。单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。|
    |**自动排版**|设置完映射关系后，字段排序展示。|

3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15529871967675_zh-CN.png)

    |配置项|说明|
    |:--|:-|
    |**DMU**|数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。|
    |**作业并发数**|配置的时候会结合读取端指定的切分建，将数据分成多个Task，多个Task同时运行，以达到提速的效果。|
    |**同步速率**|设置同步速率可保护读取端数据库，以避免抽取速度过大，给源库造成太大的压力。同步速率建议限流，结合源库的配置，请合理配置抽取速率。|
    |**错误记录数**|错误记录数，表示脏数据的最大容忍条数。|
    |**任务资源组**|任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增任务资源](cn.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。|

4.  单击**保存**和**提交**，配置任务需要的其他信息，例如基础属性、时间属性、调度依赖以及节点上下文。
5.  完成同步任务的配置后，先**保存**和**提交**节点，再单击**运行**，开始导入操作。

## 通过脚本模式实时导入数据至AnalyticDB {#section_cp2_wsh_p2b .section}

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

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|连接url|AnalyticDB连接信息，格式为Address:Port。|是|无|
|数据库|AnalyticDB的数据库名称。|是|无|
|Access Id|AnalyticDB对应的AccessKey Id。|是|无|
|Access Key|AnalyticDB对应的AccessKey Secret。|是|无|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须与添加的数据源名称保持一致。|是|无|
|table|目标表的表名称。|是|无|
|partition|目标表的分区名称，当目标表为普通表，需要指定该字段。|否|无|
|writeMode|Insert模式，覆盖写（在主键冲突情况下新的记录会覆盖旧的记录）。|是|无|
|column|目的表字段列表，可以为\["\*"\]，或者具体的字段列表，例如\["a", "b", "c"\]。|是|无|
|suffix|AnalyticDB url配置项的格式为ip:port，实际在AnalyticDB数据库访问时，会变成JDBC数据库连接串，此部分为您定制的连接串，可选参数（请参见MySQL支持的JDBC控制参数）。例如配置suffix为autoReconnect=true&failOverReadOnly=false&maxReconnects=10。|否|无|
|batchSize|AnalyticDB提交数据写的批量条数，当writeMode为insert时，该值才会生效。|writeMode为insert时，为必选。|无|
|bufferSize|DataX数据收集缓冲区大小，缓冲区的目的是攒一个较大的Buffer，源头的数据首先进入到此Buffer中进行排序，排序完成后再提交到AnalyticDB。排序是根据AnalyticDB的分区列模式进行的，排序的目的是数据顺序对AnalyticDB服务端更友好（出于性能考虑）。BufferSize缓冲区中的数据会经过batchSize批量提交到ADB中，一般如果要设置bufferSize，设置bufferSize为batchSize数量的多倍。当writeMode为insert时，该值才会生效。

|writeMode为insert时，为必选。|默认不配置不开启此功能。|

