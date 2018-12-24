# 配置HybridDB for PostgreSQL Reader {#concept_es2_5zb_5fb .concept}

本文将为您介绍HybridDB for PostgreSQL Reader支持的数据类型、读取方式、字段映射和数据源等参数及配置举例。

HybridDB for PostgreSQL Reader插件从HybridDB for PostgreSQL读取数据。在底层实现上，HybridDB for PostgreSQL Reader通过JDBC连接远程HybridDB for PostgreSQL数据库并执行相应的SQL语句，将数据从HybridDB for PostgreSQL库中SELECT出来。RDS在公共云提供HybridDB for PostgreSQL存储引擎。

简而言之，HybridDB for PostgreSQL Reader通过JDBC连接器连接到远程的HybridDB for PostgreSQL数据库，根据您配置的信息生成查询SELECT SQL语句并发送到远程HybridDB for PostgreSQL数据库，将该SQL执行返回结果使用数据集成自定义的数据类型拼装为抽象的数据集，并传递给下游Writer处理。

-   对于您配置的table、column和where等信息，HybridDB for PostgreSQL Reader将其拼接为SQL语句发送到HybridDB for PostgreSQL数据库。
-   对于您配置的querySql信息，HybridDB for PostgreSQL直接将其发送到HybridDB for PostgreSQL数据库。

## 类型转换列表 {#section_ny3_4fr_5fb .section}

HybridDB for PostgreSQL Reader支持大部分HybridDB for PostgreSQL类型，但也存在部分类型没有支持的情况，请注意检查您的类型。

HybridDB for PostgreSQL Reader针对HybridDB for PostgreSQL的类型转换列表，如下所示。

|类型分类|HybridDB for PostgreSQL数据类型|
|:---|:--------------------------|
|整数类|Bigint、Bigserial、Integer、Smallint和Serial|
|浮点类|Double、Precision、Money、Numeric和Real|
|字符串类|Varchar、Char、Text、Bit和Inet|
|日期时间类|Date、Time和Timestamp|
|布尔型|Bool|
|二进制类|Bytea|

## 参数说明 {#section_vpb_wfr_5fb .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|table|选取的需要同步的表名称。|是|无|
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
|splitPk|HybridDB for PostgreSQL Reader进行数据抽取时，如果指定splitPk，表示您希望使用splitPk代表的字段进行数据分片，数据同步因此会启动并发任务进行数据同步，这样可以大大提高数据同步的效能。-   推荐splitPk用户使用表主键，因为表主键通常情况下比较均匀，因此切分出来的分片也不容易出现数据热点。
-   目前splitPk仅支持整型数据切分，不支持字符串、浮点、日期等其他类型 。如果您指定其他非支持类型，忽略plitPk功能，使用单通道进行同步。
-   如果splitPk不填写，包括不提供splitPk或者splitPk值为空，数据同步视作使用单通道同步该表数据 。

|否|无|
|where|筛选条件，HybridDB for PostgreSQLReader根据指定的column、table和where条件拼接SQL，并根据这个SQL进行数据抽取。例如在做测试时，可以将where条件指定实际业务场景，往往会选择当天的数据进行同步，将where条件指定为id\>2 and sex=1 。-   where条件可以有效地进行业务增量同步。
-   where条件不配置或者为空，视作全表同步数据。

|否|无|
|querySql（高级模式，向导模式不提供）|在部分业务场景中，where配置项不足以描述所筛选的条件，您可以通过该配置型来自定义筛选SQL。当配置此项后，数据同步系统就会忽略tables、columns和splitPk配置项，直接使用这项配置的内容对数据进行筛选，例如需要进行多表join后同步数据，使用`select a,b from table_a join table_b on table_a.id = table_b.id`。当您配置querySql时，HybridDB for PostgreSQL Reader直接忽略table、column、where条件的配置。|否|无|
|fetchSize|该配置项定义了插件和数据库服务器端每次批量数据获取条数，该值决定了数据集成和服务器端的网络交互次数，能够较大的提升数据抽取性能。**说明：** fetchSize值过大（\>2048）可能造成数据同步进程OOM。

 |否|512|

## 向导开发介绍 {#section_jvk_sgr_5fb .section}

1.  **选择数据源**

    配置同步任务的**数据来源**和**数据去向**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62195/154229542332069_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源**|即上述参数说明中的datasource，一般填写您配置的数据源名称。|
    |**表**|即上述参数说明中的table，选择需要同步的表。|
    |**数据过滤**|您将要同步数据的筛选条件，暂时不支持limit关键字过滤。SQL语法与选择的数据源一致。|
    |**切分键**|您可以将源数据表中某一列作为切分键，建议使用主键或有索引的列作为切分键，仅支持类型为整型的字段。读取数据时，根据配置的字段进行数据分片，实现并发读取，可提升数据同步效率。**说明：** 切分键的设置跟数据同步里的选择来源有关，在配置数据来源时才显示切分键配置项。

|

2.  **字段映射**，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62195/154229542332070_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**同名映射**|单击**同名映射**，可以根据名称建立相应的映射关系，请注意匹配数据类型。|
    |**同行映射**|单击**同行映射**，可以在同行建立相应的映射关系，请注意匹配数据类型。|
    |**取消映射**|单击**取消映射**，可以取消建立的映射关系。|
    |**自动排版**|可以根据相应的规律自动排版。|
    |**手动编辑源表字段**|请手动编辑字段，一行表示一个字段，首尾空行会被采用，其他空行会被忽略。|
    |**添加一行**|     -   可以输入常量，输入的值需要使用英文单引号，如’abc’、’123’等。
    -   可以配合调度参数使用，如$\{bizdate\}等。
    -   可以输入关系数据库支持的函数，如now\(\)、count\(1\)等。
    -   如果您输入的值无法解析，则类型显示为未识别。
 |

3.  **通道控制**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62209/154229542332018_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**DMU**|数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。|
    |**作业并发数**|可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度。|
    |**错误记录数**|错误记录数，表示脏数据的最大容忍条数。|
    |**任务资源组**|任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增调度资源](cn.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。|


## 脚本开发介绍 {#section_xqz_qyr_5fb .section}

```
{
    "type": "job",
    "steps": [
        {
            "parameter": {
                "datasource": "test_004",//数据源名
                "column": [//源端表的列名
                    "id",
                    "name",
                    "sex",
                    "salary",
                    "age"
                ],
                "where": "id=1001",//过滤条件
                "splitPk": "id",//切分键
                "table": "public.person"//源端表名
            },
            "name": "Reader",
            "category": "reader"
        },
        {
            "parameter": {},
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

