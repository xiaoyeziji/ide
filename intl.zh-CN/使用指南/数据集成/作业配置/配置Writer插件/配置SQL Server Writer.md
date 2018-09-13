# 配置SQL Server Writer {#concept_sdt_fb4_q2b .concept}

SQL Server Writer插件实现了写入数据到SQL Server主库的目标表的功能。在底层实现上， SQL Server Writer通过JDBC连接远程SQL Server数据库，并执行相应的`insert into...`的SQL语句将数据写入SQL Server，内部会分批次提交入库。

SQL Server Writer面向ETL开发工程师，他们使用SQL Server Writer从数仓导入数据到SQL Server。同时SQL Server Writer可以作为数据迁移工具为DBA等用户提供服务。

SQL Server Writer通过数据集成框架获取Reader生成的协议数据，生成`insert into...`当主键/唯一性索引冲突时，会写不进去冲突的行，出于性能考虑采用了`PreparedStatement+Batch`并且设置了`rewriteBatchedStatements=true`，将数据缓冲到线程上下文Buffer中，当Buffer累计到预定阈值时，才发起写入请求。

**说明：** 

-   目标表所在数据库必须是主库才能写入数据。
-   整个任务至少需要具备insert into…的权限，是否需要其他权限，取决于您配置任务时在preSql和postSql中指定的语句。

## 类型转换列表 {#section_vcg_fvn_q2b .section}

SQL Server Writer支持大部分SQL Server类型，但也存在部分类型没有支持的情况，请注意检查您的类型。

SQL Server Writer针对SQL Server的类型转换列表，如下所示。

|类型分类|SQL Server数据类型|
|:---|:-------------|
|整数类| Bigint、Int、Smallint和Tinyint

 |
|浮点类|Float、Decimal、Real何Numeric|
|字符串类| Char、NChar、NText、NVarchar、Text、Varchar、NVarchar（MAX）和Varchar（MAX）

 |
|日期时间类|Date、Time和Datetime|
|布尔类|Bit|
|二进制类|Binary、Varbinary、Varbinary（MAX）和Timestamp|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|table|选取的需要同步的表名称。|是|无|
|column|目标表需要写入数据的字段，字段之间用英文逗号分隔。例如`"column":["id","name","age"]`。如果要依次写入全部列，使用\*表示，例如`"column":["*"]`。|是|无|
|preSql|执行数据同步任务之前率先执行的SQL语句。目前向导模式仅允许执行一条SQL语句，脚本模式可以支持多条SQL语句，例如清除旧数据。|否|无|
|postSql|执行数据同步任务之后执行的SQL语句。目前向导模式仅允许执行一条SQL语句，脚本模式可以支持多条SQL语句，例如加上某一个时间戳。|否|无|
|writeMode|选择导入模式，可以支持insert方式。insert：当主键/唯一性索引冲突时，数据集成视为脏数据但保留原有的数据。

|否|insert|
|batchSize|一次性批量提交的记录数大小，该值可以极大减少数据集成与PostgreSQL的网络交互次数，并提升整体吞吐量。但是该值设置过大可能会造成数据集成运行进程OOM情况。|否|1024|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16255/15368019398218_zh-CN.png)

    配置项说明如下：

    -   数据源：即上述参数说明中的datasource，一般填写您配置的数据源名称。
    -   表：即上述参数说明中的table，选择需要同步的表。
    -   导入前准备语句：即上述参数说明中的preSql，输入执行数据同步任务之前率先执行的SQL语句。
    -   导入后完成语句：即上述参数说明中的postSql，输入执行数据同步任务之后执行的SQL语句。
    -   主键冲突：即上述参数说明中的writeMode，可选择需要的导入模式。
2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16255/15368019398219_zh-CN.png)

    -   同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。
    -   自动排版：可以根据相应的规律自动排版。
3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15368019397675_zh-CN.png)

    配置项说明如下：

    -   DMU：数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
    -   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度
    -   错误记录数：错误记录数，表示脏数据的最大容忍条数。
    -   任务资源组：任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。

## 向导模式介绍 {#section_cp2_wsh_p2b .section}

配置写入SQL Server的作业，具体参数填写请参见参数说明。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[//下面是关于Reader的模板，可以找相应的读插件文档
        {
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType":"sqlserver",//插件名
            "parameter":{
                "postSql":[],//执行数据同步任务之后率先执行的 SQL 语句
                "datasource":"",//数据源
                "column":[//字段
                    "id",
                    "name"
                ],
                "table":"",//表名
                "preSql":[]//执行数据同步任务之前率先执行的 SQL 语句
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
            "throttle":false,////false代表不限流，下面的限流的速度不生效，true代表限流
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

