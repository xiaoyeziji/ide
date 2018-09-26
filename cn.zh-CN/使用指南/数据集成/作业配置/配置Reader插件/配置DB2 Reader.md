# 配置DB2 Reader {#concept_ydt_b4h_p2b .concept}

DB2 Reader插件实现了从DB2读取数据。在底层实现上，DB2 Reader通过JDBC连接远程DB2数据库，并执行相应的SQL语句将数据从DB2库中SELECT出来。

简而言之，DB2 Reader通过JDBC连接器连接到远程的DB2数据库，根据您配置的信息生成查询SELECT SQL语句并发送到远程DB2数据库，将该SQL执行返回结果使用数据集成自定义的数据类型拼装为抽象的数据集，并传递给下游Writer处理。

-   对于您配置的table、column、where信息，DB2 Reader将其拼接为SQL语句并发送到DB2数据库。
-   对于您配置的querySql信息，DB2 Reader直接将其发送到DB2数据库。

DB2 Reader支持大部分DB2类型，但也存在部分个别类型没有支持的情况，请注意检查您的类型。

DB2 Reader针对DB2类型的转换列表，如下所示：

|类型分类|DB2数据类型|
|:---|:------|
|整数类|SMALLINT|
|浮点类|Decimal、Real和Double|
|字符串类|Char、Character、Varchar、Graphic、Vargraphic、Long Varchar、Clob、Long vargraphic和DBClob|
|日期时间类|Date、Time和Timestamp|
|布尔类|—|
|二进制类|Blob|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|jdbcUrl|描述的是到DB2数据库的JDBC连接信息，jdbcUrl按照DB2官方规范，DB2格式为jdbc:db2://ip:port/database，并可以填写连接附件控制信息。|是|无|
|username|数据源的用户名。|是|无|
|password|数据源指定用户名的密码。|是|无|
|table|所选取的需要同步的表，一个作业只能支持一个表同步。|是|无|
|column|所配置的表中需要同步的列名集合，使用JSON的数组描述字段信息，默认使用所有列配置，例如\[ \* \]。-   支持列裁剪，即列可以挑选部分列进行导出。
-   支持列换序，即列可以不按照表schema信息顺序进行导出。
-   支持常量配置，您需要按照DB2 SQL语法格式，例如`["id", "1", "'const name'", "null", "upper('abc_lower')", "2.3" , "true"]`。
    -   id为普通列名
    -   1为整型数字常量
    -   'const name'为字符串常量（需要加上一对单引号）
    -   null为空指针
    -   upper\(‘abc\_lower’\)为函数表达式
    -   2.3为浮点数
    -   true为布尔值
-   column必须显示您指定同步的列集合，不允许为空 。

|是|无|
|splitPk|DB2 Reader进行数据抽取时，如果指定splitPk，表示您希望使用splitPk代表的字段进行数据分片，数据同步系统因此会启动并发任务进行数据同步，这样可以大大提供数据同步的效能。-   推荐splitPk用户使用表主键，因为表主键通常情况下比较均匀，因此切分出来的分片也不容易出现数据热点。
-   目前splitPk仅支持整形数据切分，不支持浮点、字符串和日期等其他类型。如果您指定其他非支持类型，DB2 Reader将报错。

|否|空|
|where|筛选条件，DB2 Reader根据指定的column、table、where条件拼接SQL，并根据这个SQL进行数据抽取。在实际业务场景中，往往会选择当天的数据进行同步，可以将where条件指定为gmt\_create\>$bizdate。where条件可以有效地进行业务增量同步。如果该值为空，代表同步全表所有的信息。|否|无|
|querySql|在部分业务场景中，where配置项不足以描述所筛选的条件，您可以通过该配置型来自定义筛选SQL。当您配置了这项后，数据同步系统就会忽略table，column等配置，直接使用这个配置项的内容对数据进行筛选。例如需要进行多表join后同步数据，使用`select a,b from table_a join table_b on table_a.id = table_b.id` 。当您配置querySql时，DB2 Reader直接忽略table、column、where条件的配置。

|否|无|
|fetchSize|该配置项定义了插件和数据库服务器端每次批量数据获取条数，该值决定了数据同步系统和服务器端的网络交互次数，能够较大的提升数据抽取性能。**说明：** fetchSize值过大（\>2048）可能造成数据同步进程OOM。

|否|1024|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂时没有向导开发模式。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从DB2数据库同步抽取数据作业。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"db2",//插件名
            "parameter":{
                "password":"",//密码
                "jdbcUrl":"",//DB2 数据库的 JDBC 连接信息
                "column":[
                    "id"
                ],
                "where":"",//筛选条件
                "splitPk":"",//splitPk 代表的字段进行数据分片
                "table":"",//表名 
                "username":""//用户名
            },
            "name":"Reader",
            "category":"reader"
        },
        { //下面是关于Writer的模板，可以找相应的写插件文档
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

## 补充说明 {#section_htd_3th_p2b .section}

**主备同步数据恢复问题**

主备同步问题指DB2使用主从灾备，备库从主库不间断通过binlog恢复数据。由于主备数据同步存在一定的时间差，特别在于某些特定情况，例如网络延迟等问题，导致备库同步恢复的数据与主库有较大差别，导致从备库同步的数据不是一份当前时间的完整镜像。

**一致性约束**

DB2在数据存储划分中属于RDBMS系统，对外可以提供强一致性数据查询接口。例如当一次同步任务启动运行过程中，当该库存在其他数据写入方写入数据时，DB2 Reader完全不会获取到写入更新数据，这是由于数据库本身的快照特性决定的。数据库快照的特性请参见[MVCC Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control) 。

上述是在DB2 Reader单线程模型下数据同步一致性的特性，由于DB2 Reader可以根据您配置信息使用了并发数据抽取，因此不能严格保证数据一致性。当DB2 Reader根据splitPk进行数据切分后，会先后启动多个并发任务完成数据同步。由于多个并发任务相互之间不属于同一个读事务，同时多个并发任务存在时间间隔，因此这份数据并不是完整的、一致的数据快照信息。

针对多线程的一致性快照需求，目前在技术上无法实现，只能从工程角度解决。工程化的方式存在取舍，在此提供以下解决思路，您可根据自身情况进行选择。

-   使用单线程同步，即不再进行数据切片。缺点是速度比较慢，但是能够很好保证一致性。
-   关闭其他数据写入方，保证当前数据为静态数据，例如，锁表、关闭备库同步等。缺点是可能影响在线业务。

**数据库编码问题**

DB2 Reader底层使用JDBC进行数据抽取，JDBC天然适配各类编码，并在底层进行了编码转换。因此DB2 Reader不需您指定编码，可以自动识别编码并转码。

**增量数据同步**

DB2 Reader使用JDBC SELECT语句完成数据抽取工作，因此可以使用SELECT…WHERE…进行增量数据抽取，有以下几种方式。

-   数据库在线应用写入数据库时，填充modify字段为更改时间戳，包括新增、更新、删除（逻辑删除）。对于这类应用，DB2 Reader只需要WHERE条件后跟上一同步阶段时间戳即可。
-   对于新增流水型数据，DB2 Reader在WHERE条件后跟上一阶段最大自增ID即可。

对于业务上无字段区分新增、修改数据的情况，DB2 Reader也无法进行增量数据同步，只能同步全量数据 。

**SQL安全性**

DB2 Reader提供querySql语句交给您自己实现SELECT抽取语句，DB2 Reader本身对querySql不做任何安全性校验。

