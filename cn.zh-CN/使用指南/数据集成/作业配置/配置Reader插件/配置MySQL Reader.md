# 配置MySQL Reader {#concept_edt_bgp_p2b .concept}

MySQL Reader插件通过JDBC连接器连接到远程的MySQL数据库，并根据您配置的信息生成查询SQL语句并发送到远程MySQL数据库，并将该SQL语句执行返回结果，然后使用数据同步自定义的数据类型拼装为抽象的数据集，并传递给下游Writer处理。

简而言之，MySQL Reader插件通过JDBC连接远程MySQL数据库，并执行相应的SQL语句，将数据从MySQL库中Select出来，从底层实现了从MySQL数据库读取数据。

MySQL Reader插件支持读取表和视图。表字段可以依序指定全部列、指定部分列、调整列顺序、指定常量字段和配置MySQL的函数如now\(\)等。

MySQL Reader支持MySQL中的数据类型，如下所示。

|类型分类|MySQL数据类型|
|----|---------|
|整数类|int，tinyint，smallint，mediumint，int，bigint|
|浮点类|float，double，decimal|
|字符串类|varchar，char，tinytext，text，mediumtext，longtext|
|日期时间类|date，datetime，timestamp，time，year|
|布尔类|bit，bool|
|二进制类|tinyblob，mediumblob，blob，longblob，varbinary|

**说明：** 

-   除上述罗列字段类型外，其他类型均不支持。
-   MySQL Reader插件将tinyint\(1\)视作整型。

## 类型转换列表 {#section_f5g_g1n_q2b .section}

MySQL Writer针对MySQL类型的转换列表，如下所示。

|类型分类|MySQL数据类型|
|:---|:--------|
|整数类|Int、Tinyint、Smallint、Mediumint和Bigint|
|浮点类|Float、Double和Decimal|
|字符串类|Varchar、Char、Tinytext、T ext、Mediumtext和LongText|
|日期时间类|Date、Datetime、Timestamp、Time和Year|
|布尔型|Bool|
|二进制类|Tinyblob、Mediumblob、Blob、LongBlob和Varbinary|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|table|选取的需要同步的表名称，一个数据集成Job只能同步一张表。|是|无|
|column|所配置的表中需要同步的列名集合，使用JSON的数组描述字段信息 。默认使用所有列配置，例如\[ \* \]。-   支持列裁剪，即列可以挑选部分列进行导出。
-   支持列换序，即列可以不按照表schema信息顺序进行导出。
-   支持常量配置，您需要按照MySQL SQL语法格式，例如`["id", "table","1", "'mingya.wmy'", "'null'", "to_char(a+1)", "2.3" , "true"]` 。
    -   id为普通列名
    -   table为包含保留字的列名
    -   1为整形数字常量
    -   'mingya.wmy'为字符串常量（注意需要加上一对单引号）
    -   null为空指针
    -   CHAR\_LENGTH\(s\)为计算字符串长度函数
    -   2.3为浮点数
    -   true为布尔值
-   column必须显示指定同步的列集合，不允许为空。

|是|无|
|splitPk|MySQL Reader进行数据抽取时，如果指定splitPk，表示您希望使用splitPk代表的字段进行数据分片，数据同步因此会启动并发任务进行数据同步，这样可以大大提高数据同步的效能。-   推荐splitPk用户使用表主键，因为表主键通常情况下比较均匀，因此切分出来的分片也不容易出现数据热点。
-   目前splitPk仅支持整型数据切分，不支持字符串、浮点、日期等其他类型 。如果您指定其他非支持类型，忽略plitPk功能，使用单通道进行同步。
-   如果splitPk不填写，包括不提供splitPk或者splitPk值为空，数据同步视作使用单通道同步该表数据 。

|否|无|
|where|筛选条件，在实际业务场景中，往往会选择当天的数据进行同步，将where条件指定为gmt\_create\>$bizdate。-   where条件可以有效地进行业务增量同步。如果不填写where语句，包括不提供where的key或value，数据同步均视作同步全量数据。
-   不可以将where条件指定为limit 10，这不符合MySQL SQL WHERE子句约束。

|否|无|
|querySql（高级模式，向导模式不提供）|在部分业务场景中，where配置项不足以描述所筛选的条件，您可以通过该配置型来自定义筛选SQL 。当配置此项后，数据同步系统就会忽略tables、columns和splitPk配置项，直接使用这项配置的内容对数据进行筛选，例如需要进行多表 join 后同步数据，使用`select a,b from table_a join table_b on table_a.id = table_b.id`。当您配置querySql时，MySQL Reader直接忽略table、column、where和splitPk条件的配置，querySql优先级大于table、column、where、splitPk选项。datasource会使用它解析出用户名和密码等信息。|否|无|
|singleOrMulti（只适合分库分表）|表示分库分表，向导模式转换成脚本模式主动生成此配置`"singleOrMulti": "multi"`，但是配置脚本任务模板不会直接生成此配置必须手动添加，否则只会识别第一个数据源。singleOrMulti只是前端在用，后端没有用这个做分库分表判断。|是|multi|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16227/15368020677781_zh-CN.png)

    配置项说明如下：

    -   数据源：即上述参数说明中的datasource，一般填写您配置的数据源名称。
    -   表：即上述参数说明中的table，选择需要同步的表。
    -   数据过滤：您将要同步数据的筛选条件，暂时不支持limit关键字过滤。SQL语法与选择的数据源一致。
    -   切分键：您可以将源数据表中某一列作为切分键，建议使用主键或有索引的列作为切分键，仅支持类型为整型的字段。

        读取数据时，根据配置的字段进行数据分片，实现并发读取，可提升数据同步效率。

        **说明：** 切分键的设置跟数据同步里的选择来源有关，在配置数据来源时才显示切分键配置项。

2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16227/15368020677782_zh-CN.png)

    -   同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。
    -   自动排版：可以根据相应的规律自动排版。
    -   手动编辑源表字段：请手动编辑字段，一行表示一个字段，首尾空行会被采用，其他空行会被忽略。
    添加一行的功能如下所示：

    -   可以输入常量，输入的值需要使用英文单引号，如'abc'、'123'等。
    -   可以配合调度参数使用，如$\{bizdate\}等。
    -   可以输入关系数据库支持的函数，如now\(\)、count\(1\)等。
    -   如果您输入的值无法解析，则类型显示为未识别。
3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15368020677675_zh-CN.png)

    配置项说明如下：

    -   DMU：数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
    -   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度
    -   错误记录数：错误记录数，表示脏数据的最大容忍条数。
    -   任务资源组：任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

单库单表的脚本样例如下，详情请参见上述参数说明。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"mysql",//插件名
            "parameter":{
                "column":[//列名
                    "id"
                ],
                "connection":[
                    {
                        "datasource":"",//数据源
                        "table":[//表名
                            "xxx"
                        ]
                    }
                ],
                "where":"",//过滤条件
                "splitPk":"",//切分键
                "encoding":"UTF-8"//编码格式
            },
            "name":"Reader",
            "category":"reader"
        },
        {//下面是关于writer的模板，可以找相应的写插件文档
            "stepType":"stream",
            "parameter":{},
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

