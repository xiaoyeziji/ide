# Configure PostgreSQL Writer {#concept_pyy_trn_q2b .concept}

This topic describes the data types and parameters supported by PostgreSQL Writer and how to configure Writer in both Wizard Mode and Script Mode.

The PostgreSQL Writer plug-in reads data from PostgreSQL. At the underlying implementation level, PostgreSQL Writer connects to a remote PostgreSQL database through Java DataBase Connectivity \(JDBC\) and runs corresponding SQL statements to select data from the PostgreSQL database. On the public cloud, Relational Database Service \(RDS\) provides a PostgreSQL storage engine. 

**Note:** Configure the data source before configuring a PostgreSQL Writer plug-in. For details, see [Configure PostgreSQL data source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure PostgreSQL data source.md#).

In short, PostgreSQL Writer connects to a remote PostgreSQL database through a JDBC connector, and generates SELECT SQL query statements based on your configurations, and then sends the statements to the remote PostgreSQL database. The PostgreSQLWriter then assembles returned results of the executed SQL statement into abstract datasets through the custom CDP data types, and passes the datasets to the downstream writer.

-   PostgreSQL Writer concatenates the configured table, column, and WHERE information into SQL statements and sends them to the PostgreSQL database.
-   PostgreSQL directly sends the configured querySQL information to the PostgreSQL database.

## Type conversion list {#section_vcg_fvn_q2b .section}

PostgreSQL Writer supports most PostgreSQL data types. Check whether the data type you are using is supported.

PostgreSQL Writer converts PostgreSQL data types as follows:

|Data integration internal types| PostgreSQL data type|
|:------------------------------|:--------------------|
|Long|Bigint, Bigserial, Integer, Smallint, and Serial|
|Double|Double Precision, Money, Numeric, and Real|
|String|Varchar, Char, Text, Bit, and Inet|
|Date|Date, Time, and Timestamp|
|Boolean|Bool|
|Bytes|Bytea|

**Note:** 

-   Only the preceding field types are supported.
-   To convert data types including "money", "inet", and "bit", you need to use syntaxes, such as "a\_inet::varchar".

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Parameter|Description| Required|Default value|
|:--------|:----------|:--------|:------------|
|datasource|The data source name. Adding data source is supported in Script Mode, the data source name must be the same as the added data source. .|Yes|None|
|table|The selected table name that requires synchronization.|Yes|None|
|writeMode|The specified import mode, which allows data insertion.insert: If the primary key conflicts with the unique index, Data Integration determines the data as dirty data, but retains the original data.

|No|insert|
|column|The target table fields into which data is required to be written. These fields are separated by commas \(,\). For example: `"column":["id","name","age"]`. To write all columns subsequently, use the asterisk \(\*\) for representation. For example: `"column":["*"]`|Yes|None|
|preSQL|The SQL statement that runs before running the data synchronization task. Currently, you can run only one SQL statement in Wizard Mode, and multiple SQL statement in Script Mode. For example: clear old data.|No|None|
|postSQL|The SQL statement that runs after running the data synchronization task d. Currently, you can run only one SQL statement in Wizard Mode, and multiple SQL statements in Script Mode. For example: add a timestamp.|No|None|
|batchSize|The number of records submitted in an operation. This parameter can greatly reduce interactions between Data Integration and PostgreSQL over the network, and increase the overall throughput. However, an excessively large value may cause the running process of Data Integration to become Out of memory \(OOM\).|No|1,024|

## Development in Wizard Mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    The following is configuration item descriptions:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16253/15514335468214_en-US.png)

    Parameters:

    -   Data source: The datasource in the preceding parameter description section. Enter the configured data source name.
    -   Table: The table in the preceding parameter description section. Select the table for synchronization.
    -   Before import: The preSQL in the preceding parameter description section, namely, the SQL statement that runs before running the data synchronization task.
    -   After import: The postSQL in the preceding parameter description section, which is the SQL statement that runs after running the data synchronization task.
2.  Field mapping: The column in the preceding parameter description section.

    The Source Table Field on the left maps with the Target Table Field on the right. To add a field, click **Add Line**. To delete a line, move the cursor over a line, and click **Delete**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16253/15514335468215_en-US.png)

    -   In-row mapping: To create a mapping for the same row, click **Enable Same-Line Mapping**. Note the mapped data type must be consistent.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15514335467675_en-US.png)

    Parameters:

    -   DMU: The unit that measures the resources consumed during data integration, including CPU, memory, and network bandwidth. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: The maximum number of threads used to concurrently read /write data into the data storage media in a data synchronization task. You can configure a concurrency for the specified task under Wizard Mode.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task resource group: The machine on which the task runs. If the number of tasks is large, the default Resource Group is used to wait for a resource. We recommend that you add a Custom Resource Group. \(Currently, only 1 East China and East China 2 regions support adding custom resource groups\). For more information, see[Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

## Development in Script Mode {#section_cp2_wsh_p2b .section}

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

