# Configure RDBMS Reader {#concept_otk_h4k_q2b .concept}

This topic describes the data types and parameters supported by RDBMS Reader and how to configure Reader in script mode.

The RDBMS Reader plug-in allows you to read data from RDBMS \(distributed RDS\). At the underlying implementation level, RDBMS Reader connects to a remote RDBMS database through JDBC and runs corresponding SQL statements to SELECT data from the RDBMS database. Currently, it supports reading data from databases including DM, DB2, PPAS, and Sybase. Currently, the RDBMS plug-in is only adapted to the MySQL engine. RDBMS is a distributed MySQL database, and most of the communication protocols are applicable to MySQL use cases.

Specifically, RDBMS Reader connects to a remote RDBMS database through the JDBC connector. The SELECT SQL query statements are generated and sent to the remote RDBMS database based on your configuration. Then, the SQL statements are run and the returned results are assembled into abstract datasets using the custom data types of data synchronization. Datasets are passed to the downstream writer for processing.

RDBMS Reader concatenates the table, column, and WHERE information you configured into SQL statements and sends them to the RDBMS database. For the querySQL information that you configure, the RDBMS sends it directly to the RDBMS database.

RDBMS Reader supports the most generic rational database types, such as numbers and characters. Check whether your data type is supported and select a reader based on a specific database.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|jdbcUrl|Information of the JDBC connection to the opposite-end database. The format of jdbcUrl is in accordance with the RDBMS official specification, and the URL attachment control information can be entered. Note that JDBC formats vary with databases and DataX selects an appropriate database driver for data reading based on a specific JDBC format.-   DM: jdbc:dm://ip:port/database
-   DB2 jdbc:db2://ip:port/database
-   PPAS jdbc:edb://ip:port/database

RDBMS Writer adds new database support in the following ways.

-   Enter the corresponding directory of RDBMSWriter. $\{DATAX\_HOME\} is the main directory of DataX, that is, $\{DATAX\_HOME\}/plugin/writer/rdbmswriter.
-   Under the RDBMS Reader directory, you can find the plugin.json configuration file. Use this file to register your specific database driver, which is placed in the drivers array. The RDBMS Reader plug-in dynamically selects the appropriate database driver to connect to the database when executing the job.

```
{
    "name": "RDBMS Reader ",
    "class": "com.alibaba.datax.plugin.reader.RDBMS Reader.RDBMS Reader",
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
The RDBMS Reader directory contains the libs sub-directory, under which you need to put your specific database driver.
```
$tree
.
|-- libs
| |-- Dm7JdbcDriver16.jar
| |-- commons-collections-3.0.jar
| |-- commons-io-2.4.jar
| |-- commons-lang3-3.3.2.jar
| |-- commons-math3-3.1.1.jar
| |-- datax-common-0.0.1-SNAPSHOT.jar
| |-- datax-service-face-1.0.23-20160120.024328-1.jar
| |-- db2jcc4.jar
| |-- druid-1.0.15.jar
| |-- edb-jdbc16.jar
| |-- fastjson-1.1.46.sec01.jar
| |-- guava-r05.jar
| |-- hamcrest-core-1.3.jar
| |-- jconn3-1.0.0-SNAPSHOT.jar
| |-- logback-classic-1.0.13.jar
| |-- logback-core-1.0.13.jar
| |-- plugin-rdbms-util-0.0.1-SNAPSHOT.jar
| `-- slf4j-api-1.7.10.jar
|-- plugin.json
|-- plugin_job_template.json
`-- RDBMS Reader-0.0.1-SNAPSHOT.jar
```

|Yes|N/A|
|username|The User Name of the data source.|Yes|N/A|
|password |The password corresponding to the specified username for the data source.|Yes|N/A|
|table.|The selected table that needs to be synchronized.|Yes|N/A|
|column|The configured table requires a collection of column names that are synchronized, using a JSON array to describe the field information, all column configurations, such as \[\*\], are used by default.-   Column pruning is supported, which means you can select some columns to export.
-   Change of column order is supported, which means you can export the columns in an order different from the schema order of the table.
-   Constant configuration is supported, and you need to follow the JSON format `["id","1", "'bazhen.csy'", "null", "to_char(a + 1)", "2.3" , "true"]`.
    -   ID is normal column name
    -   1 For plastic digital Constants
    -   'Bazarn. CSY 'is a String constant
    -   Null is a null pointer
    -   To\_char \(a + 1\) is a function expression
    -   2.3 is a floating point number
    -   True is a Boolean Value
-   Column must contain the specified column set to be synchronized and it cannot be blank.

|Yes|N/A|
|  splitPk| If you specify the splitPk when using RDBMS Reader to extract data, it means that you want to use the fields represented by splitPk for data sharding. Then, the DataX starts concurrent tasks to synchronize data, which greatly improves data synchronization efficiency.

-   If you are using splitPk, we recommend that you use the tables primary keys because the primary keys are generally even and data hot spots are less prone to split data fragments.
-   Currently, splitPk only supports data sharding for integer data types. Other types such as floating point, string, and date are not supported. If you specify an unsupported data type, DB2 Reader reports an error.
-   If you do not fill in splitPk, you will be treated as if you do not split the single table, RDBMS reader uses a single channel to synchronize full data.

 |No |Blank|
|where|The filtering condition. RDBMS Reader concatenates an SQL command based on specified column, table, and WHERE statements and extracts data according to the SQL. For example, you can specify the WHERE statement as limit 10 during a test. In actual business scenarios, the data on the current day is usually required for synchronization. You can specify the WHERE statement as gmt\_create \> $bizdate.-   The WHERE statement can be effectively used for incremental synchronization.
-   If the WHERE statement is not set or is left null, full table data synchronization is applied.

|No |N/A|
|querySql|In some business scenarios, the WHERE statement is insufficient for filtering. In such cases, the user can customize a filter SQL using this configuration item. When you configure this, the data synchronization system ignores the Table, column, and so on, filter the data directly using the contents of this configuration item.For example, you need to synchronize data after a multi-table join, using `select a,b from table_a join table_b on table_a.id = table_b.id`. When querySQL is configured, RDBMS Reader directly ignores the configuration of table, column, and WHERE statements.

|No|N/A|
| fetchSize|It defines the pieces of batch data that the plug-in and database server can fetch each time. The value determines the number of network interactions between the DataX system and the server, which can greatly improve data extraction performance.**Note:** The fetchsize value \(\> 2048\) may cause the data synchronization process Out of Memory \(O\).

|No|1,024|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Development in wizard mode is not supported currently.

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to synchronously extract data from an RDBMS database:

```language-json
{
    "order": {
        "hops": [
            {
                "from": "Reader",
                "to": "Writer"
            }
        ]
    },
    "setting": {
        "errorLimit": {
            "record": "0"
        },
        "speed": {
            "concurrent": 1,
            "dmu": 1,
            "throttle": false
        }
    },
    "steps": [
        {
            "category": "reader",
            "name": "Reader",
            "parameter": {
                "column": [
                    {
                        "type": "string",
                        "value": "field"
                    },
                    {
                        "type": "long",
                        "value": 100
                    },
                    {
                        "dateFormat": "yyyy-MM-dd HH:mm:ss",
                        "type": "date",
                        "value": "2014-12-12 12:12:12"
                    },
                    {
                        "type": "bool",
                        "value": true
                    },
                    {
                        "type": "bytes",
                        "value": "byte string"
                    }
                ],
                "sliceRecordCount": "10"
            },
            "stepType": "stream"
        },
        {
            "category": "writer",
            "name": "Writer",
            "parameter": {
                "connection": [
                    {
                        "jdbcUrl": "jdbc:dm://ip:port/database",
                        "table": [
                            "table"
                        ]
                    }
                ],
                "username": "username",
                "password": "password",
                "table": "table",
                "column": [
                    "*"
                ],
                "preSql": [
                    "delete from XXX;"
                ]
            },
            "stepType": "rdbms"
        }
    ],
    "type": "job",
    "version": "2.0"
}
```

