# 配置RDBMS Reader {#concept_otk_h4k_q2b .concept}

本文为您介绍RDBMS Reader支持的数据类型、读取方式、字段映射和数据源等参数及配置举例。

RDBMS Reader插件实现了从RDBMS读取数据。在底层实现上，RDBMS Reader通过JDBC连接远程RDBMS数据库，并执行相应的SQL语句将数据从RDBMS库中SELECT出来。目前支持达梦、DB2、PPAS、Sybase数据库的读取。RDBMS Reader是一个通用的关系数据库读插件，您可以通过注册数据库驱动等方式增加任意多样的关系数据库读支持。

简而言之，RDBMS Reader通过JDBC连接器连接到远程的RDBMS数据库，并根据您配置的信息生成查询SQL语句并发送到远程RDBMS数据库，并将该SQL执行返回的结果，使用DataX自定义的数据类型拼装为抽象的数据集，并传递给下游Writer处理。

对于您配置的Table、Column、Where等信息，RDBMS Reader将其拼接为SQL语句发送到RDBMS数据库。对于您配置的querySql信息，RDBMS直接将其发送到RDBMS数据库。

目前RDBMS Reader支持大部分通用的关系数据库类型如数字、字符等，但也存在部分类型没有支持的情况，请注意检查您的类型，根据具体的数据库做选择。

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|jdbcUrl|描述的是到对端数据库的JDBC连接信息，jdbcUrl按照RDBMS官方规范，并可以填写连接附件控制信息。请注意不同的数据库JDBC的格式是不同的，DataX会根据具体JDBC的格式选择合适的数据库驱动完成数据读取。-   达梦格式：jdbc:dm://ip:port/database
-   DB2格式：jdbc:db2://ip:port/database
-   PPAS格式：jdbc:edb://ip:port/database

RDBMS Writer可通过以下方式增加新的数据库支持。

-   进入RDBMS Reader对应目录，$\{DATAX\_HOME\}为DataX主目录，即$\{DATAX\_HOME\}/plugin/reader/rdbmswriter。
-   在RDBMS Reader插件目录下有plugin.json配置文件，在此文件中注册您具体的数据库驱动，放在drivers数组中。RDBMS Reader插件在任务执行时会动态选择合适的数据库驱动连接数据库。

```
{
    "name": "rdbmsreader",
    "class": "com.alibaba.datax.plugin.reader.rdbmsreader.RdbmsReader",
    "description": "useScene: prod. mechanism: Jdbc connection using the database, execute select sql, retrieve data from the ResultSet. warn: The more you know about the database, the less problems you encounter.",
    "developer": "alibaba",
    "drivers": [
        "dm.jdbc.driver.DmDriver",
        "com.ibm.db2.jcc.DB2Driver",
        "com.sybase.jdbc3.jdbc.SybDriver",
        "com.edb.Driver"
    ]
}
```
- 在rdbmsreader插件目录下有libs子目录，您需要将您具体的数据库驱动放到libs目录下。
```
$tree
.
|-- libs
|   |-- Dm7JdbcDriver16.jar
|   |-- commons-collections-3.0.jar
|   |-- commons-io-2.4.jar
|   |-- commons-lang3-3.3.2.jar
|   |-- commons-math3-3.1.1.jar
|   |-- datax-common-0.0.1-SNAPSHOT.jar
|   |-- datax-service-face-1.0.23-20160120.024328-1.jar
|   |-- db2jcc4.jar
|   |-- druid-1.0.15.jar
|   |-- edb-jdbc16.jar
|   |-- fastjson-1.1.46.sec01.jar
|   |-- guava-r05.jar
|   |-- hamcrest-core-1.3.jar
|   |-- jconn3-1.0.0-SNAPSHOT.jar
|   |-- logback-classic-1.0.13.jar
|   |-- logback-core-1.0.13.jar
|   |-- plugin-rdbms-util-0.0.1-SNAPSHOT.jar
|   `-- slf4j-api-1.7.10.jar
|-- plugin.json
|-- plugin_job_template.json
`-- rdbmsreader-0.0.1-SNAPSHOT.jar
```

|是|无|
|username|数据源的用户名。|是|无|
|password|数据源指定用户名的密码。|是|无|
|table|所选取的需要同步的表。|是|无|
|column|所配置的表中需要同步的列名集合，使用JSON的数组描述字段信息，默认使用所有列配置，例如\[ \* \]。-   支持列裁剪，即列可以挑选部分列进行导出。
-   支持列换序，即列可以不按照表schema信息顺序进行导出。
-   支持常量配置，您需要按照JSON格式`["id","1", "'bazhen.csy'", "null", "to_char(a + 1)", "2.3" , "true"]`。
    -   id为普通列名
    -   1为整形数字常量
    -   'bazhen.csy'为字符串常量
    -   null为空指针
    -   to\_char\(a + 1\)为函数表达式
    -   2.3为浮点数
    -   true为布尔值
-   column必须显示您指定同步的列集合，不允许为空 。

|是|无|
|splitPk| RDBMS Reader进行数据抽取时，如果指定splitPk，表示您希望使用splitPk代表的字段进行数据分片，数据同步系统因此会启动并发任务进行数据同步，这样可以大大提供数据同步的效能。

-   推荐splitPk用户使用表主键，因为表主键通常情况下比较均匀，因此切分出来的分片也不容易出现数据热点。
-   目前splitPk仅支持整型数据切分，不支持浮点、字符串和日期等其他类型。如果您指定其他非支持类型，RDBMS Reader将报错。
-   如果不填写splitPk，将视作您不对单表进行切分，RDBMS Reader使用单通道同步全量数据。

 |否|空|
|where|筛选条件，RDBMS Reader根据指定的column、table、where条件拼接SQL，并根据这个SQL进行数据抽取。例如在做测试时，可以将where条件指定为limit 10。在实际业务场景中，往往会选择当天的数据进行同步，可以将where条件指定为gmt\_create\>$bizdate。-   where条件可以有效地进行业务增量同步。
-   where条件不配置或为空时，则视作全表同步数据。

|否|无|
|querySql|在部分业务场景中，where配置项不足以描述所筛选的条件，您可以通过该配置型来自定义筛选SQL。当您配置了这项后，数据同步系统就会忽略table，column等配置，直接使用这个配置项的内容对数据进行筛选。例如需要进行多表join后同步数据，使用`select a,b from table_a join table_b on table_a.id = table_b.id` 。当您配置querySql时，RDBMS Reader直接忽略table、column、where条件的配置。

|否|无|
|fetchSize|该配置项定义了插件和数据库服务器端每次批量数据获取条数，该值决定了数据同步系统和服务器端的网络交互次数，能够较大的提升数据抽取性能。**说明：** fetchSize值过大（\>2048）可能造成数据同步进程OOM。

|否|1024|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂时不支持向导开发模式。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从RDBMS数据库同步抽取数据作业。

```
{
    "job": {
        "setting": {
            "speed": {
                "byte": 1048576
            },
            "errorLimit": {
                "record": 0,
                "percentage": 0.02
            }
        },
        "content": [
            {
                "reader": {
                    "name": "rdbmsreader",
                    "parameter": {
                        "username": "xxx",
                        "password": "xxx",
                        "column": [
                            "id",
                            "name"
                        ],
                        "splitPk": "pk",
                        "connection": [
                            {
                                "table": [
                                    "table"
                                ],
                                "jdbcUrl": [
                                    "jdbc:dm://ip:port/database"
                                ]
                            }
                        ],
                        "fetchSize": 1024,
                        "where": "1 = 1"
                    }
                },
                "writer": {
                    "name": "streamwriter",
                    "parameter": {
                        "print": true
                    }
                }
            }
        ]
    }
}
```

配置一个自定义SQL的数据库同步任务到MaxCompute（原ODPS）的作业。

```
{
    "job": {
        "setting": {
            "speed": {
                "byte": 1048576
            },
            "errorLimit": {
                "record": 0,
                "percentage": 0.02
            }
        },
        "content": [
            {
                "reader": {
                    "name": "rdbmsreader",
                    "parameter": {
                        "username": "xxx",
                        "password": "xxx",
                        "column": [
                            "id",
                            "name"
                        ],
                        "splitPk": "pk",
                        "connection": [
                            {
                                "querySql": [
                                    "SELECT * from dual"
                                ],
                                "jdbcUrl": [
                                    "jdbc:dm://ip:port/database"
                                ]
                            }
                        ],
                        "fetchSize": 1024,
                        "where": "1 = 1"
                    }
                },
                "writer": {
                    "name": "streamwriter",
                    "parameter": {
                        "print": true
                    }
                }
            }
        ]
    }
}
```

