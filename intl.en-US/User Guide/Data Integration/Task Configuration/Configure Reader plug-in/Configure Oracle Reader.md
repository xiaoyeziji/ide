# Configure Oracle Reader {#concept_t3p_vvp_p2b .concept}

The Oracle Reader plug-in provides the ability to read data from Oracle. At the underlying implementation level, Oracle Reader connects to a remote Oracle database through JDBC and runs the SELECT statements to extract data from the database.

On the public cloud, RDS or DRDS does not provide the Oracle storage engine. Currently, Oracle Reader is mainly used for private cloud data migration and Data Integration projects.

In short, Oracle Reader connects to a remote Oracle database through the JDBC connector. The SELECT SQL query statements are generated and sent to the remote Oracle database based on your configuration. Then, the SQL statements are run and the returned results are assembled into abstract datasets using the custom data types of data synchronization. Datasets are passed to the downstream writer for processing.

-   Oracle Reader concatenates the table, column, and WHERE information you configured into SQL statements and sends them to the Oracle database.
-   Oracle directly sends the querySQL information you configured to the Oracle database.

## Type conversion list {#section_h4p_w2n_q2b .section}

Oracle Reader supports most data types in DB2. Check whether your data type is supported.

Oracle Reader converts Oracle data types as follows:

|Type Classification|Oracle data type|
|:------------------|:---------------|
|Integer|Number, rawd, integer, Int, and smallint|
|Float| Numeric, decimal, float, double precisioon, real

 |
|String type|Long, Char, nchar, varchar, varchar2, nvar2, clob, nclob, character, character varying, char varying, national character, National char, National Character varying, national char varying and nchar varying|
|Date and time type|Timestamp and Date|
|boolean|It's Bit and Bool|
|Binary|Blob, BFile, Raw, and Long Raw|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|table|The name of the selected table that needs to be synchronized.|Yes|N/A|
|column|Description: The column name set to be synchronized in the configured table. Field information is described with arrays in JSON. \[" \*\*\*"\] indicates all columns by default.-   Column pruning is supported, which means you can select some columns to export.
-   Change of column order is supported, which means you can export the columns in an order different from the schema order of the table.
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
|splitPk|If you specify the splitPk when using RDBMSReader to extract data, it means that you want to use the fields represented by splitPk for data sharding. Then, the DataX starts concurrent tasks to synchronize data, which greatly improves the efficiency of data synchronization.-   If you are using splitPk, we recommend that you use the primary keys of tables, because the primary keys are generally even and data hot spots are less prone to split data fragments.
-   The data types supported by splitPk include the integer, string, floating point, and date.
-   If splitPk is left blank, it indicates that no table splitting is required and Oracle Reader synchronizes full data through a single channel.

|No |N/A|
|where|Filtering condition. Oracle Reader concatenates an SQL command based on specified column, table, and WHERE conditions and extracts data according to the SQL command. For example, you can set the WHERE condition as row\_number\(\) during a test. In actual service scenarios, incremental synchronization typically synchronizes the data generated on the current day. You can specify the WHERE condition as `id > 2 and sex = 1`. -   The where condition can be effectively used for incremental synchronization.
-   The WHERE condition can be effectively used for incremental synchronization.

|No |N/A|
|querySQL\(advanced mode, wizard mode not available\)|In some service scenarios, the WHERE condition is insufficient for filtering. In such cases, you can customize a SQL filter using this parameter. When this item is configured, the data synchronization system filters data using this configuration item directly instead of such configuration items as table and column. For example, for data synchronization after multi-table join, use `select a,b from table_a join table_b on table_a.id = table_b.id`. When querySQL is configured, Oracle Reader directly ignores the configuration of table, column, and WHERE conditions.|No |N/A|
|fetchSize|It defines the pieces of batch data that the plug-in and database server can fetch each time. The value determines the number of network interactions between the DataX system and the server, which can greatly improve data extraction performance.**Note:** The fetchsize value \(\> 2048\) may cause the data synchronization process OOM.

|No|1,024|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the source and destination of the data for the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16228/15367217517790_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Table: table in the preceding parameter description. Select the table to be synchronized.
    -   Data Filtering: you are about to synchronize the filtering criteria for the data, and the limit keyword filtering is not supported for the time being. The SQL syntax is consistent with the selected data source.
    -   Cut key: You can use a column in the source data table as a cut key, it is recommended that you use a primary key or an indexed column as a split key, and that only fields of type Integer be supported.

        During data reading, the data is split based on the configured fields to achieve concurrent reading, improving data synchronization efficiency.

        **Note:** The configuration of splitting key is related to the source selection in data synchronization. The splitting key configuration item is displayed only when you configure the data source.

2.  The field mapping, which is the column in the above parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16228/15367217517791_en-US.png)

    -   Peer mapping: Click **Enable Same-Line Mapping** to establish a corresponding mapping relationship in the peer, note that match the data type.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit the fields. Each line indicates a field. The first and end blank lines are ignored.
    The function of adding a row is as follows:

    -   You can enter constants. The value must be enclosed by a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   You can enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as Not identified.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15367217517675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only 1 East China, east China 2 supports adding custom resource groups\). For more information, see[Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).

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

**Master standby synchronous data recovery problem**

Master/slave synchronization means that Oracle uses a master/slave disaster recovery mode in which the slave database continuously restores data from the master database through binlog. Due to the time difference in master/slave data synchronization, especially in some special situations such as network latency, the restored data in the slave database after synchronization are significantly different from the data of the master database, that is to say, the data synchronized in the slave database are not a full image of the master database at the current time.

**Consistency Constraints**

Oracle is an RDBMS system in terms of data storage, which can provide APIs for querying strong consistency data. For example, if another data writer writes data to the database during a synchronization task, Oracle Reader does not get the newly written data because of the snapshot features of the database. For the snapshot features of the database, see [MVCC Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control) .

Above are the characteristics of data synchronization consistency under the Oracle reader single-threaded model, since Oracle reader can use Concurrent Data Extraction Based on your configuration information, data consistency is not strictly guaranteed. When the Oracle reader is split Based on the splitpk data, multiple concurrent tasks are initiated to complete the synchronization of data. Since multiple concurrent tasks do not belong to the same read transaction and time intervals exist between the concurrent tasks, the data is not complete and consistent data snapshot information.

For the need of multithread consistent snapshot, it can not be realized technically at present, but can only be solved from the perspective of engineering. The method of engineering exists, and the following solutions are provided here, and you can choose according to your own circumstances.

-   - Use single-threaded synchronization without data sharding. This is slow but can ensure robust data consistency.
-   - Close other data writers to ensure the current data is static. For example, you can lock the table or close slave database synchronization. The disadvantage is that the online businesses may be affected.

**Database coding problem**

Oracle Reader extracts data using JDBC at the underlying level. JDBC is applicable to all types of encodings and can complete transcoding at the underlying level. Therefore, Oracle Reader can obtain the encoding and complete transcoding automatically without the need to specify the encoding.

Oracle Reader cannot identify the inconsistency between the encoding written to the underlying layer of the Oracle system and the configured encoding, nor provide a solution. Due to this issue, \*\*the exported codes may contain garbage codes\*\*.

**Incremental Synchronization**

Since Oracle Reader extracts data using JDBC SELECT statements, you can extract incremental data using the SELECT and WHERE conditions in either of the following ways:

-   When online database applications write data into the database, the modify field is filled with the modification timestamp, including addition, update, and deletion \(logical deletion\). For this type of applications, Oracle Reader only requires the WHERE condition followed by the timestamp of the last synchronization phase.
-   For new streamline data, Oracle Reader requires the WHERE condition followed by the maximum auto-increment ID of the last synchronization phase.

In case no field is provided for the business to identify the addition or modification of data, Oracle Reader cannot perform incremental data synchronization and can only perform full data synchronization.

**SQL Security**

Oracle Reader provides querySQL statements for you to SELECT data by yourself. Oracle Reader conducts no security verification on querySQL.

