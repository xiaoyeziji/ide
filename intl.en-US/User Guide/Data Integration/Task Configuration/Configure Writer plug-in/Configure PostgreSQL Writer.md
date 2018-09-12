# Configure PostgreSQL Writer {#concept_pyy_trn_q2b .concept}

The PostgreSQL Writer plug-in reads data from PostgreSQL. At the underlying implementation level, PostgreSQL Writer connects to a remote PostgreSQL database through Java DataBase Connectivity \(JDBC\) and runs corresponding SQL statements to select data from the PostgreSQL database. On the public cloud, Relational Database Service \(RDS\) provides a PostgreSQL storage engine. 

**Note:** Configure the data source before configuring a PostgreSQL Writer plug-in. For details, see [Configure PostgreSQL Data Source](intl.en-US/User Guide/Data Integration/Data source configuration/Configure PostgreSQL Data Source.md#)Configure the PostgreSQL Data Source.

In short, PostgreSQL Writer connects to a remote PostgreSQL database through a JDBC connector, generates SELECT SQL query statements based on configuration, sends the statements to the remote PostgreSQL database, assembles returned results of SQL statement execution into abstract datasets through the custom data types of CDP, and passes the datasets to the downstream writer.

-   PostgreSQL Writer concatenates the configured table, column, and WHERE information into SQL statements and sends them to the PostgreSQL database.
-   PostgreSQL directly sends the configured querySql information to the PostgreSQL database.

## Type conversion list {#section_vcg_fvn_q2b .section}

PostgreSQL Writer supports most PostgreSQL data types. Check whether the data type is supported.

PostgreSQL Writer converts PostgreSQL data types as follows:

|Data integration internal types| PostgreSQL data type|
|:------------------------------|:--------------------|
|Long|bigint, bigserial, integer, smallint, and serial|
|Double|double precision, money, numeric, and real|
|String|varchar, char, text, bit, and inet|
|Date|Date, time, and timestamp|
|Boolean|bool|
|Bytes|Bytea|

**Note:** 

-   Except the preceding field types, other types are not supported.
-   For "money", "inet", and "bit", you need to use syntaxes such as "a\_inet::varchar" to convert data types.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|None|
|table|The name of the selected table that needs to be synchronized.|Yes|None|
|writeMode|Description: Specifies the import mode. Data can be inserted.insert: If the primary key conflicts with the unique index, Data Integration determines the data as dirty data but retains the original data.

|No|insert|
|column|Description: The fields of the target table into which data is required to be written. These fields are separated by commas. For example, `"column":["id","name","age"]`. If you want to write all columns in turn, use the \* representation, for example, `"column":["*"]`.|Yes|None|
|preSQL|Description: The SQL statement that is run before the data synchronization task is run. Currently, you can run only one SQL statement in wizard mode, and more than one SQL statement in script mode. For example, clear old data.|No|None|
|postSQL|SQL statement executed after the data synchronization task is executed. Currently, you can run only one SQL statement in wizard mode, and more than one SQL statement in script mode. For example, add a timestamp.|No|None|
|batchSize|Description: The quantity of records submitted in one operation. This parameter can greatly reduce the interactions between Data Integration and PostgreSQL over the network, and increase the overall throughput. However, an excessively large value may cause the running process of Data Integration to become out of memory \(OOM\).|No|1,024|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configuration item descriptions:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16253/15367227528214_en-US.png)

    Parameters:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Table: table in the preceding parameter description. Select the table to be synchronized.
    -   Prepared statement before import: preSQL in the preceding parameter description, namely, the SQL statement that is run before the data synchronization task is run.
    -   Post-import completion statement: postSQL in the preceding parameter description, which is the SQL statement that is run after the data synchronization task is run.
2.  Field mapping: The column in the preceding parameter description.

    The Source Table Field on the left maps with the Target Table Field on the right. Click **Add Line**, and then a field is added. Hover the cursor over a line, click **Delete**, and then the line is deleted.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16253/15367227528215_en-US.png)

    -   In-row mapping: You can click **Enable Same-Line Mapping** to create a mapping for the same row. Note that the data type must be consistent.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15367227527675_en-US.png)

    Parameters:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only 1 East China, east China 2 supports adding custom resource groups\). For more information, see[Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

The following is a script configuration sample. For details about parameters, see Parameter Description.

```
{
    "type": "job",
    "version": 2.0 ", // version number
    "steps": [// below is the template for reader, you can find the appropriate read plug-in documentation.
        {
            "stepType":"stream",
            "Parameter ":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType": "postgresql",/plug-in name
            "parameter": {
                "postSQL": [], // SQL statement that was first executed after the data synchronization task was executed
                "datasource": "// Data Source
                    "col1",
                    "col2",
                ],
                "table": ", // table name
                "postSQL": [], // SQL statement that was first executed after the data synchronization task was executed
            },
            "name":"Reader",
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
            "dmu": 1 // DMU Value
        }
    },
    "order":{
        "hops":[
            {
                "name":"Reader",
                "To": "Writer"
            }
        ]
    }
}
```

