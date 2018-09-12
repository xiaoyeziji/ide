# Configure DB2 Reader {#concept_ydt_b4h_p2b .concept}

The DB2 Reader plug-in enables data reading from DB2. At the underlying implementation level, DB2 Reader connects to a remote DB2 database through JDBC and runs corresponding SQL statements to select data from the DB2 database.

Specifically, DB2 Reader connects to a remote DB2 database through the JDBC connector. The SELECT SQL query statements are generated and sent to the remote DB2 database based on your configuration. Then, the SQL statements are run and the returned results are assembled into abstract datasets using the custom data types of data integration. Datasets are passed to the downstream writer for processing.

-   DB2 Reader concatenates the table, column, and WHERE information you configured into SQL statements and sends them to the DB2 database.
-   DB2 Reader directly sends the query SQL information you configured to the DB2 database.

DB2 Reader supports most of the DB2 data types. Check whether your data type is supported.

DB2 Reader converts DB2 data types as follows:

|Type Classification|DB2 data type|
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
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|jdbcUrl|Information of the JDBC connection to the DB2 database. In accordance with the DB2 official specification, jdbcUrl in the DB2 format is jdbc:db2://ip:port/database, and the connection accessory control information can be filled in.|Yes|N/A|
|username|User name for the data source.|Yes|N/A|
|password|Description: Password corresponding to the specified username for the data source.|Yes|N/A|
|table.|You select a table that needs to be synchronized, and one operation can only support one table synchronization.|Yes|N/A|
|column|The configured table requires a collection of column names that are synchronized, using an array of JSON to describe the field information, all column configurations, such as \[\*\], are used by default.-   Column pruning is supported, which means you can select some columns to export.
-   Change of column order is supported, which means you can export the columns in an order different from the schema order of the table.
-   Constant configuration is supported. You must follow the DB2 SQL syntax format, for example,`["id", "1", "'const name'", "null", "upper('abc_lower')", "2.3" , "true"]`, 
    -   where id refers to the ordinary column name
    -   1 is an integer numeric constant
    -   'const name' is a String constant \(requires a pair of single quotes\)
    -   null is a null pointer
    -   upper \('abc \_ down'\) is a function expression
    -   2.3 is a floating point number
    -   True is a Boolean Value
-   Column must contain the specified column set to be synchronized and it cannot be blank.

|Yes|N/A|
|Splitpk|Description: If you specify the splitPk when using RDBMSReader to extract data, it means that you want to use the fields represented by splitPk for data sharding. Then, the DataX starts concurrent tasks to synchronize data, which greatly improves the efficiency of data synchronization.-   We recommend that the splitPk users use the primary keys of tables, because the primary keys are generally even and data hot spots are less prone to split data fragments.
-   Currently, splitPk only supports data sharding for integer data types. Other types such as floating point, string, and date are not supported. If you specify an unsupported data type, DB2 Reader reports an error.

|No|Blank|
|where|Filtering condition. DB2 Reader concatenates an SQL statement based on specified column, table, and where conditions and extracts data according to the SQL statement.  In actual business scenarios, the data on the current day is usually required to be synchronized. You can specify the where condition as gmt\_create \> $bizdate. The where condition can be used to synchronize incremental business data effectively. If the value is null, it means synchronizing all the information in the table.|No|N/A|
|Querysql|In some business scenarios, the where condition is insufficient for filtration. In such cases, you can customize a filter SQL statement using this configuration item.  When this item is configured, the data synchronization system filters data using this configuration item directly instead of such configuration items as table and column.For example, for data synchronization after multi-table join, use `select a,b from table_a join table_b on table_a.id = table_b.id`. When query SQL is configured, DB2 Reader directly ignores the configuration of table, column, and where conditions.

|No|N/A|
|Fetchsize| It defines the pieces of batch data that the plug-in and database server can fetch each time. The value determines the number of network interactions between the data synchronization system and the server, which can greatly improve data extraction performance.**Note:** A value greater than 2048 may lead to OOM for data synchronization.

|No|1,024|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Development in wizard mode is unavailable currently.

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

**Master standby synchronous data recovery problem**

Master/slave synchronization means that DB2 uses a master/slave disaster recovery mode in which the slave database continuously restores data from the master database through binlog. Due to the time difference in master/slave data synchronization, especially in some special situations such as network latency, the restored data in the slave database after synchronization are significantly different from the data of the master database, that is to say, the data synchronized in the slave database are not a full image of the master database at the current time.

**Consistency Constraints**

DB2 is a RDBMS system in terms of data storage, which can provide APIs for querying strong consistency data. For example, if another data writer writes data to the database during a synchronization task, DB2 Reader does not get the newly written data because of the snapshot features of the database. For the characteristics of database snapshots, see [MVCC Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control).

These are the features of data synchronization consistency in the single-threaded model of DB2 Reader. Because DB2 Reader uses concurrent data extraction according to your configuration information, robust data consistency cannot be guaranteed.  After DB2 Reader completes data sharding based on splitPk, multiple concurrent tasks are successively enabled to synchronize data. Since multiple concurrent tasks do not belong to the same read transaction and time intervals exist between the concurrent tasks, The data is not complete and consistent data snapshot information.

Currently, the consistency snapshot demands in the multi-thread model cannot be met technically, which can only be solved from the engineering point of view.  The engineering approaches have both advantages and disadvantages. The following solutions are provided for your consideration:

-   Use single-threaded synchronization without data sharding. This is slow but can ensure robust data consistency.
-   Close other data writers to ensure the current data is static. For example, you can lock the table or disable backup database synchronization. The disadvantage is that the online businesses may be affected.

**Database coding problem**

DB2 Reader extracts data using JDBC at the underlying level. JDBC is applicable to all types of encodings and can complete transcoding at the underlying level. Therefore, DB2 Reader can identify the encoding and complete transcoding automatically without the need to specify the encoding.

**Incremental Synchronization**

Since Oracle Reader extracts data using JDBC SELECT statements, you can extract incremental data using the SELECT...WHERE... in either of the following ways:

-   When online database applications write data into the database, the modify field is filled with the modification timestamp, including addition, update, and deletion \(logical deletion\). For this type of applications, DB2 Reader only requires the WHERE condition followed by the timestamp of the last synchronization phase.
-   For new streamline data, DB2 Reader requires the WHERE condition followed by the maximum auto-increment ID of the last synchronization phase.

In case that no field is provided for the business to identify the addition or modification of data, DB2 Reader cannot perform incremental data synchronization and can only perform full data synchronization.

**SQL Security**

DB2 Reader provides query SQL statements for you to SELECT data by yourself. DB2 Reader conducts no security verification on query SQL.

