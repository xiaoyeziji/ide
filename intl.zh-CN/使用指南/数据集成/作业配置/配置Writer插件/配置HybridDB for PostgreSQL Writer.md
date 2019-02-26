# 配置HybridDB for PostgreSQL Writer {#concept_vzd_n1c_5fb .concept}

本文将为您介绍HybridDB for PostgreSQL Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

HybridDB for PostgreSQL Writer插件实现了向HybridDB for PostgreSQL写入数据。在底层实现上，HybridDB for PostgreSQL Writer通过JDBC连接远程HybridDB for PostgreSQL数据库，并执行相应的SQL语句将数据从PHybridDB for PostgreSQL库中SELECT出来，RDS在公共云提供HybridDB for PostgreSQL存储引擎。

**说明：** 在开始配置HybridDB for PostgreSQL Writer插件前，请首先配置好数据源，详情请参见[配置HybridDB for PostgreSQL数据源](intl.zh-CN/使用指南/数据集成/数据源配置/配置HybridDB for PostgreSQL数据源.md#)。

简而言之，HybridDB for PostgreSQL Writer通过JDBC连接器连接到远程的HybridDB for PostgreSQL数据库，根据您配置的信息生成查询SELECT SQL语句并发送到远程HybridDB for PostgreSQL数据库，并将该SQL执行返回结果使用CDP自定义的数据类型拼装为抽象的数据集，并传递给下游Writer处理。

-   对于您配置的table、column和where等信息，HybridDB for PostgreSQL Writer将其拼接为SQL语句发送到HybridDB for PostgreSQL数据库。
-   对于您配置的querySql信息，HybridDB for PostgreSQL直接将其发送到HybridDB for PostgreSQL数据库。

## 类型转换列表 {#section_enj_rjr_5fb .section}

HybridDB for PostgreSQL Writer支持大部分HybridDB for PostgreSQL类型，但也存在部分类型没有支持的情况，请注意检查您的类型。

PHybridDB for PostgreSQL Writer针对HybridDB for PostgreSQL的类型转换列表，如下所示。

|类型分类|HybridDB for PostgreSQL数据类型|
|:---|:--------------------------|
|Long|Bigint、Bigserial、Integer、Smallint和Serial|
|Double|Double、Precision、Money、Numeric和Real|
|String|Varchar、Char、Text、Bit和Inet|
|Date|Date、Time和Timestamp|
|Boolean|Bool|
|Bytes|Bytea|

**说明：** 

-   除上述罗列字段类型外，其他类型均不支持。
-   Money、Inet和Bit需要您使用a\_inet::varchar类似的语法进行转换。

## 参数说明 {#section_pld_gkr_5fb .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|table|选取的需要同步的表名称。|是|无|
|writeMode|选择导入模式，可以支持insert方式。insert：当主键/唯一性索引冲突时，数据集成视为脏数据但保留原有的数据。|否|insert|
|column|目标表需要写入数据的字段，字段之间用英文逗号分隔。例如`"column":["id","name","age"]`。如果要依次写入全部列，使用\*表示，例如"column":\["\*"\]。|是|无|
|preSql|执行数据同步任务之前率先执行的SQL语句。目前向导模式仅允许执行一条SQL语句，脚本模式可以支持多条SQL语句，例如清除旧数据。|否|无|
|postSql|执行数据同步任务之后执行的SQL语句。目前向导模式仅允许执行一条SQL语句，脚本模式可以支持多条SQL语句，例如加上某一个时间戳。|否|无|
|batchSize|一次性批量提交的记录数大小，该值可以极大减少数据集成与HybridDB for PostgreSQL的网络交互次数，并提升整体吞吐量。但是该值设置过大可能会造成数据集成运行进程OOM情况。|否|1024|

## 向导开发介绍 {#section_akn_qlr_5fb .section}

1.  **选择数据源**

    配置同步任务的**数据来源**和**数据去向**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62207/155117025532025_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源**|即上述参数说明中的datasource，一般选择您配置的数据源名称。|
    |**表**|即上述参数说明中的table，选择需要同步的表。|
    |**导入前准备语句**|即上述参数说明中的preSql，输入执行数据同步任务之前率先执行的SQL语句。|
    |**导入后完成语句**|即上述参数说明中的postSql，输入执行数据同步任务之后执行的SQL语句。|

2.  **字段映射**，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，将鼠标放到需要删除的字段，即可选择**删除**按钮。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62207/155117025532030_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**同名映射**|单击**同名映射**，可以根据名称建立相应的映射关系，请注意匹配数据类型。|
    |**同行映射**|单击**同行映射**，可以在同行建立相应的映射关系，请注意匹配数据类型。|
    |**取消映射**|单击**取消映射**，可以取消建立的映射关系。|
    |**自动排版**|可以根据相应的规律自动排版。|

3.  **通道控制**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62209/155117025532018_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**DMU**|数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。|
    |**作业并发数**|可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度。|
    |**错误记录数**|错误记录数，表示脏数据的最大容忍条数。|
    |**任务资源组**|任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增任务资源](intl.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。|


## 脚本开发介绍 {#section_ugh_hnr_5fb .section}

```
{
    "type": "job",
    "steps": [
        {
            "parameter": {},
            "name": "Reader",
            "category": "reader"
        },
        {
            "parameter": {
                "postSql": [],//导入后完成语句
                "datasource": "test_004",//数据源名
                "column": [//目标表的列名
                    "id",
                    "name",
                    "sex",
                    "salary",
                    "age"
                ],
                "table": "public.person",//目标表名
                "preSql": []//导入前准备语句
            },
            "name": "Writer",
            "category": "writer"
        }
    ],
    "version": "2.0",//版本号
    "order": {
        "hops": [
            {
                "from": "Reader",
                "to": "Writer"
            }
        ]
    },
    "setting": {
        "errorLimit": {//错误记录数
            "record": ""
        },
        "speed": {
            "concurrent": 6,//并发数
            "throttle": false,//同步速率是否限流
            "dmu": 7//dmu值
        }
    }
}
```

