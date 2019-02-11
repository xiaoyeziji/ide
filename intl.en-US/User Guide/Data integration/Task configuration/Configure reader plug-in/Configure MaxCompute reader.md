# Configure MaxCompute reader {#concept_xtt_j1p_p2b .concept}

The MaxCompute Reader plug-in allows you to read data from MaxCompute. For more information about MaxCompute, see [MaxCompute Overview](https://www.alibabacloud.com/help/doc-detail/27800.htm).

At the underlying implementation level, the MaxCompute Reader plug-in reads data from the MaxCompute system by using a Tunnel based on the source project, table, partition, table fields and other configured information. For common Tunnel commands, see [Tunnel Command Operations](https://www.alibabacloud.com/help/doc-detail/27833.htm).

MaxCompute Reader can read both partition and non-partition tables, but cannot read virtual views. To read a partition table, you must specify the partition configuration. For example, to read table t0 with a partition configuration of “pt=1, ds=hangzhou”, you must set the value in the configuration. For a non-partition table, the partition configuration is empty. For table fields, you can specify all or some of the columns sequentially, change the column order arrangement, and specify constant fields and partition columns. \(A partition column is not a table field\).

## Supported data types {#section_yn2_gpm_q2b .section}

MaxCompute Reader supports the following data types in MaxCompute.

|Data type|MaxCompute data type|
|:--------|:-------------------|
|Integer|bigint|
|Floating point|double, decimal|
|String|string|
|Date|Datetime|
|Boolean|Boolean|

## Parameter description {#section_jn2_gqh_p2b .section}

|Parameter|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. It must be identical to the added data source name. Adding data source is supported in script mode.|Yes|None|
|table|The data table name to be read. It is case-insensitive.|Yes|None|
|partition|The partition information of the read data. Linux shell wildcards are allowed \("" represents 0 or multiple characters, and "?" represents any character.\) For example, a partition table named "test" has four partitions: pt=1/ds=hangzhou, pt=1/ds=shanghai, pt=2/ds=hangzhou, and pt=2/ds=beijing.-   To read data from partition pt=1/ds=shanghai, configure it to `"partition":"pt=1/ds=shanghai"`.
-   To read data from all partitions under pt=1, configure it to `"partition":"pt=1/ds=* "`.
-   To read data from all partitions of the "test" table, configure it to `"partition":"pt=*/ds=*"`.

|This configuration is required for partition tables, but can be left empty for non-partition tables.|None|
|column|The MaxCompute source table column information. For example, the fields of a table named “test” are id, name, and age.-   To read the fields in turn, configure it to `"column":["id","name","age"]` or `"column":["*"]`. 

**Note:** We do not recommend configuring the extracted field with an asterisk \(\*\) because it indicates every table field is read sequentially. If you change the order or table field types, add or delete some table fields. It is likely the source table columns cannot be aligned with the target table columns, causing errors or even exceptions.

-   To read name and id sequentially, configure it to: `"coulumn":["name","id"]`. 
-   To add a constant field in the fields extracted from the source table to match the target table field order. For example, if the data values you want to extract are values of age, name, constant date "1988-08-08 08:08:08", and id columns, configure it to: `"column":["age","name","'1988-08-08 08:08:08'","id"]`, with the constant value enclosed by `'`. In internal implementation, any field enclosed by `'` is considered a constant field, and its value is the content in the `'`.

**Note:** 

    -   MaxCompute Reader does not use Select SQL statements for extracting table data. Therefore, you cannot specify field functions.
    -   The column must contain the specified synchronized column set and cannot be blank.

 |Yes|None|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the synchronization task data source and destination.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16225/15498531987759_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the configured data source name.
    -   Table: The table in the preceding parameter description. Select the table for synchronization.
    **Note:** If you specify all columns, you can configure them in the column. For example, "column ": \[""\]. Partition supports configuration methods that configure multiple partitions and wildcard characters.

    -   `"partition": "pt=20140501/ds=*"`: Reads data from all partitions in ds. 
    -   `"partition":"pt=top?"` The question mark \(?\) means whether the preceding character exists. This configuration specifies the two partitions with pt=top and pt=to.
    You can enter partition columns for synchronization, such as partition columns with pt. Example: Assuming that the value of each MaxCompute partition is pt=$\{bdp.system.bizdate\}, add the partition name pt to a source table field, ignore the unrecognized mark if any, and proceed to the next step. To synchronize all partitions, configure the partition value to pt=$\{\*\}. To synchronize a certain partition, select a partition time value.

2.  Field mapping: The column in the preceding parameter description.

    The Source Table Field on the left maps with the Target Table Field on the right. Click **Add Line** to add a field. To delete a line, move the mouse cursor over a line and click **Delete**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16225/15498531987760_en-US.png)

    -   In-row mapping: You can click **Enable Same-Line Mapping** to create a mapping for the same row. Note that the data type must be consistent.
    -   Automatic formatting: The fields are automatically sorted by rules.
    -   Manually edit source table field: Manually edit fields, where each line indicates a field. The first and end blank lines are ignored.
    By clicking Add Row, 

    -   Each constant must be enclosed in a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   Enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as 'Unidentified'.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15498531987675_en-US.png)

    Configurations:

    -   DMU: A unit that measures resources consumed during data integration, including CPU, memory, and network bandwidth. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read or write data into the data storage media in a data synchronization task. Under wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: The machine on which the task runs, if there are a large number of tasks, the default Resource Group is used for resource pending. We recommend you add a Custom Resource Group. Currently, only East China 1 and East China 2 supports adding custom resource groups. For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common Configuration/Add task resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

For more information on how to configure a job for extracting data locally from MaxCompute, see the preceding parameter descriptions for details.

```
{
    "type":"job",
    "version":"2.0",
    "steps":[
        {
            "stepType":"odps", // plug-in name
            "parameter":{
                "partition": [], the partition where the read data is located
                "isCompress":false, //do you Want to compress?
                "datasource":"", //Data Source
                "column":column information for [//source table
                    "id",
                ],
                "emptyAsNull":true,
                "table":"//table name
            },
            "name":"Reader ",
            "category":"reader"
        },
        {//The following is a writer template. You can find the corresponding writer plug-in documentations.
            "stepType":"stream ",
            "parameter":{
            },
            "name":"Writer ",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record":"0"//Number of error records
        },
        "speed":{
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "concurrent":"1",//Number of concurrent tasks
            "dmu":1//DMU Value
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

