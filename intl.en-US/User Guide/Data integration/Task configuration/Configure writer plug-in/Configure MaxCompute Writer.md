# Configure MaxCompute Writer {#concept_jjy_y4m_q2b .concept}

This topic describes the data types and parameters supported by MaxCompute Writer and how to configure Writer in both wizard and script modes.

The MaxCompute Writer plug-in is designed for ETL developers to insert or update data in MaxCompute and has the capability to import business data to MaxCompute. This plug-in is suitable for TB and GB-level data transmission.

**Note:** Before you start configuring the MaxCompute writer plug-in, first configure the data source. For more information, see[Configure MaxCompute data source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure MaxCompute data source.md#).

For more information on MaxCompute, see [Introduction to MaxCompute](https://www.alibabacloud.com/help/doc-detail/27800.htm).

At the underlying implementation level, MaxCompute Writer writes data into MaxCompute by using Tunnel based on the source project, table, partition, table field, and other configured information. For more information on common, see [Tunnel Command Operations](https://www.alibabacloud.com/help/doc-detail/27833.htm).

## Supported data type {#section_yn2_gpm_q2b .section}

MaxCompute Writer supports the following data types in MaxCompute:

|Data|MaxCompute data|
|:---|:--------------|
|Integer|Bigint|
|Float|Double and decimal|
|String type|String|
|Date and time|Datetime|
|Boolean|Boolean|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. The name must be identical to the added data source name. Adding data source is supported in script mode.|Yes|None|
|table|The data table name to write data into is case-insensitive. Writing data into multiple tables is not supported.|Yes|None|
|partition|The data table partition information must be written. Specify the parameter until the last-level partition. For example, to write data in a three-level partition table, you must configure to the last-level partition, for example, pt=20150101, type=1, biz=2.-   This parameter is not required for non-partition tables, this results in the data directly imported to the target table.
-   MaxCompute Writer does not support writing data by routing. For partition tables, always ensure data is written through to a last-level partition.

|Required if the table is a partition table. This can be left empty in non-partition tables.|None|
|column|A list of fields that need to be imported, which can be configured as `"column": ["*"]` when all fields are imported ": \["\*"\] When you need to insert a partial MaxCompute column, enter a partial column, for example, `"column": ["id","name"]`.-   MaxCompute writer supports Column Filtering, column switching, for example, there are three fields in a table, A, B, and C. You can configure to `"column": ["c","b"]` by synchronizing only the C and B fields. During the import process, field A is automatically empty, and set to null.
-   Column must contain the specified column set to be synchronized and it cannot be blank.

 |Yes|None|
|truncate|`"truncate": "true"` is configured to ensure the idempotent of write operations. When a reattempt is made after a failed write attempt, MaxCompute Writer cleans up this data and imports new data. This ensures the data is consistent after each rerun.The option truncate is not an atomic operation. SQL cannot be atomic because MaxCompute SQL is used for data cleansing. Therefore, when multiple tasks clean up a Table/Partition at the same time, the concurrency and timing problem may occur. So proceed with caution.

To avoid this problem, we recommend that you do not operate on one partition with multiple job DDLs at the same time, or that you create partitions before starting multiple concurrent jobs.

|Yes|None|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configuration item descriptions:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16247/15514328488125_en-US.png)

    Parameters:

    -   Data source: The datasource in the preceding parameter description. Enter the configured data source name.
    -   Table: The table in the preceding parameter description. Select the table for synchronization.
    -   Partition information: If all columns are specified, you can configure them in column, for example, "column ": \[""\]. Partition supports configuration methods that configure multiple partitions and wildcard characters.

        -   `"partition":"pt=20140501/ds=*"` Represents all partitions in DS.
        -   `"partition":"pt=top?"` In? indicates whether the character in front of it exists. This configuration specifies the two partitions with pt=top and pt=to.
        You can enter the partition columns for synchronization, such as partition columns with pt. For example: Assume the value of each MaxCompute partition is pt=$\{bdp.system.bizdate\}, add the partition name pt to a field in the source table, ignore the unrecognized mark if any, and proceed to the next step. To synchronize all partitions, configure the partition value to pt=$\{\*\}. To synchronize a specific partition, select a time value for the partition.

    -   Clearance rules:
        -   Clear Existing Data Before Import: All data in the table or partition is cleared before import, which is equivalent to insert overwrite.
        -   Keep Existing Data Before Writing: No data is cleared before data import. New data is always appended with each run, which is equivalent to "Insert into".
    -   Compression: Default selection is not compressed.
    -   Whether the empty string is null: The default setting is yes.
2.  The field mapping is the column in the above parameter description.

    The source table field on the left and the target table field on the right have a one-to-one relationships, click Add row to **Add a Single** field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16247/15514328488126_en-US.png)

    -   In-row mapping: You can click **In-row Mapping** to create a mapping for the same row. Note that the data type must be consistent.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15514328497675_en-US.png)

    Parameters:

    -   DMU: A unit that measures the resources consumed during data integration, including CPU, memory, and network bandwidth. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: The maximum number of threads used to concurrently read/write data into the data storage media in a data synchronization task. In Wizard Mode, configure a concurrency for the specified task on the Wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task resource group: The machine on which the task runs. If there is a large number of tasks, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only East China 1 and East China 2 supports adding custom resource groups\). For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

## Development in Script Mode {#section_cp2_wsh_p2b .section}

The following example is a script configuration. Please refer to the preceding parameter descriptions for details.

```
{
    "type": "job",
    "version": 2.0 ", // version number
    "steps":[
        {//You can locate the corresponding writer plug-in documentation among the following documentations.
            "stepType":"stream",
            "parameter":{},
            "name": "Reader ",
            "category":"reader"
        },
        {
            "stepType":"odps", // plug-in name
            "parameter":{
                "partition": "",//Shard information
                "truncate": true, // write Rule
                "compress":false, //do you Want to compress?
                "datasource": "odps_first",//The data source name.
                "column": [// column name
                    "*"
                ],
                "emptyAsNull": false, if the empty string is null?
                "table": ""// table name
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "Setting ":{
        "errorLimit": {
            "record": "0"//Maximum number of error records
        },
        "speed": {
            "throttle":false, // do you want to limit the flow?
            "concurrent": "1",//Number of concurrent tasks
            "dmu": 1 // DMU Value
        }
    },
    "order":{
        "hops":[
            {
                "from":"Reader",
                "to":"Writer"
            }
        ]
    }
}
```

## Additional instructions {#section_j3w_xrm_q2b .section}

**Questions about Column Filtering**

MaxCompute does not support column filtering, reordering, and null filling, but MaxCompute Writer does. For example, a list of fields that need to be imported can be configured as `"column": ["*"]` when all fields are imported ": \["\*"\].

The MaxCompute table has three fields, A, B, and C. You can configure the column as `"column": ["c","b"]` by synchronizing only C and B fields ": \["C", "B"\], indicates the first and second columns of reader will be imported into the C and B fields of MaxCompute. The newly inserted a field in the MaxCompute table is set to null.

**Column configuration error handling**

To ensure data is written in a reliable manner, data loss from redundant columns must be prevented to avoid data quality failure. When redundant columns are written, MaxCompute Writer produces an error. For example, the MaxCompute Writer will generate an error when the MaxCompute table has fields A, B, and C, but the MaxCompute Writer writes more than three fields.

**Partition configuration**

MaxCompute Writer only provides the write through to a last-level partition function, and does not support partition routing of writing based on a specific field. For a table that has three levels of partition, you must specify writing data to a level-3 partition. For example, write data to the level-3 partition of a table. You can configure it to pt=20150101, type=1, biz=2, but not pt=20150101, type=1 or pt=20150101.

**Task rerun and failover**

In MaxCompute Writer, `"truncate": true` is configured to ensure the idempotent of write operations. When a reattempt is made after a failed write attempt, MaxCompute Writer cleans up this data and imports new data. This ensures data is consistent after each rerun. If the task is interrupted by any exceptions during the run process, the data atomicity cannot be guaranteed, nor will data be rolled back or rerun automatically. It is required that you use this idempotent to rerun the task to ensure data integrity.

**Note:** 

Setting "truncate" to "true" cleans up all data of the specified partition or table, so proceed with caution.

