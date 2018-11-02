# 配置DRDS Writer {#concept_tgk_fkl_q2b .concept}

本文为您介绍DRDS Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

DRDS Writer插件实现将数据写到DRDS表的功能。在底层DRDS Writer是通过JDBC连接远程DRDS数据库的Proxy，执行相应的`replace into…`的SQL语句将数据写入DRDS。

**说明：** 执行的SQL语句是replace into，为避免数据重复写入，需要您的表具备主键（Primary Key）或者唯一性索引（Unique index）。开始配置DRDS Writer插件前，请首先配置好数据源，详情请参见[配置DRDS数据源](intl.zh-CN/使用指南/数据集成/数据源配置/配置DRDS数据源.md#)。

DRDS Writer面向ETL开发工程师，使用DRDS Writer从数仓导入数据到DRDS。同时DRDS Writer也可以作为数据迁移工具为DBA等用户提供服务。

DRDS Writer通过CDP框架获取Reader生成的协议数据，通过`replace into...`（没有遇到主键/唯一性索引冲突时，与insert into行为一致。冲突时会用新行替换原有行所有字段）的语句写入数据到DRDS。DRDS Writer累积一定数据，提交给DRDS的Proxy，该Proxy内部决定数据是写入一张还是多张表以及多张表写入时如何路由数据。

**说明：** 整个任务至少需要具备replace into…的权限，是否需要其他权限，取决于您配置任务时在preSql和postSql中指定的语句。

类似于MySQL Writer，目前DRDS Writer支持大部分MySQL类型，但也存在部分类型没有支持的情况，请注意检查您的类型。

DRDS Writer针对DRDS类型的转换列表，如下所示。

|类型分类|DRDS数据类型|
|:---|:-------|
|整数类|Int、Tinyint、Smallint、Mediumint、Bigint和Year|
|浮点类|Float、Double和Decimal|
|字符串类|Varchar、Char、Tinytext、Text、Mediumtext和LongText|
|日期时间类|Date、Datetime、Timestamp和Time|
|布尔类|Bit和Bool|
|二进制类|Tinyblob、Mediumblob、Blob、Longblob和Varbinary|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|table|所选取的需要同步的表。|是|无|
|writeMode|选择导入模式，可以支持replace/insert ignore方式。-   replace：当没有遇到主键/唯一性索引冲突时，与insert行为一致。当主键/唯一性索引冲突时，会用新行替换原有行所有字段。
-   insert ignore：当主键/唯一性索引冲突时，数据集成将直接忽略更新丢弃，并且不记录。

|否|insert ignore|
|column|目标表需要写入数据的字段，字段之间用英文逗号分隔。例如"column": \["id", "name", "age"\]。如果要依次写入全部列，使用\*表示。例如"column": \["\*"\] 。|是|无|
|preSql|执行数据同步任务之前率先执行的SQL语句，目前向导模式仅允许执行一条SQL语句，脚本模式可以支持多条SQL语句，例如清除旧数据。|否|无|
|postSql|执行数据同步任务之后执行的SQL语句，目前向导模式仅允许执行一条SQL语句，脚本模式可以支持多条SQL语句，例如加上某一个时间戳。|否|无|
|batchSize|一次性批量提交的记录数大小，该值可以极大减少数据集成与MySQL的网络交互次数，并提升整体吞吐量。但是该值设置过大可能会造成数据集成运行进程OOM情况。|否|1024|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    配置项说明如下：

    -   数据源：即上述参数说明中的datasource，一般填写您配置的数据源名称。
    -   表：即上述参数说明中的table，选择需要同步的表。
    -   导入前准备语句：即上述参数说明中的preSql，输入执行数据同步任务之前率先执行的SQL语句。
    -   导入后完成语句：即上述参数说明中的postSql，输入执行数据同步任务之后执行的SQL语句。
2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16242/15411401908014_zh-CN.png)

    -   同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。
    -   自动排版：可以根据相应的规律自动排版。
3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15411401907675_zh-CN.png)

    配置项说明如下：

    -   DMU：数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
    -   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度
    -   错误记录数：错误记录数，表示脏数据的最大容忍条数。
    -   任务资源组：任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置写入DRDS的数据同步作业。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {//下面是关于Reader的模板，可以找相应的读插件文档
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType":"drds",//插件名
            "parameter":{
                "postSql":[],//执行数据同步任务之后执行的 SQL 语句
                "datasource":"",//数据源
                "column":[//列名
                    "id"
                ],
                "writeMode":"insert ignore",
                "batchSize":"1024",//一次性批量提交的记录数大小
                "table":"test",//表名
                "preSql":[]//执行数据同步任务之前执行的 SQL 语句
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
            "concurrent":1,//并发数
            "dmu":1//DMU数
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

