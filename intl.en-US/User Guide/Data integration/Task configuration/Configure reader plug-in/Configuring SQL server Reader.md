# Configuring SQL server Reader {#concept_bgv_5qq_p2b .concept}

This topic describes data types and parameters supported by the SQL server Reader and how to configure Reader in both wizard mode and script mode.

The SQL Server Reader plug-in provides the ability to read data from the SQL Server. At the underlying implementation level, the SQL Server Reader connects to a remote SQL Server database through JDBC and runs SELECT statements to extract data from the database.

Specifically, the SQL Server Reader connects to a remote SQL Server database through the JDBC connector. The SELECT SQL query statements are generated and sent to the remote SQL Server database based on your configuration. Then, the SQL statements are runned and the returned results are assembled into abstract datasets using the custom data types of data integration. Datasets are passed to the downstream writer for processing.

-   SQL Server Reader concatenates the table, column, and the WHERE information you configured into SQL statements and sends them to the SQL Server database.
-   SQL Server directly sends the querySQL information you configured to the SQL Server database.

SQL Server Reader supports most data types in SQL Server. Check whether your data type is supported.

SQL Server Reader converts SQL Server data types as follows:

|Category|SQL server data type|
|:-------|:-------------------|
|Integer|bigint, int, smallint, and tinyint|
|Float|float, decimal, real, and numeric|
|String type|char, nchar, ntext, nvarchar, text, varchar, nvarchar \(MAX\), and varchar \(MAX\)|
|Date and time type|date, datetime, and time|
|Boolean|bit|
|Binary, varbinary, varbinary \(MAX\), and timestamp|Binary, varbinary, varbinary \(max\), and timestamp|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|table.|The table selected for synchronization. One job can only synchronize one table.|Yes|N/A|
|column|The column name set to be synchronized in the configured table. Field information is described with JSON arrays. \[""\] indicates all columns by default.-   Column pruning is supported, which means you can select some columns to export.
-   Change column order is supported, which means you can export the columns in an order different from the table schema order.
-   Constant configuration is supported. You must follow the MySQL SQL syntax format, for example `["id", "table", "1", "'mingya.wmy'", "'null'", "to_char(a + 1)", "2.3" , "true"]` .
    -   ID is normal column name
    -   Table is a column name that contains Reserved Words
    -   1 For plastic digital Constants
    -   'mingya.wmy' is a String constant \(note that a pair of single quotes is required\)
    -   null refers to the null pointer
    -   to\_char\(a + 1\) is a function expression
    -   2.3 is a floating point number
    -   true is a Boolean value
-   Column must contain the specified column set to be synchronized and it cannot be blank.

|Yes|N/A|
|splitPk|If you specify the splitPk when using SQL Server Reader to extract data, it means the fields are represented by splitPk for data sharding. Then, the data synchronization system starts concurrent tasks to synchronize data, which greatly improves the data synchronization efficiency. -   We recommend that splitPk users use the tables primary keys because the primary keys are generally even and the data hot spots are less prone to split data fragments.
-   Currently, splitPk only supports data sharding for integer data types. Other types such as float point, string, and date are not supported. If you specify an unsupported data type, SQL Server Reader reports an error.

|No|N/A|
|where|The filtering condition. The SQL Server Reader concatenates an SQL command based on the specified column, table, and WHERE statement and extracts data according to the SQL command. For example, you can specify the WHERE statement as limit 10 during a test. In actual business scenarios, the data on the current day is usually required to be synchronized. You can specify the WHERE statement as gmt\_create \> $bizdate.-   The WHERE statement can be effectively used for incremental synchronization.
-   The WHERE statement can be effectively used for incremental synchronization. If the value is null, it means synchronizing all the information in the table.

|No|N/A|
|querySQL|In some business scenarios, the WHERE statement is insufficient for filtering. In such cases, you can customize a filter SQL statement using this configuration item. When this item is configured, the data synchronization system filters data using this configuration item directly instead of configuration items, such as table and column. For example, for data synchronization after multi-table join, use `select a,b from table_a join table_b on table_a.id = table_b.id`. When querySQL is configured, SQL Server Reader directly ignores the configuration of table, column, and WHERE statements.|No|N/A|
|fetchSize|It defines the pieces of batch data that the plug-in and database server can fetch each time. The value determines the number of network interactions between the data synchronization system and the server, which can greatly improve data extraction performance.**Note:** A value greater than 2048 may lead to Out of Memory \(OOM\) for data synchronization.

|No|1,024|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Data source and destination

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16232/15515085737861_en-US.png)

    Configurations:

    -   Data source: The data source in the preceding parameter description. Enter the data source name you configured.
    -   Table: The table in the preceding parameter description. Select the table for synchronization.
    -   Filtering condition: You should synchronize the data filtering conditions. Limit keyword filter is not supported yet. SQL syntaxes vary with data sources.
    -   Shard key: You can use a column in the source table as the shard key. It is recommended to use a primary key or an indexed column as the shard key.
2.  Field mapping: The column in the preceding parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16232/15515085737862_en-US.png)

    -   Peer mapping: Click **Enable Same-Line Mapping** to establish a corresponding mapping relationship in the peer that matches the data type.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit the fields where each line indicates a field. The first and end blank lines are ignored.
    The function of adding a row is as follows:

    -   You can enter constants. Each constant must be enclosed in a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   Enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as 'Not Identified'.
3.  Channel control

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15515085737675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources consumed during data integration, including CPU, memory, and network bandwidth. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent count: The maximum number of threads used to concurrently read or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.-
    -   Task resource group: The machine on which the task runs. If the number of tasks is large, the default Resource Group is used to wait for a resource. We recommend that you add a Custom Resource Group. For more information, see [Add scheduling resources](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to synchronously extract data from an SQL Server database:

```
{
    "type": "job",
    "version": "2.0"} //Indicates the version.
    "steps":[
        {
            "stepType": "SQL Server", // plug-in name
            "parameter": {
                "datasource": "", // Data Source
                "column": [// column name
                    "id",
                    "name"
                ],
                "where": "", //Filtering condition
                "splitPk": "", // If split PK is specified, indicates that you want to slice the data using the fields represented by splitpk
                "table": "// Data Sheet
            },
            "name": "Reader ",
            "category": "Reader"
        },
        {//The following is a writer template. You can find the corresponding writer plug-in documentations.
            "stepType": "stream ",
            "parameter":{}
            "name": "writer",
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
            "dmu": 1 // DMU Value
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

Active/standby synchronization means the SQL Server uses a active/standby disaster recovery mode in which the standby database continuously restores data from the master database through binlog. Due to the time difference in the primary/backup data synchronization, especially in situations such as network latency, the restored data in the backup database after synchronization is significantly different from the primary database data, that is to say, the data synchronized from the backup database is not a full image of the primary database at the current time.

If the data integration system synchronizes RDS data provided by Alibaba Cloud, the data is directly read from the primary database without data restoration concerns. However, this may cause concerns on the master database load and configure it properly for throttling.

**Consistency constraints**

SQL Server is an RDBMS system in terms of data storage, which can provide APIs for querying strong consistency data. For example, if another data writer writes data to the database during a synchronization task, SQL Server Reader does not obtain the newly written data because of the database snapshot features. For more information on the database snapshot features, refer to the [MVCC Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control).

The preceding paragraph lists the characteristics of data synchronization consistency under the SQL Server reader single-threaded model. Data consistency cannot be guaranteed because the SQL Server reader can use Concurrent Data Extraction based on your configuration information. When the SQL Server reader is split based on the splitPk data, multiple concurrent tasks are initiated to complete the synchronization of data. Since multiple concurrent tasks do not belong to the same read transaction , and time intervals for multiple concurrent tasks exist at the same time, therefore, this data is an incomplete and inconsistent snapshot of the data.

Multi-threaded consistent snapshot can only be solved from an engineering perspective. The following are engineering method and solutions for different application scenarios.

-   - Use single-threaded synchronization without data sharding. This is slow but can ensure robust data consistency.
-   - Close other data writers to ensure the current data is static. For example, you can lock the table or close the standby database synchronization. However, the disadvantage is online businesses may be affected.

**Database coding problem**

The SQL Server Reader extracts data using JDBC at the underlying level. JDBC is applicable to all types of encodings and can complete transcoding at the underlying level. Therefore, SQL Server Reader can identify the encoding and complete transcoding automatically without the need to specify the encoding.

**Incremental synchronization**

SQL Server reader uses a JDBC SELECT statement for data extraction, so you can use select... Where... in either of the following ways:

-   When online database applications write data into the database, the modify field is filled with the modification timestamp, including addition, update, and deletion \(logical deletion\). For this type of applications, SQL Server Reader only requires the WHERE statement followed by the timestamp of the last synchronization phase.
-   For new streamline data, SQL Server Reader requires the WHERE statement followed by the maximum auto-increment ID of the last synchronization phase.

In case no field is provided for the business to identify the addition or modification of data, SQL Server Reader cannot perform incremental data synchronization and can only perform full data synchronization.

**SQL security**

SQL Server Reader provides querySQL statements for you to SELECT data. The SQL Server Reader conducts no security verification on querySQL. The security during use is ensured by the data synchronization users.

