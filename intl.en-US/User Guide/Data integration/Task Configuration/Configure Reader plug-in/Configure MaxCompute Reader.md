# Configure MaxCompute Reader {#concept_xtt_j1p_p2b .concept}

The MaxCompute Reader plugin provides the ability to read data from MaxCompute. For details about MaxCompute, see [MaxCompute Overview](https://www.alibabacloud.com/help/doc-detail/27800.htm).

At the underlying implementation level, it reads data from the MaxCompute system by using Tunnel based on the source project/table/partition/table fields and other information you configured. For common Tunnel commands, see [Tunnel Command Operations](https://www.alibabacloud.com/help/doc-detail/27833.htm).

MaxCompute Reader can read both partition and non-partition tables, but cannot read virtual views. To read a partition table, you must specify the partition configuration. For example, to read table t0 with a partition configuration of "pt=1, ds=hangzhou", you must set the value in the configuration. For a non-partition table, the partition configuration is left empty. For table fields, you can specify all or some of the columns sequentially, change the order in which columns are arranged, and specify constant fields and partition columns. \(A partition column is not a table field\).

## Supported data types {#section_yn2_gpm_q2b .section}

MaxCompute Writer supports the following data types in MaxCompute:

|Data type|MaxCompute data type|
|:--------|:-------------------|
|Integer|bigint|
|Floating point|double and decimal|
|String|string|
|Date and time type|Datetime|
|Boolean|Boolean|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A|
|table.|Reads the table name of the data table \(not case sensitive \).|Yes|N/A|
|partition|The information of the partition from which you read data. Linux shell wildcard is allowed \("" represents 0 or multiple characters, and "?"  represents any character.\) For example, a partition table named "test" has four partitions: pt=1/ds=hangzhou, pt=1/ds=shanghai, pt=2/ds=hangzhou, and pt=2/ds=beijing.-   To read data from partition pt=1/ds=shanghai, configure it to `"partition":"pt=1/ds=shanghai"`.
-   To read data from all the partitions under pt=1, configure it to `"partition":"pt=1/ds=* "`.
-   To read data from all the partitions of the "test" table, configure it to `"partition":"pt=*/ds=*"`.

|Required if the table is a partition table. For non-partition tables, it is left empty.|N/A|
|column|The column information of the MaxCompute source table.  For example, the fields of a table named "test" are id, name, and age. -   To read the fields in turn, configure it to `"column":["id","name","age"]]` or `"column":["*"]`. 

**Note:** We don't recommend that you configure the extracted field to "\*", because it indicates that every field in the table is read in turn. If you change the order or types of the table fields, or add or delete some table fields, it is likely that the source table columns cannot be aligned with the target table columns, causing incorrect results or even failure. 

-   To read name and id in sequence, configure it to `"coulumn":["name","id"]`. 
-   To add a constant field to the fields to be extracted from the source table \(to match the field order of the target table\),  for example, if the data values you want to extract are values of age, name, constant date "1988-08-08 08:08:08", and id columns, configure it to: `"column":["age","name","'1988-08-08 08:08:08'","id"]`, with the constant value enclosed by `'`. In internal implementation, any field enclosed by `'` is considered as a constant field, and its value is the content in`'`.

**Note:** 

    -   MaxCompute Reader does not use Select SQL statement of MaxCompute for extracting data from a table. Therefore, you cannot specify functions in fields.
    -   Column must contain the specified column set to be synchronized and it cannot be blank.

 |Yes|N/A|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the source and destination of the data for the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16225/15408695087759_en-US.png)

    Configurations:

    -   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
    -   Table: table in the preceding parameter description. Select the table to be synchronized.
    **Note:** If you specify all columns, you can configure them in column, for example, "column ": \[""\]. Partition supports configuration methods that configure multiple partitions and wildcard characters.

    -   `"partition": "pt=20140501/ds=*"`: Reads the data from all partitions in ds. 
    -   `"partition":"pt=top?"` The mark ? indicates whether the character in front of it exists. This configuration specifies the two partitions with pt=top and pt=to.
    You can enter the partition columns to be synchronized, such as partition columns with pt.  Example: Assuming that the value of each MaxCompute partition is pt=$\{bdp.system.bizdate\}, add the partition name pt to a field in the source table, ignore the unrecognized mark if any, and proceed with the next step. To synchronize all partitions, configure the partition value to pt=$\{\*\}. To synchronize a certain partition, select a time value for the partition.

2.  Field mapping: The column in the preceding parameter description.

    The Source Table Field on the left maps with the Target Table Field on the right. Click **Add Line**, and then a field is added. Hover the cursor over a line, click **Delete**, and then the line is deleted.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16225/15408695087760_en-US.png)

    -   In-row mapping: You can click **Enable Same-Line Mapping** to create a mapping for the same row. Note that the data type must be consistent.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
    -   Manually edit source table field: Manually edit the fields. Each line indicates a field. The first and end blank lines are ignored.
    By clicking Add Row, 

    -   Each constant must be enclosed in a pair of single quotes, such as 'abc' and '123'.
    -   Use this function with scheduling parameters, such as $\{bizdate\}.
    -   Enter functions supported by relational databases, such as now\(\) and count\(1\).
    -   If the value you entered cannot be parsed, the type is displayed as 'Not Identified'.
3.  Control the tunnel

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15408695087675_en-US.png)

    Configurations:

    -   DMU: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. One DMU represents the minimum amount of resources used for a data synchronization task.
    -   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   The maximum number of errors indicates the maximum number of dirty data records.
    -   Task Resource Group: the machine on which the task runs, if the number of tasks is large, the default Resource Group is used to wait for a resource, it is recommended that you add a Custom Resource Group \(currently only 1 East China, east China 2 supports adding custom resource groups\). For more information, see[Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).

## Development in script mode {#section_cp2_wsh_p2b .section}

To configure a job to extract data locally from MaxCompute, please refer to the above parameter descriptions for details.

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

