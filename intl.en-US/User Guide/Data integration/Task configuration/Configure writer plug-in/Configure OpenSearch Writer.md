# Configure OpenSearch Writer {#concept_kwq_vnt_q2b .concept}

In this article we will show you the data types and parameters supported by OpenSearch Writer and how to configure Writer in both wizard mode and script mode.

The OpenSearch Writer plug-in is designed to insert or update data into OpenSearch. Data developers can use it to import processed data into OpenSearch and output data by searching. How fast data can be transmitted depends on the qps of the account corresponding to the OpenSearch table.

Implementation

At the underlying implementation level, OpenSearch Writer provides the openly available OpenSearch API by means of OpenSearch.

-   OpenSearch V3 uses internal dependent databases, with POM of com.aliyun.opensearch aliyun-sdk-opensearch 2.1.3.

**Note:** 

-   To use the OpenSearch Writer plug-in, you must use JDK 1.6-32 or later versions. You can view the Java version through java -version.
-   Currently, the default resource group does not support connections to the VPC environment due to possible network problems.

## Plug-in features {#section_jn2_gqh_p2b .section}

Column order

The columns in OpenSearch are unordered, so you should use OpenSearch Writer to write data in strict accordance with the order of the specified columns. If the number of specified columns is less than that in OpenSearch, the redundant columns are set to the default value or null.

For example, if the field list to be imported contains fields b and c but the OpenSearch table contains fields a, b, and c, you can configure the column to "column": \["c","b"\]. The first two columns in Reader are imported to fields c and b in OpenSearch, and the field a, into which new records are inserted, is set to the default value or null.

-   **How to handle column configuration errors**

    To ensure data is written in a reliable manner, data loss from redundant columns must be prevented to avoid data quality failure. When redundant columns are written, OpenSearch Writer produces an error.- If the OpenSearch table contains fields a, b, and c, OpenSearch Writer produces an error when more than three fields are written by OpenSearch Writer.

-   **Table configuration considerations**

    OpenSearch Writer can only write the data from one table at a time.

-   **Task rerunning and failover:**

    After one task is rerun, the data is automatically overwritten based on IDs. Therefore, OpenSearch must contain one ID column. The ID uniquely identifies a record line in OpenSearch. The data same as the unique ID will be overwritten.

-   **Task rerunning and failover:**

    After one task is rerun, the data is automatically overwritten based on IDs.


OpenSearch Writer supports most data types in OpenSearch. Check whether your data type is supported. OpenSearch Writer converts the data types in OpenSearch as follows:

|Category|Opensearch Data Type|
|:-------|:-------------------|
|Integer|Int|
|Float point|Double/Float|
|String type|TEXT/Literal/SHORT\_TEXT|
|Date and time type|Int|
|Boolean|Literal|

## Parameter description​ {#section_amt_td4_q2b .section}

|Attribute|Description| Required|Default Value|
|:--------|:----------|:--------|:------------|
|accessId|Description: Logon ID for the Alibaba Cloud system.|Yes|None |
|accessKey|Description: Key for the Alibaba Cloud system.|Yes|None |
|host| |Yes|None |
|indexName|Description: The name of the OpenSearch project.|Yes|None |
|table|Description: The table to which the data is written. You cannot enter more than one table, because DataX does not support importing multiple tables at a time.|Yes|None |
|column|Description: The list of fields to be imported. If you need to import all the fields, it can be configured to "column": \["\*"\]. If you need to insert some of the OpenSearch columns, enter these columns, for example, "column": \["id", "name"\]. OpenSearch supports column filtering and column order changing. For example, a table has three fields: a, b, and c, and you only want to synchronize fields c and b. You can configure it to \["c, b"\]. During the import process, field a is automatically inserted with null values and set to null.|Yes|None |
|batchSize|The number of data lines written in a single note. Data is written into OpenSearch in batches. In general, the advantage of OpenSearch is query, and its write performance \(tps\) is not impressive. Proceed with the configuration based on the resources applied by your account. For OpenSearch, generally, a single item of data is less than 1 MB, and the data to be written at a time is less than 2 MB.|Required: This option is required for a partition table. Do not enter this field if the target table is not a partition table.|300|
|writeMode|Description: In OpenSearch Writer, "writeMode": "add/update" is configured to ensure the idempotence of write operations.-   -"add": When a reattempt is made after a failed write attempt, OpenSearch Writer cleans up this data and imports the new data \(atomic operation\).
-   -"update": It indicates that the data is inserted in a modified manner \(atomic operation\).

**Note:** 

In OpenSearch, batch insert is not an atomic operation, which may be partially successful. Therefore, writeMode is a critical option. OpenSearch with version=v3 does not support the update operation currently\*\*.


|Yes|None |
|ignoreWriteError|Description: Ignores write errors.Configuration example: "ignoreWriteError": true. OpenSearch write operations are performed in batches. It indicates whether to ignore the write failure occurred in the current batch. If yes, other write operations keep going. If no, the current task is ended, and an error is returned. The default value is recommended.

|No|false|
|version|Description: The version information of OpenSearch. Configuration sample: "version": "v3". OpenSearch V2 has multiple limitations on push operations, so OpenSearch V3 is preferable.|No|v2|

## Development in script mode {#section_hy1_dys_q2b .section}

Configure the data synchronization job to write data to OpenSearch:

```language-json
{
    "type": "job",
    "version": "1.0",
    "configuration": {
        "reader": {},
        "writer": {
            "plugin": "opensearch",
            "parameter": {
                "accessId": "*********",
                "accessKey": "********",
                "host": "http://yyyy.aliyuncs.com",
                "indexName": "datax_xxx",
                "table": "datax_yyy",
                "column": [
                "appkey",
                "id",
                "title",
                "gmt_create",
                "pic_default"
                ],
                "batchSize": 500,
                "writeMode": add,
                "version":"v2",
                "ignoreWriteError": false
            }
        }
    }
}
```

