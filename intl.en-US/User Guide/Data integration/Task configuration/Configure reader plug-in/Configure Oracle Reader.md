# Configure Oracle Reader {#concept_t3p_vvp_p2b .concept}

This topic describes how to configure an Oracle Reader. The Oracle Reader plug-in provides the capability to read data from Oracle. At the underlying implementation level, Oracle Reader connects to a remote Oracle database through JDBC and runs SELECT statements to extract data from the database.

On the public cloud, RDS or DRDS does not provide the Oracle storage engine. Currently, Oracle Reader is mainly used for private cloud data migration and Data Integration projects.

In short, Oracle Reader connects to a remote Oracle database through the JDBC connector. The SELECT SQL query statements are generated and sent to the remote Oracle database based on your configuration. Then, the SQL statements are run and returned results are assembled into abstract datasets using the data synchronization custom data types. Datasets are passed to the downstream writer for processing.

-   Oracle Reader concatenates configured table, column, and WHERE information into SQL statements and sends them to the Oracle database.
-   Oracle sends the querySQL information you configured to the Oracle database.

## Type conversion list {#section_h4p_w2n_q2b .section}

Oracle Reader supports most data types in DB2. Check whether your data type is supported.

Oracle Reader converts Oracle data types as follows:

|Type classification|Oracle data type|
|:------------------|:---------------|
|Integer|Number, rawd, integer, Int, and smallint|
|Float| Numeric, decimal, float, double precision, real

 |
|String type|Long, Char, NChar, Varchar, Varchar2,NVar2, Clob, NClob, character, character varying, char varying, national character, National char, National Character varying, national char varying and nchar varying|
|Date and time type|Timestamp and Date|
|Boolean|Bit and Bool|
|Binary|Blob, BFile, Raw, and Long Raw|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|datasource|The data source name. It must be identical to the added data source name . Adding data source is supported in script mode.|Yes|N/A|
|table|The name of the selected table that needs to be synchronized.|Yes|N/A|
|column|The column name set to be synchronized in the configured table. Field information is described with JSON arrays. \[" \*\*\*"\] indicates all columns by default.-   Column pruning is supported, which means you can select export columns.
-   Change column order is supported, which means you can export columns in an order different from the table schema order.
-   Constant configuration is supported, and you need to configure in JSON format.

    ```
 ["id", "1", "'mingya.wmy'", "null", "to_char(a + 1)", "2.3" , "true"]
    ```

    -   ID is normal column name
    -   1 is an integer numeric constant
    -   'Mingya.wmy' is a String constant \(note that a pair of single quotes is required\)
    -   Null is a null pointer
    -   to\_char\(a + 1\) is an expression
    -   2.3 is a floating point number
    -   True is a Boolean Value
-   Column is required and cannot be blank.

|Yes|N/A|
|SplitPk|If you specify the SplitPk when using RDBMSReader to extract data, it means the fields are represented by SplitPk for data sharding. Then, the DataX starts concurrent tasks to synchronize data, which greatly improves t data synchronization efficiency.-   If you are using SplitPk, we recommend that you use table primary keys because the primary keys are generally even and less likely to generate data hot spots during data sharding.
-   The data types supported by SplitPk include the integer, string, floating point, and date.
-   If SplitPk is left blank, it indicates that no table sharding is required and Oracle Reader synchronizes full data through a single channel.

|No |N/A|
|WHERE|The filtering condition. Oracle Reader concatenates an SQL command based on specified column, table, and WHERE clauses and extracts data according to the SQL command. For example, you can set the WHERE clauses as row\_number\(\) during a test. In actual service scenarios, the incremental synchronization typically synchronizes data generated on the current day. You can specify the WHERE clauses as `id > 2 and sex = 1`. -   The WHERE clauses can be effectively used for incremental synchronization.
-   The WHERE clauses can be effectively used for incremental synchronization.

|No |N/A|
|querySQL\(only available in advanced mode\)|In some service scenarios, the WHERE clauses is insufficient for filtering. In such cases, you can customize a SQL filter using this parameter. When this item is configured, the data synchronization system filters data using this configuration item directly instead of configuration items, such as table and column. For example, data synchronization after multi-table join, uses `select a,b from table_a join table_b on table_a.id = table_b.id`. When querySQL is configured, Oracle Reader directly ignores the configuration of tables, columns, and WHERE clauses.|No |N/A|
|fetchSize|It defines the pieces of batch data that the plug-in and database server can fetch each time. The value determines the number of network interactions between the DataX system and the server, which can greatly improve data extraction performance.**Note:** The fetchsize value \(\> 2048\) may cause out of memory \(OOM\) during the data synchronization process .

|No|1,024|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the source and destination of the synchronization task data.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16228/15515083817790_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name configured.
    -   Table:The table in the preceding parameter description. Select the table for synchronization.
    -   Data filtering: You are about to synchronize the data filtering criteria, and limit keyword filtering is not supported for the time being. The SQL syntax is consistent with the selected data source.
    -   Shard key: You can use a column in the source data table as a shard key, it is recommended you use a primary key or an indexed column as a shard key, and that only Integer type fields are supported.

        The read data is sharded based on the configured fields to achieve concurrent reading, and improve data synchronization efficiency.

        **Note:** The shard key configuration is related to the source selection in data synchronization. The shard key configuration item is displayed only when you configure the data source.

2.  The field mapping is the column in the above parameter description.

    The source table field on the left and the target table field on the right are one-to-one correspondence, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16228/15515083817791_en-US.png)

    -   Peer mapping: Click **Enable Same-Line Mapping** to establish a corresponding mapping relationship in the peer, note the data type match.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit fields, where each line indicates a field. The first and end blank lines are ignored.
    The function of adding a row is as follows:

    -   You can enter constants. The value must be enclosed by a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as not identified.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15515083817675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources, including CPU, memory, and network bandwidth consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: The machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource. We recommend that you add a Custom Resource Group. Currently only East China 1, East China 2 supports adding custom resource groups. For more information, see[Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to synchronously extract data from an Oracle database:

```
{
    "type": "job",
    "version": "2.0"} //Indicates the version.
    "steps":[
        {
            "stepType":"oracle",
            "parameter": {
                "fetchSize: 1024, // The configuration item defines the number of plug-ins and database server-side data acquisition lines per volume
                "datasource": "", // fill in the added Data Source Name
                "column": [// column name
                    "id",
                    "name"
                ],
                "where": "", //Filtering condition
                "splitPk": "", // cut key
                "table": "// table name
            },
            "name": "Reader ",
            "category": "reader"
        },
        {// Below is a stream example, if it is the other plug-in, you can find the corresponding plug-in, fill in the corresponding content.
            "stepType": "stream ",
            "parameter":{}
            "name": "writer",
            "category": "writer"
        }
    ],
    "setting":{
        "errorLimit": {
            "record": "0"//Maximum number of error records
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
} "to": "Writer"
            }
        ]
    }
}
```

## Additional instructions {#section_lrm_l1q_p2b .section}

**Active/standby synchronous data recovery problem**

Active/standby synchronization means that Oracle uses an active/standby disaster recovery mode in which the standby database continuously restores data from the active database through binlog. Because of time difference in active/standby data synchronization, especially in situations such as network latency, the restored data in the standby database after synchronization is significantly different from the data of the active database. That is to say, the data synchronized in the standby database currently are not a full image of the active database.

**Consistency limits**

Oracle is an RDBMS system in terms of data storage, which can provide APIs for strong consistency data querying. For example, if another user writes data to the database during a synchronization task, Oracle Reader does not obtain the newly written data because of the database snapshot features. For more information on the database snapshot features, see [MVCC Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control).

The preceding are characteristics of data synchronization consistency under the Oracle reader single-threaded model, since Oracle reader can use Concurrent Data Extraction based on your configuration information, data consistency is not strictly guaranteed. When the Oracle reader shards are based on the SplitPk data, multiple concurrent tasks are initiated to complete the data synchronization. Since multiple concurrent tasks do not belong to the same read transaction and time intervals exist between the concurrent tasks, the data is incomplete and data snapshot information is inconsistent .

Multi-thread consistent snapshot requirements can only be solved from an engineering perspective. The following are suggested engineering solutions, and you can choose according to your circumstances.

-   Use single-threaded synchronization without data sharding. This is slow but can ensure robust data consistency.
-   Close other data writers to ensure the current data is static. For example, you can lock the table or close standby database synchronization. The disadvantage is it may affect online businesses.

**Database coding problem**

The Oracle Reader extracts data using JDBC at the underlying level. JDBC is applicable to all types of encodings and can complete transcoding at the underlying level. Therefore, the Oracle Reader can obtain the encoding and complete transcoding automatically without the need to specify the encoding.

The Oracle Reader cannot identify inconsistencies between the encoding written in the underlying layer of the Oracle system and the configured encoding, nor provides a solution. Due to this issue, \*\*the exported codes may contain junk codes\*\*.

**Incremental synchronization**

Since Oracle Reader extracts data using JDBC SELECT statements, you can extract incremental data using the SELECT and WHERE clauses using either of the following methods:

-   When online database applications write data into the database, the modify field is entered with the modification timestamp, including addition, update, and deletion \(logical deletion\). For this type of applications, Oracle Reader only requires the WHERE clauses followed by the last synchronization phase timestamp.
-   For new streamline data, the Oracle Reader requires the WHERE clauses followed by the maximum auto-increment ID of the last synchronization phase.

In case no field is provided for the business to identify added or modified data, the Oracle Reader cannot perform incremental data synchronization and can only perform full data synchronization.

**SQL security**

The Oracle Reader provides querySQL statements for you to SELECT data. The Oracle Reader does not perform security verification on querySQL.

