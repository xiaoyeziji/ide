# Configure OpenSearch Writer {#concept_kwq_vnt_q2b .concept}

This topic describes data types and parameters supported by OpenSearch Writer and how to configure Writer in both wizard and script mode.

The OpenSearch Writer plug-in is designed to insert or update data in OpenSearch. Data developers can use it to import processed data in OpenSearch and output data by searching. How fast data can be transmitted depends on Queries per second \(QPS\) of the account corresponding to the OpenSearch table.

Implementation

At the underlying implementation level, OpenSearch Writer provides the openly available OpenSearch API by means of OpenSearch.

-   OpenSearch V3 uses internal dependent databases, with the following POM: com.aliyun.opensearch aliyun-sdk-opensearch 2.1.3.

**Note:** 

-   To use the OpenSearch Writer plug-in, you must use JDK 1.6-32 or later versions. You can view the Java version through java -version.
-   Currently, the default resource group does not support connections to the VPC environment because of possible network problems.

## Plug-in features {#section_jn2_gqh_p2b .section}

Column order

The columns in OpenSearch are unordered, so you should use OpenSearch Writer to write data in strict accordance with the specified column order. If the number of specified columns is less than that in OpenSearch, the redundant columns are set to the default value or null.

For example, if the imported field list contains fields b and c, but the OpenSearch table contains the fields a, b, and c, you can configure the column to "column": \["c","b"\]. The first two columns in Reader are imported to fields c and b in OpenSearch, and the field a, into which new records are inserted is set to the default value or null.

-   **How to handle column configuration errors**

    To ensure the data written is reliable, OpenSearch writer prevents data loss from redundant columns that can lead to data quality failure. When redundant columns are written, OpenSearch Writer generates an error.- If the OpenSearch table contains fields a, b, and c, OpenSearch Writer generates an error when more than three fields are written by OpenSearch Writer.

-   **Table configuration considerations**

    The OpenSearch Writer can only write data from one table at a time.

-   **Rerun task and failover:**

    After a task is rerun, the data is automatically overwritten based on the ID. Therefore, OpenSearch must contain one ID column. The ID uniquely identifies a record line in OpenSearch. The data is the same as the overwritten unique ID .

-   **Rerun task and failover:**

    After one task is rerun, the data is automatically overwritten based on the IDs.


OpenSearch Writer supports most OpenSearch data types. Check whether the data type is supported. OpenSearch Writer converts data types in OpenSearch as follows:

|Category|Opensearch data type|
|:-------|:-------------------|
|Integer|Int|
|Float point|Double/Float|
|String type|TEXT/Literal/SHORT\_TEXT|
|Date and time type|Int|
|Boolean|Literal|

## Parameter description​ {#section_amt_td4_q2b .section}

|Attribute|Description| Required|Default value|
|:--------|:----------|:--------|:------------|
|accessId|The Logon ID for Alibaba Cloud.|Yes|None |
|accessKey|The Logon Key for Alibaba Cloud.|Yes|None |
|host| |Yes|None |
|indexName|The name of the OpenSearch project.|Yes|None |
|table|The table for which the data is written. You cannot enter more than one table because DataX does not support importing multiple tables simultaneously.|Yes|None |
|column|The list of fields imported. If you need to import all the fields, it can be configured to "column": \["\*"\]. Enter specified columns, if you need to insert some OpenSearch columns. For example: "column": \["id", "name"\]. OpenSearch supports column filtering and column order changes. For example, a table has three fields: a, b, and c, and only fields c and b need to be syncrhonized. You can configure the fields to \["c, b"\]. During the import process, the field a is automatically inserted with null values and set to null.|Yes|None |
|batchSize|The number of data lines written in a single note. Data is written into OpenSearch in batches. In general, the advantage of OpenSearch is query, and its write performance Transactions per seconds \(TPS\) is unimpressive. Proceed with the configuration based on the resources applied with your account. For OpenSearch, a single data item size is generally less than 1 MB, and the size of each data entry written is less than 2 MB.|This field is required for a partition table, but is not required if the target table is a non-partition table.|300|
|writeMode|In OpenSearch Writer, "writeMode": "add/update" is configured to ensure the idempotence of write operations.-   -"add": When a reattempt is made after a failed write attempt, OpenSearch Writer cleans up this data and imports new data \(atomic operation\).
-   -"update": It indicates the data is inserted in a modified manner \(atomic operation\).

**Note:** 

In OpenSearch, batch insert is not an atomic operation, which may be partially successful. Therefore, writeMode is a critical option. OpenSearch with version=v3 does not support the update operation currently\*\*.


|Yes|None |
|ignoreWriteError|This parameter ignores write errors.The following is a configuration example: "ignoreWriteError": true. OpenSearch performs write operations in batches. If ignoreWriteError is enabled, all write failures are ignored and other write operations are continued . If this parameter is disabled, when a write failure occurs the task ends, and an error is returned. The default value is recommended.

|No|false|
|version|The OpenSearch version information. The following is a configuration example: "version": "v3". OpenSearch V2 has multiple limitations for push operations, so OpenSearch V3 is preferable.|No|v2|

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

