# 配置POLARDB Reader {#concept_ud2_vzb_5fb .concept}

本文为您介绍POLARDB Reader支持的数据类型、读取方式、字段映射和数据源等参数及配置举例。

POLARDB Reader插件通过JDBC连接器连接到远程的POLARDB数据库，并根据您配置的信息生成查询SQL语句，发送到远程POLARDB数据库，并将该SQL语句执行返回结果，然后使用数据同步自定义的数据类型拼装为抽象的数据集，并传递给下游Writer处理。

POLARDB Reader插件通过JDBC连接远程POLARDB数据库，并执行相应的SQL语句，将数据从POLARDB库中Select出来，从底层实现了从POLARDB数据库读取数据。POLARDB Reader插件支持读取表和视图。表字段可以依序指定全部列、指定部分列、调整列顺序、指定常量字段和配置POLARDB的函数如now\(\)等。

## 类型转换列表 {#section_ny3_4fr_5fb .section}

POLARDB Writer针对POLARDB类型的转换列表，如下所示。

|类型分类|POLARDB数据类型|
|:---|:----------|
|整数类|Int，Tinyint，Smallint，Mediumint，Int，Bigint|
|浮点类|Float，Double，Decimal|
|字符串类|Varchar，Char，Tinytext，Text，Mediumtext，Longtext|
|日期时间类|Date，Datetime，Timestamp，Time，Year|
|布尔型|Bit，Bool|
|二进制类|Tinyblob，Mediumblob，Blob，Longblob，Varbinary|

**说明：** 

-   除上述罗列字段类型外，其他类型均不支持。
-   POLARDB Reader插件将tinyint\(1\)视作整型。

## 参数说明 {#section_vpb_wfr_5fb .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|table|选取的需要同步的表名称，一个数据集成Job只能同步一张表。|是|无|
|column|所配置的表中需要同步的列名集合，使用JSON的数组描述字段信息 。默认使用所有列配置，例如\[ \* \]。-   支持列裁剪，即列可以挑选部分列进行导出。
-   支持列换序，即列可以不按照表schema信息顺序进行导出。
-   支持常量配置，您需要按照MySQL SQL语法格式，例如`[“id”, “table”,”1”, “‘mingya.wmy’”, “‘null’”, “to_char(a+1)”, “2.3” , “true”]`。
    -   id为普通列名
    -   table为包含保留字的列名
    -   1为整形数字常量
    -   ‘mingya.wmy’为字符串常量（注意需要加上一对单引号）
    -   null为空指针
    -   CHAR\_LENGTH\(s\)为计算字符串长度函数
    -   2.3为浮点数
    -   true为布尔值
-   column必须显示指定同步的列集合，不允许为空。

|是|无|
|splitPk|POLARDB Reader进行数据抽取时，如果指定splitPk，表示您希望使用splitPk代表的字段进行数据分片，数据同步因此会启动并发任务进行数据同步，这样可以大大提高数据同步的效能。-   推荐splitPk用户使用表主键，因为表主键通常情况下比较均匀，因此切分出来的分片也不容易出现数据热点。
-   目前splitPk仅支持整型数据切分，不支持字符串、浮点、日期等其他类型 。如果您指定其他非支持类型，忽略plitPk功能，使用单通道进行同步。
-   如果splitPk不填写，包括不提供splitPk或者splitPk值为空，数据同步视作使用单通道同步该表数据 。

|否|无|
|where|筛选条件，在实际业务场景中，往往会选择当天的数据进行同步，将where条件指定为gmt\_create\>$bizdate。-   where条件可以有效地进行业务增量同步。如果不填写where语句，包括不提供where的key或value，数据同步均视作同步全量数据。
-   不可以将where条件指定为limit 10，这不符合MySQL SQL WHERE子句约束。

|否|无|
|querySql（高级模式，向导模式不提供）|在部分业务场景中，where配置项不足以描述所筛选的条件，您可以通过该配置型来自定义筛选SQL 。当配置此项后，数据同步系统就会忽略tables、columns和splitPk配置项，直接使用这项配置的内容对数据进行筛选，例如需要进行多表 join 后同步数据，使用`select a,b from table_a join table_b on table_a.id = table_b.id`。当您配置querySql时，POLARDB Reader直接忽略table、column、where和splitPk条件的配置，querySql优先级大于table、column、where、splitPk选项。datasource会使用它解析出用户名和密码等信息。|否|无|
|singleOrMulti（只适合分库分表）|表示分库分表，向导模式转换成脚本模式主动生成此配置`“singleOrMulti”: “multi”`，但是配置脚本任务模板不会直接生成此配置必须手动添加，否则只会识别第一个数据源。singleOrMulti只是前端在用，后端没有用这个做分库分表判断。|是|multi|

## 向导开发介绍 {#section_jvk_sgr_5fb .section}

1.  **选择数据源**

    配置同步任务的**数据来源**和**数据去向**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62197/154229539032058_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源**|即上述参数说明中的datasource，一般填写您配置的数据源名称。|
    |**表**|即上述参数说明中的table，选择需要同步的表。|
    |**导入前准备语句**|即上述参数说明中的preSql，输入执行数据同步任务之前率先执行的SQL语句。|
    |**导入后完成语句**|即上述参数说明中的postSql，输入执行数据同步任务之后执行的SQL语句。|
    |**主键冲突**|即上述参数说明中的writeMode，可选择需要的导入模式。|

2.  **字段映射**，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62209/154229539032015_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**同名映射**|单击**同名映射**，可以根据名称建立相应的映射关系，请注意匹配数据类型。|
    |**同行映射**|单击**同行映射**，可以在同行建立相应的映射关系，请注意匹配数据类型。|
    |**取消映射**|单击**取消映射**，可以取消建立的映射关系。|
    |**自动排版**|可以根据相应的规律自动排版。|

3.  **通道控制**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62209/154229539032018_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**DMU**|数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。|
    |**作业并发数**|可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度。|
    |**错误记录数**|错误记录数，表示脏数据的最大容忍条数。|
    |**任务资源组**|任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增调度资源](cn.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。|


## 脚本开发介绍 {#section_vw5_p3r_5fb .section}

单库单表的脚本样例如下，详情请参见上述参数说明。

```
{
    "type": "job",
    "steps": [
        {
            "parameter": {
                "datasource": "test_005",//数据源名
                "column": [//源端列名
                    "id",
                    "name",
                    "age",
                    "sex",
                    "salary",
                    "interest"
                ],
                "where": "id=1001",//过滤条件
                "splitPk": "id",//切分键
                "table": "polardb_person"//源端表名
            },
            "name": "Reader",
            "category": "reader"
        },
        {
            "parameter": {}
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
            "throttle": false,//同步速率限流
            "dmu": 6//dmu值
        }
    }
}
```

