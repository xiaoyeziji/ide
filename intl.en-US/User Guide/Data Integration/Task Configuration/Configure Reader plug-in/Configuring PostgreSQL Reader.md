# Configuring PostgreSQL Reader {#concept_axy_tmq_p2b .concept}

The PostgreSQL Reader plug-in reads data from PostgreSQL databases. At the underlying implementation level, PostgreSQL Reader connects to a remote PostgreSQL database through JDBC and runs SELECT statements to extract data from the database. On the public cloud, RDS provides a PostgreSQL storage engine.

Specifically, PostgreSQL Reader connects to a remote PostgreSQL database through the JDBC connector. The SELECT SQL query statements are generated and sent to the remote PostgreSQL database based on your configuration. Then, the SQL statements are run and the returned results are assembled into abstract datasets using the custom data types of data integration. Datasets are passed to the downstream writer for processing.

-   PostgreSQL Reader concatenates the table, column, and WHERE information you configured into SQL statements, and sends them to the PostgreSQL database.
-   PostgreSQL directly sends the configured querySQL information to the PostgreSQL database.

## Type conversion list {#section_vcg_fvn_q2b .section}

PostgreSQL Reader supports most data types in PostgreSQL. Check whether your data type is supported.

The PostgreSQL reader has a list of Type transformations for PostgreSQL, as shown below.

|Category|PostgreSQL data type|
|:-------|:-------------------|
|Integer|bigint, bigserial, integer, smallint, and serial|
|Floating point|double precision, money, numeric, and real|
|String|varchar, char, text, bit, and inet|
|Date and time|date, time, and timestamp|
|Boolean|bool|
|Binary|bytea|

**Note:** 

-   Except the preceding field types, other types are not supported.
-   For "money", "inet", and "bit", you need to use syntaxes such as "a\_inet::varchar" to convert data types.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|table.|The column name set to be synchronized in the configured table.|Yes|N/A|
|column|Field information is described with arrays in JSON. \[ \* \] indicates all columns by default.-   Column pruning is supported, which means you can select some columns to export.
-   Change of column order is supported, which means you can export the columns in an order different from the schema order of the table.
-   Constant configuration is supported. You must follow the MySQL SQL syntax format, for example `[["id", "table","1", "'mingya.wmy'", "'null'", "to_char(a+1)", "2.3" , "true"]`.
    -   ID is normal column name
    -   Table is a column name that contains Reserved Words
    -   1 For plastic digital Constants
    -   'mingya.wmy' is a String constant \(note that a pair of single quotes is required\)
    -   Null is a null pointer
    -   Char\_length \(s\) is the computed String Length Function
    -   2.3 is a floating point number
    -   True is a Boolean Value
-   Column must contain the specified column set to be synchronized and it cannot be blank.

|Yes|N/A|
|Splitpk|- Split key: If you specify the splitPk when using PostgreSQLReader to extract data, it means that you want to use the fields represented by the splitPk for data sharding. In this case, the Data Integration initiates concurrent jobs to synchronize data, which greatly improves the efficiency of data synchronization.-   If you are using splitPk, we recommend that you use the primary keys of tables, because the primary keys are generally even and data hot spots are less prone to split data fragments.
-   Currently, splitPk only supports data sharding for integer data types. Other types such as string, floating point, and date are not supported. If you specify an unsupported data type, the splitPk is ignored and the data is synchronized using a single channel.
-   If the splitPk is not specified \(for example, splitPk is not provided or splitPk value is null\), the table data is synchronized using a single channel.

|No|N/A|
|where|PostgreSQLReader concatenates an SQL statement based on the specified column, table, and WHERE conditions and extracts data according to the SQL statement. For example, you can set the WHERE condition during a test. In actual service scenarios, the data on the current day are usually required to be synchronized, in which case you can set the WHERE condition as id \> 2 and sex = 1.-   The where condition can be effectively used for incremental synchronization.
-   If the where condition is not set or is left null, full table data synchronization is applied.

|No|N/A|
|querySQL \(advanced mode, wizard mode not available\)|In some business scenarios, the where condition is insufficient for filtration. In such cases, the user can customize a filter SQL using this configuration item. When this item is configured, the data synchronization system filters data using this configuration item directly instead of such configuration items as tables, columns, and splitPk. For example, for data synchronization after multi-table join, use `select a,b from table_a join table_b on table_a.id = table_b.id`. When querySQL is configured, PostgreSql Reader directly ignores the configuration of table, column, and WHERE conditions.|No|N/A|
|Fetchsize|Description: It defines the pieces of batch data that the plug-in and database server can fetch each time. The value determines the number of network interactions between the DataX system and the server, which can greatly improve data extraction performance.**Note:** The fetchsize value \(\> 2048\) may cause the data synchronization process oom.

|No|512 MB|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the source and destination of the data for the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16231/15367218587845_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Table: table in the preceding parameter description. Select the table to be synchronized.
    -   Data Filtering: you are about to synchronize the filtering criteria for the data, and the limit keyword filtering is not supported for the time being. The SQL syntax is consistent with the selected data source.
    -   Cut key: You can use a column in the source data table as a cut key, it is recommended that you use a primary key or an indexed column as a split key, and that only fields of type Integer be supported.

        During data reading, the data is split based on the configured fields to achieve concurrent reading, improving data synchronization efficiency.

        **Note:** The configuration of splitting key is related to the source selection in data synchronization. The splitting key configuration item is displayed only when you configure the data source.

2.  The field mapping, which is the column in the above parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16231/15367218587846_en-US.png)

    -   Peer mapping: Click **Enable Same-Line Mapping** to establish a corresponding mapping relationship in the peer, note that match the data type.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit the fields. Each line indicates a field. The first and end blank lines are ignored.
    The function of adding a row is as follows:

    -   You can enter constants. The value must be enclosed by a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as Not identified.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15367218587675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group. For more information, see[Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to synchronously extract data from a PostgreSQL database.

```
{
    "type": "job",
    "version": "2.0"} //Indicates the version
    "steps":[
        {
            "stepType": "postgresql",/plug-in name
            "parameter": {
                "datasource": "", // Data Source
                "column": [// Field
                    "col1",
                    "col2"
                ],
                "where": "", //Filtering condition
                "splitPk": "",/using the fields represented by splitpk for Data Division, data Synchronization thus starts concurrent tasks for Data Synchronization
                "table": "// table name
            },
            "name": "Reader ",
            "category": "reader"
        },
        {//The following is a reader template. You can find corresponding writer plug-in documentations
            "stepType": "stream ",
            "parameter":{},
            "name": "Writer ",
            "category": "writer"
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

## Additional instructions- {#section_htd_3th_p2b .section}

**Master standby synchronous data recovery problem**

Master/slave synchronization means that PostgreSQL uses a master/slave disaster recovery mode in which the slave database continuously restores data from the master database through binlog. Due to the time difference in primary/backup data synchronization, especially in some special situations such as network latency, the restored data in the backup database after synchronization is significantly different from the data of the primary database, that is to say, the data synchronized from the backup database is not a full image of the primary database at the current time.

If the data integration system synchronizes data of the RDS provided by Alibaba Cloud, the data is directly read from the primary database without data restoration concerns. However, this may cause concerns on the master database load. Configure it properly for throttling.

**Consistency Constraints**

PostgreSQL is an RDBMS system in terms of data storage, which can provide APIs for querying strong consistency data. For example, if another data writer writes data to the database during a synchronization task, PostgreSQL Reader does not get the newly written data because of the snapshot features of the database. For the characteristics of database snapshots, see [MVCC Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control).

Above are the characteristics of data synchronization consistency under the PostgreSQL reader single-threaded model, since PostgreSQL reader can use Concurrent Data Extraction Based on your configuration information, therefore, data consistency cannot be strictly guaranteed. When the PostgreSQL reader is split Based on the splitpk data, multiple concurrent tasks are initiated to complete the synchronization of data. Since multiple concurrent tasks do not belong to the same read transaction with each other, there are time intervals for multiple concurrent tasks at the same time, therefore, this data is not a complete, consistent snapshot of the data.

For the need of multithreaded consistent snapshot, it can not be realized technically at present, but can only be solved from the perspective of engineering. The method of engineering exists, and the following solutions are provided here, and you can choose according to your own circumstances.

-   Use single-threaded synchronization without data sharding. This is slow but can ensure robust data consistency.
-   Close other data writers to ensure the current data is static. For example, you can lock the table or close slave database synchronization. The disadvantage is that the online businesses may be affected.

**Database coding problem**

PostgreSQL supports EUC\_CN and UTF-8 encoding for simplified Chinese. PostgreSQL Reader extracts data using JDBC at the underlying level. JDBC is applicable to all types of encodings and can complete the transcoding at the underlying level. Therefore, PostgreSQL Reader can acquire the encoding and complete transcoding automatically without the need to specify the encoding.

PostgreSQL Reader cannot identify the inconsistency between the encoding written to the underlying layer of PostgreSQL and the configured encoding, nor provide a solution. Due to this issue, the exported codes may contain garbage codes.

**Incremental Synchronization**

PostgreSQL reader uses a jdbc select statement for data extraction, so you can use select... Where... in either of the following ways:

-   When online database applications write data into the database, the modify field is filled with the modification timestamp, including addition, update, and deletion \(logical deletion\). For this type of applications, PostgreSQL Reader only requires the WHERE condition followed by the timestamp of the last synchronization phase.
-   For new streamline data, PostgreSQL Reader requires the WHERE condition followed by the maximum auto-increment ID of the last synchronization phase.

In case that no field is provided for the business to identify the addition or modification of data, PostgreSQL Reader cannot perform incremental data synchronization and can only perform full data synchronization.

**SQL Security**

PostgreSQL Reader provides querySQL statements for you to SELECT data by yourself. PostgreSQL Reader conducts no security verification on querySQL.

