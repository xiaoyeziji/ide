# Configure OpenSearch Writer {#concept_kwq_vnt_q2b .concept}

This topic describes data types and parameters supported by OpenSearch Writer and how to configure Writer in both Wizard and Script mode.

The OpenSearch Writer plug-in is designed to insert or update data in OpenSearch. Data developers can use it to import processed data in OpenSearch and output data by searching. How fast data can be transmitted depends on the account Queries per second \(QPS\) that corresponds to the OpenSearch table.

Implementation

At the underlying implementation level, OpenSearch Writer provides the publicly available OpenSearch API through OpenSearch.

-   OpenSearch v3 uses internal dependent databases, with the following POM: com.aliyun.opensearch aliyun-sdk-opensearch 2.1.3.

**Note:** 

-   To use the OpenSearch Writer plug-in, you must use JDK 1.6-32 or later versions. You can view the Java version through java-version.
-   Currently, the default resource group does not support connections to the VPC environment because of potential network problems.

## Plug-in features {#section_jn2_gqh_p2b .section}

Column order

Because the columns in OpenSearch are unordered,you need to use OpenSearch Writer to write data in strict compliance with the specified column order. If the number of specified columns are less than those in OpenSearch, the redundant columns are set to the default value or null.

For example, if the imported field list contains fields b and c, but the OpenSearch table contains the fields a, b, and c. You can configure the column to "column": \["c","b"\]. The first two columns in Reader are imported to fields c and b in OpenSearch, and the field a, in which new records are inserted is set to the default value or null.

-   **How to handle column configuration errors**

    To ensure data written is reliable, OpenSearch Writer prevents data loss caused by redundant columns that can lead to data quality failure. OpenSearch Writer reports an error when redundant columns are written.- If the OpenSearch table contains fields a, b, and c, the OpenSearch Writer generates an error when more than three fields are written by OpenSearch Writer.

-   **Table configuration precautions**

    The OpenSearch Writer can only write data from one table at a time.

-   **Rerun task and failover:**

    After the task is rerun, the data is automatically overwritten by the ID. Therefore, OpenSearch must contain one ID column. The ID uniquely identifies a record line in OpenSearch. The data is the same as the overwritten unique ID.

-   **Rerun task and failover:**

    After a task is rerun, the data is automatically overwritten by the IDs.


OpenSearch Writer supports most OpenSearch data types. Check whether the data type is supported. OpenSearch Writer converts data types in OpenSearch as follows:

|Category|Opensearch data type|
|:-------|:-------------------|
|Integer|Int|
|Float point|Double/Float|
|String type|TEXT/Literal/SHORT\_TEXT|
|Date and time type|Int|
|Boolean|Literal|

## Parameter description​ {#section_amt_td4_q2b .section}

|Parameter|Description| Required|Default value|
|:--------|:----------|:--------|:------------|
|accessId|The Logon ID for Alibaba Cloud.|Yes|None |
|accessKey|The Logon Key for Alibaba Cloud.|Yes|None |
|host| |Yes|None |
|indexName|The name of the OpenSearch project.|Yes|None |
|table|The table for which the data is written into. You cannot enter more than one table because DataX does not support importing multiple tables simultaneously.|Yes|None |
|column|The list of imported fields. If you need to import all fields, it can be configured to "column": \["\*"\]. Enter the specified columns, if you need to insert OpenSearch columns. For example: "column": \["id", "name"\]. OpenSearch supports column filtering and column order changes. For example, you can configure the fields to \["c, b"\], if a table has three fields: a, b, and c, and only fields c and b must be synchronized. . The field a is automatically inserted with null values and set to null during import.|Yes|None |
|batchSize|The number of data lines written in a single entry. Data is written into OpenSearch in batches. Typically, the advantage of OpenSearch is query, but has low write Transactions per seconds \(TPS\) performance. Proceed with the configuration based on the resources applied to your account. For OpenSearch, a single data item size is generally less than 1 MB, and the size of each written data entry is less than 2 MB.|This field is required for a partition table, but is not required if the target table is a non-partition table.|300|
|writeMode|In OpenSearch Writer, "writeMode": "add/update" is configured to ensure the idempotence of write operations.-   -"add": When a reattempt is made after a failed write attempt, OpenSearch Writer clears the data and imports new data \(atomic operation\).
-   -"update": It indicates the data is inserted in a modified manner \(atomic operation\).

**Note:** 

Because batch insert is not an atomic operation in OpenSearch, it may be partially successful. Therefore, writeMode is a key option and currently OpenSearch with version=v3 does not support update\*\*.


|Yes|None |
|ignoreWriteError|This parameter ignores write errors.The following is an example of the configuration: "ignoreWriteError": true. OpenSearch performs batch write operations . When ignoreWriteError is enabled, all write failures are ignored, but continues other write operations. When this parameter is disabled, an error is returned when a write failure occurs and the task ends. We recommend you use the default value: False, for configuration.

|No|False|
|version|The OpenSearch version information. The following is a configuration example: "version": "v3". OpenSearch v3 is more preferable because OpenSearch v2 has multiple push operation limitations.|No|v2|

## Development in Script Mode {#section_hy1_dys_q2b .section}

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

