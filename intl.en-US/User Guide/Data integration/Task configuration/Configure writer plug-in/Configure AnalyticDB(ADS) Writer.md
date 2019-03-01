# Configure AnalyticDB\(ADS\) Writer {#concept_bft_jyk_q2b .concept}

This topic describes the data types and parameters supported by AnalyticDB\(ADS\) Writer and how to configure Writer in both wizard and script mode.

AnalyticDB Writer allows you to write data to AnalyticDB in the following two modes:

-   Load Data \(batch import\): Transfers and loads data from the data source to AnalyticDB.
    -   Advantage: Imports a large volume of data \(more than 10 million data records\) at a high speed.
    -   Disadvantage: Authorization from the third party is required.
-   Insert Ignore \(real-time insertion\): Directly writes data to AnalyticDB.
    -   Advantage: Writes a small volume of data \(less than 10 million data records\) at a high speed.
    -   Disadvantage: Unsuitable for writing a large volume of data due to a low speed.

You must configure the data source before configuring the AnalyticDB Writer plug-in. For more information, see [Configure AnalyticDB data source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure AnalyticDB data source.md#).

AnalyticDB Writer supports the following data types in AnalyticDB:

|Type|AnalyticDB data type|
|:---|:-------------------|
|Integer|int, tinyint, smallint, int, bigint|
|Floating point|float and double|
|String type|varchar|
|Date and time|date|
|Boolean|bool|

## Prerequisites {#section_iv2_mzk_q2b .section}

-   Before importing data in Load Data mode with a MaxCompute table as the data source, you must grant the Describe and Select permissions for the table to the import account of AnalyticDB in MaxCompute.

    Public cloud accounts are garuda\_build@aliyun.com and garuda\_data@aliyun.com. Authorization is required for both accounts. For the import accounts of private clouds, see the configuration documents of relevant private clouds. Generally, the import account of a private cloud is test1000000009@aliyun.com.

    Command for granting permissions:

    ```
    USE projectname;--The MaxCompute project to which the table belongs.
    ADD USER ALIYUN$xxxx@aliyun.com;--Enter a correct cloud account (when adding the account for the first time).
    GRANT Describe,Select ON TABLE table_name TO USER ALIYUN$xxxx@aliyun.com; –Enter the table on which permissions are granted and a correct cloud account.
    ```

    To ensure your data security, only the data from the MaxCompute Project in which the operator is the project owner or MaxCompute table owner can be imported to AnalyticDB. Most of private clouds have no such restriction.


## ​Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|url|ADS connection information in the form ip:port.|Yes|N/A|
|schema.|The schema name of the ADS.|Yes|N/A|
|username|The user name of the AnalyticDB account, which is the current AccessID.|Yes|N/A|
|password|The password of the AnalyticDB account, which is the current AccessKey.|Yes|N/A|
|datasource|The data source name. The name entered here must be the same as the added data source. You can add a data source in script mode.|Yes|N/A|
|table.|The name of the target table.|Yes|N/A|
|partition|The partition name of the target table. If the target table is partitioned, this field is required. If the Reader is MaxCompute, and AnalyticDB Writer imports data in Load Data mode, the partitions of MaxCompute only support the following three configurations \(take two-level partitions as an example\):-   "partition" :\[" pt=\\\*, ds=\\\*"\] \(reads data from all partitions under the table\)
-   "partition":\["pt=1,ds=\*"\] \(reads data from all the secondary partitions under the primary partition pt=1 under the table\)
-   "partition":\["pt=1,ds=hangzhou"\] \(reads data from the secondary partition ds=hangzhou under the primary partition pt=1 under the table\)

|No|None|
|Writemode|Supports both the Load Data \(batch import\) and Insert Ignore \(real-time insertion\) modes. If the record with the same primary key already exists, the INSERT IGNORE statement can be successfully executed, but the new record is discarded.|Yes|N/A|
|column|The list of fields in the target table. The value can be \["\*"\] or a list of specific fields, such as \["a", "b", "c"\].|Yes|N/A|
|overWrite|Specified whether to overwrite the current target table when writing data to AnalyticDB. True means the table is overwritten, and False means that the table is not overwritten and the data is appended. This value takes effect only if the writeMode is Load.|Yes|N/A|
|lifeCycle|The life cycle of an AnalyticDB temporary table. This value takes effect only if the writeMode is Load.|Yes|N/A|
|suffix|The AnalyticDB URL is in the format of ip:port, which changes to a JDBC database connection string upon access to AnalyticDB. This parameter is a custom connection string and is optional. See the JDBC control parameters supported by MySQL. For example, configure the suffix to autoReconnect=true&amp;failOverReadOnly=false&amp;maxReconnects=10. Required: No|No|None|
|opIndex|Subscript of the Operation Type column of ADS peer storage, which starts from 0. This value takes effect only if the writeMode is stream.|Required: It is required if the writeMode is Stream.|N/A|
|batchSize|Number of data items of each batch committed to AnalyticDB. This value takes effect only if the writeMode is Insert.|Required: It is required if the writeMode is Insert.|N/A|
|bufferSize|Size of the DataX data buffer. The buffers are aggregated to form a large buffer. The data from the source is collected to this buffer for sorting before being committed to AnalyticDB. The data is sorted by the AnalyticDB partition column so that data is organized in an order that is more friendly for the AnalyticDB server to improve the performance.The data in the buffer with a size of BufferSize is committed to AnalyticDB in batches with a size of batchSize. The bufferSize value must be set to a multiple of batchSize. This value takes effect only if the writeMode is insert.

|Required: It is required if the writeMode is Insert.|Default value: This feature is disabled by default.|

## Introduction to wizard mode {#section_bp2_wsh_p2b .section}

1.  Choose source

    Configure the source and destination of the data for the synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16239/15514319367998_en-US.png)

    Configuration item descriptions:

    -   Data source: The datasource in the preceding parameter description. Enter the configured data source name.
    -   Table: The table in the preceding parameter description. Select the table for synchronization.
    -   Import mode: The writeMode in the preceding parameter description. Load Data \(batch import\) and Insert Ignore \(real-time insertion\) modes are supported.
2.  Field mapping: The column in the preceding parameter description.

    The source table field on the left and the target table field on the right are one-to-one relationships, click **Add row** to add a single field and click **Delete** to delete the current field.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16239/15514319368002_en-US.png)

    -   Peer mapping: Click **peer mapping** to establish a corresponding mapping relationship in the peer that matches the data type.
    -   Automatic formatting: The fields are automatically sorted based on corresponding rules.
3.  Channel control

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15514319367675_en-US.png)

    Configurations:

    -   DMU: A unit that measures the resources consumed during data integration, including CPU, memory, and network bandwidth. It represents a unit of data synchronization processing capability given limited CPU, memory, and network resources.
    -   Concurrent count: The maximum number of threads used to concurrently read or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
    -   Number of error records: The maximum number of dirty data records.

## Development in script mode {#section_cp2_wsh_p2b .section}

```
{
    "type":"job",
    "version":"2.0",
    "steps":[// below is the template for reader, you can find the appropriate read plug-in documentation.
        {
            "stepType":"stream ",
            "parameter":{
            "name":"Reader ",
            "category":"reader"
        },
        {
            "stepType":"ads", // plug-in name
            "parameter":{
                "partition:" ", // partition name of the target table
                "datasource": "", //Data Source
                "column":[// Field
                     "id"
                ],
                "writeMode":"insert",//Write mode
                "batchSize":"1000", // number of records submitted in one batch size
                "table":"",//The name of the target table.
                "overWrite": "true" // ADS write whether or not to override the currently written table, true is an overlay write, and false is a non-override (append) Write. This value takes effect only if the writeMode is Load.
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit": {
            "record":"0"//Number of error records
        },
        "speed": {
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

