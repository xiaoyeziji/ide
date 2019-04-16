# Configuring PostgreSQL Reader {#concept_axy_tmq_p2b .concept}

In this topic we will describe the data types and parameters supported by PostgreSQL Reader and how to configure the Reader in both wizard and script mode.

The PostgreSQL Reader plug-in reads data from PostgreSQL databases. At the underlying implementation level, the PostgreSQL Reader connects to a remote PostgreSQL database through JDBC and runs SELECT statements to extract data from the database. On the public cloud, RDS provides a PostgreSQL storage engine.

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
-   For "money", "inet", and "bit", you need to use syntaxes, such as "a\_inet::varchar" to convert data types.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|datasource|The data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|table.|The column name set to be synchronized in the configured table.|Yes|N/A|
|column|Field information is described with JSON arrays. \[ \* \] indicates all columns by default.-   Column pruning is supported which means you can select some columns to export.
-   Change of column order is supported, which means you can export the columns in an order different from the table schema order.
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
|SplitPk|- If you specify the SplitPk when using PostgreSQLReader to extract data, it means that you want to use the fields represented by the SplitPk for data sharding. In this case, the Data Integration initiates concurrent jobs to synchronize data, which greatly improves the data synchronization efficiency.-   If you are using SplitPk, we recommend that you use the tables primary keys because the primary keys are generally even and data hot spots are less prone to split data fragments.
-   Currently, SplitPk only supports data sharding for integer data types. Other types such as string, floating point, and date are not supported. If you specify an unsupported data type, the SplitPk is ignored and the data is synchronized using a single channel.
-   If the SplitPk is not specified, the table data is synchronized using a single channel,for example, SplitPk is not provided or SplitPk value is null.

|No|N/A|
|where|PostgreSQLReader concatenates an SQL statement based on the specified column, table, and WHERE statement and extracts data, according to the SQL statement. For example, you can set the WHERE statement during a test. In actual service scenarios, the data on the current day are usually required to be synchronized, in which case you can set the WHERE statement as id \> 2 and sex = 1.-   The WHERE statement can be effectively used for incremental synchronization.
-   If the WHERE statement is not set or is left null, the full table data synchronization is applied.

|No|N/A|
|querySQL \(only available in advanced mode\)|In business scenarios, where the WHERE statement is insufficient for filtering. In such cases, the user can customize a filter SQL using this configuration item. When this item is configured, the data synchronization system filters data using this configuration item directly instead of configuration items as tables, columns, and SplitPk. For example, for data synchronization after multi-table join, use `select a,b from table_a join table_b on table_a.id = table_b.id`. When querySQL is configured, PostgreSQL Reader directly ignores the configuration of table, column, and WHERE conditions.|No|N/A|
|Fetchsize|It defines batch data pieces that the plug-in and database server can fetch each time. The value determines the number of network interactions between the DataX system and the server, which can greatly improve data extraction performance.**Note:** The fetchsize value \(\> 2048\) may cause the data synchronization process Out of Memory \(OOM\).

|No|512 MB|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the source and destination of the data for the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16231/15515085347845_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Table: Table in the preceding parameter description. Select the table for synchronization.
    -   Data filtering: You are about to synchronize the filtering criteria for data, and limit keyword filtering is not supported for the time being. The SQL syntax is consistent with the selected data source.
    -   Shard key: You can use a column in the source data table as a shard key. It is recommended that you use a primary key or an indexed column as a shard key, and that only fields of type Integer are supported.

        During data reading, the data split is based on configured fields to achieve concurrent reading, and improving data synchronization efficiency.

        **Note:** The shard key configuration is related to the source selection in data synchronization. The shard key configuration item is displayed only when you configure the data source.

2.  The field mapping which is the column in the above parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16231/15515085347846_en-US.png)

    -   Peer mapping: Click **Enable Same-Line Mapping** to establish a corresponding mapping relationship in the peer, note that match the data type.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit the fields where each line indicates a field. The first and end blank lines are ignored.
    The function of adding a row is as follows:

    -   You can enter constants. The value must be enclosed by a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as unidentified.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15515085347675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources, including CPU, memory, and network bandwidth consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: The maximum number of threads used to concurrently read or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task resource group: The machine on which the task runs, if the task number is large, the default Resource Group is used to wait for a resource. It is recommended that you add a Custom Resource Group. For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

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

## Additional instructions {#section_htd_3th_p2b .section}

**Active/standby synchronous data recovery problem**

Active/standby synchronization means that PostgreSQL uses an active/standby disaster recovery mode in which the standby database continuously restores data from the active database through binlog. Because of time differences in the active/standby data synchronization, especially in some situations, such as network latency. The restored data in the standby database after synchronization is significantly different from the primary database data, that is to say, the data synchronized from the backup database is not a full image of the primary database of the current time.

If the data integration system synchronizes RDS data provided by Alibaba Cloud, the data can be directly read from the primary database without data restoration issues. However, this may cause issues on the master database load. Configure the data integration system properly for throttling.

**Consistency constraints**

PostgreSQL is an RDBMS data storage system, which can provide APIs for querying strong consistency data. For example, if another data writer writes data to the database during a synchronization task, PostgreSQL Reader does not obtain the newly written data because of the database snapshot features. For database snapshot characteristics, see [MVCC Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control).

The preceding paragraph lists all characteristics of data synchronization consistency under the PostgreSQL reader single-threaded model. Because PostgreSQL reader can use Concurrent Data Extraction based on your configuration information, therefore, data consistency cannot be strictly guaranteed. When the PostgreSQL reader is split based on the splitPk data, multiple concurrent tasks are initiated to complete the data synchronization. Since multiple concurrent tasks do not belong to the same read transaction, there are time intervals for multiple concurrent tasks at the same time, therefore, this data is an incomplete and inconsistent data snapshot .

Multi-threaded consistent snapshot requirements can only be solved from an engineering perspective. The following are engineering methods and solutions for different application scenarios.

-   Single-threaded synchronization without data sharding. This method is slow but can ensure robust data consistency.
-   Close other data writers to ensure the current data is static. For example, you can lock the table or close standby database synchronization. The disadvantage with this method is it may affect online businesses .

**Database coding problem**

PostgreSQL supports EUC\_CN and UTF-8 encoding for simplified Chinese. PostgreSQL Reader extracts data using JDBC at the underlying level. JDBC is applicable for all types of encodings and can complete the transcoding at the underlying level. Therefore, PostgreSQL Reader can acquire the encoding and complete transcoding automatically without the need to specify the encoding.

PostgreSQL Reader cannot identify the inconsistency between the encoding written to the underlying layer of PostgreSQL and the configured encoding, nor provides a solution. Due to this issue, the exported codes may contain junk codes.

**Incremental synchronization**

PostgreSQL Reader uses a JDBC select statement for data extraction, so you can use select... WHERE... in either of the following ways:

-   When online database applications write data into the database, the modify field is filled with the modification timestamp, including addition, update, and deletion \(logical deletion\). For this type of applications, PostgreSQL Reader only requires the WHERE statement followed by the timestamp of the last synchronization phase.
-   For new streamline data, PostgreSQL Reader requires the WHERE statement followed by the maximum auto-increment ID of the last synchronization phase.

In the case that no fields are provided for the business to identify the addition or modification of data, PostgreSQL Reader cannot perform incremental data synchronization, and can only perform full data synchronization.

**SQL Security**

PostgreSQL Reader provides querySQL statements for you to SELECT data. PostgreSQL Reader does not perform security verification on querySQL.

