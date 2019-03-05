# Configure HybridDB for PostgreSQL Writer {#concept_vzd_n1c_5fb .concept}

This topic describes the data types and parameters supported by HybridDB for PostgreSQL Writer and how to configure it in both Wizard and Script modes.

HybridDB for PostgreSQL Writer writes data into a HybridDB for PostgreSQL database. At the underlying implementation level, HybridDB for PostgreSQL Writer connects to a remote HybridDB for PostgreSQL database through JDBC, and runs SELECT statements to extract data from the database. On the public cloud, RDS provides the HybridDB for PostgreSQL storage engine.

**Note:** You must configure a data source before configuring HybridDB for PostgreSQL Writer. For more information, see [Configure a HybridDB for PostgreSQL data source](intl.en-US/User Guide/Data integration/Data source configuration/Configure HybridDB for PostgreSQL data source.md#).

In short, HybridDB for PostgreSQL Writer connects to a remote HybridDB for PostgreSQL database through a JDBC connector, generates SELECT statements based on the configuration, and sends the statements to the remote database. Then, HybridDB for PostgreSQL Writer assembles SQL execution results into abstract datasets in custom data types of Data Integration, and passes the datasets to the downstream writer.

-   HybridDB for PostgreSQL Writer concatenates the configured table, column, and WHERE information into SQL statements, and sends the statements to the HybridDB for PostgreSQL database.
-   HybridDB for PostgreSQL Writer sends the configured querySQL information to HybridDB for PostgreSQL database.

**Note:** 

## Type conversion list {#section_enj_rjr_5fb .section}

HybridDB for PostgreSQL Writer supports most data types in HybridDB for PostgreSQL. Check whether a data type is supported before configuring HybridDB for PostgreSQL Writer.

HybridDB for PostgreSQL Writer converts the data types in HybridDB for PostgreSQL as follows:

|Type classification|HybridDB for PostgreSQL data type|
|:------------------|:--------------------------------|
|Long|Bigint, Bigserial, Integer, Smallint, and Serial|
|Double|Double precision, Money, Numeric, and Real|
|String|Varchar, Char, Text, Bit, and Inet|
|Date|Date, Time, and Timestamp|
|Boolean|Boolean|
|Bytes|Bytea|

**Note:** 

-   Only the preceding field types are supported.
-   To convert Money, Inet, and Bit data types, you need to use syntax, such as a\_int::varchar.

## Parameter description {#section_pld_gkr_5fb .section}

|Parameter|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. The name must be identical to the added data source name. Script Mode supports adding data sources.|Yes|None|
|table|The name of the destination table.|Yes|None|
|writeMode|The Write Mode, which can be set to insert data. insert: If a primary key conflict or unique index conflict occurs, Data Integration determines the data as dirty data, and retains the original data.|No|Insert|
|column|The destination table fields into which data needs to be written. These fields are separated with commas \(,\). For example, `"column": ["id","name","age"]`. If you want to write all columns in turn, use the asterisk \(\*\). For example: "column": \["\*"\].|Yes|None|
|preSql|The SQL statement that runs before running the data synchronization task. For example, you can clear old data before data synchronization. Currently, you can run only one SQL statement in Wizard Mode, and multiple SQL statements in Script Mode.|No|None|
|postSql|The SQL statement runs after running the data synchronization task. For example, you can add a timestamp after data synchronization. Currently, you can run only one SQL statement in Wizard Mode, and multiple SQL statements in Script Mode.|No|None|
|batchSize|The number of records submitted in a batch. This parameter can greatly reduce the interaction frequency between Data Integration and HybridDB for PostgreSQL on the network, and increase the overall throughput. However, an excessively large value may lead to OOM during the data synchronization process.|No|1024|

## Development in Wizard Mode {#section_akn_qlr_5fb .section}

1.  **Specify data sources.**

    Configure the data source and destination for a synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62195/155175291932069_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |**Data Source**|The datasource parameter in the preceding parameter description. Select the configured data source.|
    |**Table**|The table parameter in the preceding parameter description. Select the destination table.|
    |**Before Import**|The preSQL parameter in the preceding parameter description. Enter the SQL statement that runs before running the data synchronization task.|
    |**After Import**|The postSQL parameter in the preceding parameter description. Enter the SQL statement that runs after running the data synchronization task.|

2.  Configure mappings of the fields \(the column parameters in the preceding parameter description\).

    Each source table field on the left maps a destination table field on the right. To add a mapping, click **Add**.To delete the current mapping, move the cursor over a line and click **Delete**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62195/155175291932070_en-US.png)

    |Configuration|Description|
    |:------------|:----------|
    |**Map Fields with the Same Name**|Click **Map Fields with the Same Name** to map fields with the same name. Note that the data type must be consistent.|
    |**Map Fields in the Same Row**|Click **Map Fields in the Same Row** to map the same row. Note that the data type must be consistent.|
    |**Remove Mappings**|Click **Remove Mappings** to remove established mappings .|
    |**Auto Layout**|The fields are automatically sorted based on specified rules.|

3.  **Configure channel control**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62195/155175291939902_en-US.png)

    |Configuration|Description|
    |:------------|:----------|
    |**DMU**|The unit that measures resources, including CPU, memory, and network resources consumed by Data Integration. A DMU represents the minimum operating capability of a Data Integration task, that is, the data synchronization processing capability given limited CPU, memory, and network resources.|
    |**Concurrent Jobs**|The maximum number of threads used to concurrently read data from the source or write data into the data storage media in a data synchronization task. In Wizard Mode, you can configure the concurrency for a task on the wizard page.|
    |**Dirty Data Records Allowed**|The maximum number of errors or dirty data records allowed.|
    |**Task Resource Group**|The machines on which tasks are run. If a large number of tasks are run on the default resource group, some tasks may be delayed due to insufficient resources. In this case, we recommend that you add a custom resource group. Currently, a custom resource group can be added only in the China \(Hangzhou\) and China \(Shanghai\) regions. For more information, see [Add task resources](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).|


## Development in Script Mode {#section_ugh_hnr_5fb .section}

```
{
    "type": "job",
    "steps": [
        {
            "parameter": {},
            "name": "Reader",
            "category": "reader"
        },
        {
            "parameter": {
                "postSql": [],// The SQL statement to be run after the data synchronization task is run.
                "datasource": "test_004",// The data source name.
                "column": [// The destination table columns.
                    "id",
                    "name",
                    "sex",
                    "salary",
                    "age"
                ],
                "table": "public.person",// The destination table name.
                "preSql": [],// The SQL statement to be run before the data synchronization task is run.
            },
            "name": "Writer",
            "category": "writer"
        }
    ],
    "version": "2.0",// The version number.
    "order": {
        "hops": [
            {
                "from": "Reader",
                "to": "Writer"
            }
        ]
    },
    "setting": {
        "errorLimit": {// The maximum number of errors allowed.
            "record": ""
        },
        "speed": {
            "concurrent": 6,// The number of concurrent threads.
            "throttle": false,// Indicates whether to throttle the transmission rate.
            "dmu": 7// The DMU value.
        }
    }
}
```

