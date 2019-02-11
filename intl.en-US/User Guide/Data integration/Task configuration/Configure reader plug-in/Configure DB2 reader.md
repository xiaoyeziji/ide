# Configure DB2 reader {#concept_ydt_b4h_p2b .concept}

The DB2 Reader plug-in enables data reading from DB2. At the underlying implementation level, the DB2 Reader connects to a remote DB2 database through JDBC and runs corresponding SQL statements to select data from the DB2 database.

Specifically, DB2 Reader connects to a remote DB2 database through the JDBC connector. The SELECT SQL query statements are generated and sent to the remote DB2 database based on your configurations. Then, the SQL statements are run and the returned results are assembled into abstract datasets using the custom data types of data integration. Datasets are passed to the downstream writer for processing.

-   DB2 Reader concatenates the configured table, column, and WHERE information into SQL statements and sends them to the DB2 database.
-   DB2 Reader directly sends configured query SQL information to the DB2 database.

DB2 Reader supports most DB2 data types. Check whether the data type is supported.

DB2 Reader converts DB2 data types as follows:

|Type classification|DB2 data type|
|:------------------|:------------|
|Integer|SMALLINT|
|Floating point|decimal, real, or double|
|String|char, character, varchar, graphic, vargraphic, long varchar, clob, long vargraphic, or dbclob|
|Date and time|Date, time, and timestamp|
|Boolean|—|
|Binary|blob|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. It must be identical to the added data source name. Adding data source is supported in script mode.|Yes|None|
|jdbcUrl|Information of the JDBC connection to the DB2 database. In accordance, with the DB2 official specification, jdbcUrl in the DB2 format is jdbc:db2://ip:port/database, and you can enter the connection accessory control information.|Yes|None|
|username|User name for the data source.|Yes|None|
|password|Password corresponding to the specified data source user name.|Yes|None|
|table|The table you select for synchronization. Each operation only supports one table synchronization.|Yes|None|
|column|The configured table requires a collection of column names synchronized with a JSON array to describe the field information. By default, all column configurations, such as \[\*\] are used.-   Column pruning is supported, which means you can select columns for export.
-   Changing column order is supported, which means the column export order can be different from the table schema order.
-   Constant configuration is supported. You must follow the DB2 SQL syntax format. For example:`["id", "1", "'const name'", "null", "upper('abc_lower')", "2.3" , "true"]`, 
    -   where id refers to the ordinary column name.
    -   1 is an integer numeric constant
    -   'const name' is a String constant \(requires a pair of single quotes\)
    -   null is a null pointer
    -   upper \('abc \_ down'\) is a function expression
    -   2.3 is a floating point number
    -   True is a Boolean Value
-   The column must contain the specified column set for synchronization and it cannot be blank.

|Yes|None|
|SplitPk|If you specify the SplitPk when using the RDBMSReader to extract data, it means fields represented by SplitPk are used for data sharding. Then DataX starts concurrent tasks to synchronize data, which greatly improves the data synchronization efficiency.-   We recommend you use the table primary keys for SplitPk because the primary keys are generally even and less likely to generate data hot spots during data sharding.
-   Currently, SplitPk only supports data sharding for integer data types. Other types such as floating point, string, and date are not supported. If you specify an unsupported data type, the DB2 Reader reports an error.

|No|Null|
|WHERE|A filtering condition. The DB2 Reader concatenates an SQL command based on the specified column, table, and WHERE clauses. It extracts data according to the SQL statement. In business scenarios, data from the current day are usually required for synchronization. You can specify the where condition as gmt\_create \> $bizdate. The WHERE clauses can be used to synchronize incremental business data effectively. If the value is null, it will synchronize all information in the table.|No|None|
|QuerySQL|In some business scenarios, the WHERE clause is insufficient for filtering. In this case, you can customize a filter SQL using QuerySQL. When QuerySQL is configured, the data synchronization system filters data with QuerySQL instead of other configuration items, such as tables and columns.For example, data synchronization after multi-table join, can use `select a,b from table_a join table_b on table_a.id = table_b.id`. When query SQL is configured, DB2 Reader ignores table, column, and WHERE clause configurations.

|No|None|
|Fetchsize|Defines the batch data pieces that the plug-in and database servers can fetch each time. The value determines the number of network interactions between the data synchronization system and the server, which greatly improves data extraction performance.**Note:** A value greater than 2048 may cause out-of-memory \(OOM\) during data synchronization.

|No|1,024|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Currently, development in wizard mode is unavailable.

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to synchronously extract data from a DB2 database:

```
{
    "type":"job",
    "version":"2.0", //Indicates the version.
    "steps":[
        {
            "stepType":"DB2", // plug-in name
            "parameter": {
                "password":"",//Password
                "jdbcUrl":"",//DB2 database's JDBC connection information
                "column":[
                    "id"
                ],
                "where": "", //Filtering condition
                "splitPk": "", //the field represented by/splitpk makes a data slice
                "table": "",//The name of the target table 
                "username": "//User Name
            },
            "name": "Reader ",
            "category": "reader"
        },
        {//The following is a writer template. You can find the corresponding writer plug-in documentations.
            "stepType":"stream",
            "parameter":{},
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit": {
            "record": "0"//Number of error records
        },
        "speed": {
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "concurrent": "1",//Number of concurrent tasks
            "dmu": 1//DMU Value
        }
    },
    "order":{
        "hops":[
            {
                "from": "Reader ",
                "to": "Writer"
            }
        ]
    }
}
```

## Additional instructions {#section_htd_3th_p2b .section}

**Active/standby synchronous data recovery problem**

Active/standby synchronization means that DB2 uses an active/standby disaster recovery mode in which the standby database continuously restores data from the active database through binlog. Because of time differences in active/standby data synchronization, especially in situations, such as network latency. The restored data in the standby database after synchronization are significantly different from the active database data. That is to say, the data synchronized in the standby database is not a full image of the current active database.

**Consistency limits**

In data storage, DB2 is a RDBMS system that can provide strong data consistency APIs for querying. For example, if another user writes data to the database during a synchronization task, DB2 Reader does not obtain the newly written data because of the database snapshot features. For the databases snapshot features, see [MVCC Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control).

The following are data synchronization consistency features in the single-threaded model of the DB2 Reader. Robust data consistency cannot be guaranteed because DB2 Reader uses concurrent data extraction based on configured information. After DB2 Reader completes data sharding based on SplitPk, multiple concurrent tasks are successively enabled to synchronize data. Because multiple concurrent tasks belong to different read transactions, time intervals exist between concurrent tasks. As a result, the data is incomplete and the data snapshot information is inconsistent.

Currently, consistency snapshot demands in multi-threaded model can only be solved from an engineering perspective. The engineering approaches has both advantages and disadvantages. The following are suggested solutions:

-   Use single-threaded synchronization without data sharding. This is slow but can ensure robust data consistency.
-   Disable other data writers to ensure the current data is static. For example, you can lock the table or disable standby database synchronization. Note: Disabling the data writer may affect your online business.

**Database encoding**

The DB2 Reader extracts data using JDBC at the underlying level. JDBC is applicable to all encoding types and can complete transcoding at the underlying level. Therefore, DB2 Reader can identify the encoding and automatically complete transcoding without specifying the encoding.

**Incremental synchronization**

Since Oracle Reader extracts data using JDBC SELECT statements, you can extract incremental data using SELECT...WHERE... statement in either of the following ways:

-   When online database applications write data into the database, the modify field enters the modification timestamp, including addition, update, and deletion \(logical deletion\). For this type of application, DB2 Reader only requires the WHERE condition followed by the timestamp of the last synchronization phase.
-   For new streamline data, DB2 Reader requires the WHERE statement followed by the maximum auto-increment ID of the last synchronization phase.

In the case that no fields are provided for the business to identify added or modified data, the DB2 Reader cannot perform incremental data synchronization and can only perform full data synchronization.

**SQL security**

The DB2 Reader provides query SQL statements for you to SELECT data. The DB2 Reader does not perform security verification on query SQL.

