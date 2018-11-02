# Configure MaxCompute Writer {#concept_jjy_y4m_q2b .concept}

In this article we will show you the data types and parameters supported by MaxCompute Writer and how to configure Writer in both wizard mode and script mode.

The MaxCompute Writer plug-in is designed for ETL developers to insert or update data in MaxCompute. With the ability to import business data to MaxCompute, this plug-in is suitable for TB and GB-level data transmission.

**Note:** Before you start configuring the maxcompute writer plug-in, first configure the data source. For more information, see[Configure MaxCompute Data Source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure MaxCompute data source.md#).

For a detailed introduction to maxcompute, see [Introduction to maxcompute](https://www.alibabacloud.com/help/doc-detail/27800.htm).

At the underlying implementation level, it writes data into MaxCompute by using Tunnel based on the source project/table/partition/table field and other information you configured.  For common Tunnel commands, see [Tunnel Command Operations](https://www.alibabacloud.com/help/doc-detail/27833.htm).

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

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|None|
|table|Description: the name of the data table to write data into \(case-insensitive\). Writing data into multiple tables is not supported.|Yes|None|
|partition|The partition information of the data table must be written. Specify the parameter until the last-level partition. For example, if you want to write data to a three-level partition table, configure through to a last-level partition, for example, pt= 20150101, type=1, biz=2.-   For non-partition tables, this value must not be entered, which means that the data is directly imported to the target table.
-   MaxCompute Writer does not support writing data via routing. For partition tables, always ensure the data is written through to a last-level partition.

|Required if the table is a partition table. For non-partition tables, it is left empty.|None|
|column|A list of fields that need to be imported, which can be configured as `"column": ["*"]` when all fields are imported ": \["\*"\] When you need to insert a partial maxcompute column, fill in a partial column, for example, `"column": ["id, name"]`.-   Maxcompute writer supports Column Filtering, column switching, for example, there are three fields in a table, A, B, and C, you can configure to `"column": ["c, b"]` by synchronizing only the C and B fields ": \["C, B"\] During the import process, field a is automatically empty, set to null.
-   Column must contain the specified column set to be synchronized and it cannot be blank.

 |Yes|None|
|truncate|Description: `"truncate": "true"` is configured to ensure the idempotence of write operations. When a reattempt is made after a failed write attempt, MaxCompute Writer cleans up this data and imports the new data. This ensures the data is consistent after each rerunning.The option truncate is not an atomic operation. Because MaxCompute SQL is used for data cleansing, SQL cannot be atomic. Therefore, when multiple tasks clean up a Table/Partition at the same time, the concurrency and timing problem may occur. So proceed with caution.

To avoid this problem, we recommend that you try not to operate on one partition with multiple job DDLs at the same time, or that you create partitions before starting multiple concurrent jobs.

|Yes|None|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configuration item descriptions:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16247/15411409118125_en-US.png)

    Parameters:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Table: table in the preceding parameter description. Select the table to be synchronized.
    -   Partition information: If all columns are specified, you can configure them in column, for example, "column ": \[""\]. Partition supports configuration methods that configure multiple partitions and wildcard characters.

        -   `"partition":"pt=20140501/ds=*"` represents all partitions in DS.
        -   `"partition":"pt=top?"` In? indicates whether the character in front of it exists. This configuration specifies the two partitions with pt=top and pt=to.
        You can enter the partition columns to be synchronized, such as partition columns with pt. Example: Assuming that the value of each MaxCompute partition is pt=$\{bdp.system.bizdate\}, add the partition name pt to a field in the source table, ignore the unrecognized mark if any, and proceed with the next step. To synchronize all partitions, configure the partition value to pt=$\{\*\}. To synchronize a certain partition, select a time value for the partition.

    -   Cleaning rules:
        -   Clean up Existing Data Before Import: All data in the table or partition is cleaned up before import, which is equivalent to insert overwrite.
        -   2\) Keep existing data before writing: No data needs to be cleared before data importing. New data is always appended with each run, which is equivalent to "Insert into".
    -   Compression: Default selection is not compressed.
    -   Whether the empty string is null: the default is yes.
2.  The field mapping, which is the column in the above parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click Add row to **add a single** field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16247/15411409118126_en-US.png)

    -   In-row mapping: You can click **In-row Mapping** to create a mapping for the same row. Note that the data type must be consistent.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15411409117675_en-US.png)

    Parameters:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only 1 East China, east China 2 supports adding custom resource groups\). For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

For example, under the script configuration example, please refer to the above parameter descriptions for details.

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

MaxCompute itself does not support column filtering, reordering, and null filling, but MaxCompute Writer does. For example, a list of fields that need to be imported, which can be configured as `"column": ["*"]` when all fields are imported ": \["\*"\].

The maxcompute table has three fields, A, B, and C, you can configure the column as `"column": ["c","b"]` by synchronizing only the C and B fields ": \["C", "B"\], indicates that the first and second columns of reader will be imported into the C and B fields of maxcompute, the newly inserted a field in the maxcompute table is set to null.

**Column configuration error handling**

To ensure data is written in a reliable manner, data loss from redundant columns must be prevented to avoid data quality failure. When redundant columns are written, MaxCompute Writer produces an error. For example, if the MaxCompute table has fields a, b, and c, but MaxCompute Writer writes more than three fields, MaxCompute Writer produces an error.

**Partition configuration**

MaxCompute Writer only provides the write through to a last-level partition function, and does not support partition routing of writing based on a specific field. For a table that has three levels of partition, you must specify writing data to a level-3 partition. For example, to write data to the level-3 partition of a table, you can configure it to pt=20150101,type＝1,biz=2, but not pt=20150101,type＝1 or pt=20150101.

**Task rerunning and failover**

In MaxCompute Writer, `"truncate": true` is configured to ensure the idempotence of write operations. When a reattempt is made after a failed write attempt, MaxCompute Writer cleans up this data and imports the new data. This ensures the data is consistent after each rerunning. If the task is interrupted by any exceptions during the running process, the atomicity of the data cannot be guaranteed, nor will the data be rolled back or rerun automatically. It is required that you use this idempotence to rerun the task to ensure data integrity.

**Note:** 

Setting "truncate" to "true" cleans up all the data of the specified partition or table, so proceed with caution.

