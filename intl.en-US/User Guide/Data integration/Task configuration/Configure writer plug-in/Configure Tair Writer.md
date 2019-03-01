# Configure Tair Writer {#concept_ynf_pzm_q2b .concept}

This topic describes the data types and parameters supported by Tair Writer and how to configure the Writer in both Wizard and Script mode.

## DataX3.0 Tair Writer {#section_i4h_2d4_q2b .section}

For more information about DataX2 tasks, see Tair configuration section in the Tair Writer in the synchronization center topic. For more information about the relationship between DataX2 and Tair configuration, see the last section of this document.

Tair is a high-performance, distributed, scalable and highly reliable NoSQL storage system. It supports Java, C/C++, and RESTful clients.

Message-Driven Bean \(MDB\) and Logic Data Bank \(LDB\) storage engines are available in the background. The MDB is a memory KV database similar to Memcache. The LDB is a LevelDB-based permanent storage engine.

Tair Writer imports data by writing data in MDB or LDB through universal interfaces. Use fastdump if you need to import large volume of data from ODPS-\> tair.

## Implementation {#section_jn2_gqh_p2b .section}

Tair Writer writes data read by the reader in Tair. The records are written one-by-one through four native API interfaces in a Tair client: PUT, prefixPuts, setCount, and prefixSetCounts.

In PUT and Counter modes, data is input in key-value pair format. Counter values must be non-negative integers. The pkey, skey, and value \(pkey + skey -\> value\) are input in prefixPuts and prefixCounters modes. The prefixCounter values must be non-negative integers.

## Type conversion {#section_axy_qd4_q2b .section}

Tair is a weak NoSQL database. Although, Java clients of Tair allow data serialization to be written as original, the system converts all data into strings, and then imports them to Tair in C++ and Java format.

## Parameter description {#section_amt_td4_q2b .section}

NOTE: To avoid parameter errors, do not set default values for any parameters. Required \(Yes\) parameters must be set. Some parameters can be set only in specific write modes \(six modes\). For example, required PUT parameters are required in PUT mode, which means these parameters can only be set in PUT mode. The system reports errors if the parameters set exceed or are less than needed.

|Parameter|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|languages|This parameter indicates the programming language used to interact with a Tair client, and currently C++ and Java languages are supported. You can specify this field based on the used Tair client. Tair and C++ clients are incompatible with Java clients in data writing. For example, after data is written into a target Tair system through the DataX TairWriter plug-in to access the table by using a Tair C++ client based on the subsequent business code, and enter "C++" to access the table by using a Tair Java client. Enter "java" to access the table by using a RESTful client.|Yes. You must enter either "C++" or "java"， which is case-insensitive.|None|
|Fielddelimiterc|Tair Writer writes data in tables in PUT mode and concatenates all non-key fields except for the first columns by using characters corresponding to the fieldDelimiter. Ensure that the fields corresponding to fieldDelimiter are excluded from your data. Do not use common characters, such as commas \(,\).|Mandatory in PUT mode.|None|
|skeyList|Tair Writer writes data in multiprefixput or multiprefixcounter mode. To specify a list of skey fields written, use a JavaScript Object Notation \(JSON\) list. Note: The number of columns extracted from the reader here should be less than 1 because the first column is a pkey.|Required in mutilprefixput/multiprefixcounter mode.|None|
|expire|The expiration time. If the value is 0, the data is never deleted. The expiration time is a duration in which data is stored in a Tair instance in seconds.|Yes|None|
|compressionThreshold|The compression threshold value, when a compressed Tair value exceeds the threshold size. This option sacrifices the CPU in exchange for network transmission efficiency and storage space. By default, the compressed data size is greater than 8 KB. You can raise the threshold, when bad machine performance generates many compressed logs that results in high DataX machine load and request timeout. Because C++ client does not support compression, you need to disable the compression function by setting the threshold value to 1,000,000. This threshold value setting is close to the maximum data size of 1 MB supported by the Tair client.|This is required when Java language is used.|None|
|deleteEmptyRecord|The method for processing null values in PUT mode. The key and value cannot be null in a Tair instance, with the exception of column 0 set as key. The other columns cannot all be null. If all values are null, the data will be marked as dirty data and skipped. However, if you set deleteEmptyRecord to true. All null data triggers the Delete operation so corresponding keys are deleted from the Tair instance. This is only valid in PUT mode.|This is required when the write mode is PUT.|None|
|timeout|The time-out for each request of the built-in Tair client, when the network connection is poor. To avoid too many timeout failures, the value must be configured to large.|No, the unit is milliseconds, and the default value is 2000 ms.|None|
|frontLeadingKey|Suffixes of all keys or pkeys are read from the source and then writes them in a Tair instance.|None|None|

-   frontLeadingKey

    -   Description: Suffixes of all keys or pkeys read from the source and then writes them in a Tair instance.
    -   Required: No
    For example:

    |ID|Name|Age|
    |:-|:---|:--|
    |1|lilei|24|

-   Writer type
    -   Description: Tair Writer supports the preceding six write modes. You need to select a mode based on the business demand. The following describes all writing modes because data is stored in key/value form rather than in table mode in a Tair instance. We set the first field of each record as the \(master\) key.
    -   Required: You must select PUT, prefixput, multiprefixput, COUNTER, prefixcounter, or multiprefixcounter.

1. In PUT mode, the first field of the source table is used as the Tair table key, and the other fields are concatenated in order into a string, and then the string is written. The concatenation connectors are characters specified based on fieldDelimiter configuration. For example:

|ID|Name|Age|
|::|:--:|:-:|
|1|lilei|24|

In PUT mode, the key is "1" and the value is "lilei,24" \(the "," is a configured delimiter\).

2. In prefixput mode, the data lines in a 3 \(column\) x N \(line\) table are written one-by-one through a prefixput interface. In this mode, a table must have three columns, where the columns are the pkey, skey, and value, respectively. In addition, you can convert a sparse table into a three-column table and store it in a Tair instance. This mode is also applicable to scenarios in which custom skeys are required.

For example:

|ID|Name|Age|
|:-|:---|:--|
|1|20150101|hanmeimei|

The pkey is "1", skey is "20150101", and the value is "hanmeimei".

3. In multiprefixput mode, M x N values in an M \(column\) x N \(line\) two-dimensional table are imported to a Tair instance, that is, the imported values can be found in the Tair instance through the line ID \(pkey\) and column name \(skey\). Data is written through a prefixPuts interface with the first field as pkey, the column name of the second field as skey, and the value of the second column as value.

For example, for a data line:

|ID|Name|Age|
|:-|:---|:--|
|1|lilei|24|

In multiprefixput mode, two skey values are written under one pkey value.

pkey-\>1, skey-\>name, value-\>lilei

pkey-\>1, skey-\>age, value-\>24

4. The Counter mode: The data imported to a Tair instance in the preceding modes is strings. Tair also supports Counter data. After Counter data is imported, Increase/Decrease operation can be performed through the incr/decr interface. This data is also supported by common counters and prefix counters. A Counter supports only two columns of data as follows:

|Page|PV|
|:---|:-|
|www.taobao.com|1000|

Write to Tair is key-\> page\_a'pv value-\> 100. Both negative and non-numeric values are skipped and classified as dirty data, and the key can only have 2 columns.

5. The prefixcounter mode is similar to the prefixput mode, in which three columns of data in a table are imported as pkey, skey, and value \(count\), respectively. For example:

|Data|Page|PV|
|:---|:---|:-|
|2015-03-20 18:00:00|www.taobao.com|1000|

The pkey is "2015-03-20 18:00:00", skey is "www.taobao.com", and value is "1000".

6. The chain pattern is similar to the row ID where the first column of each row is pkey, respectively, the 2, 3..N column per line. The N column names are respectively skey, with each row 2, 3...N column. The values of the N column are as value \(count\). For example:

|taobaoid|Age|Money|
|:-------|:--|:----|
|111xxx3|24|1000|

Two data pieces are written into a Tair instance:

pkey-\>1112223, skey-\>age, value-\>24

pkey-\>1112223, skey-\>money, value-\>1000

## Introduction to Wizard Mode {#section_cp2_wsh_p2b .section}

1. Synchronization configuration: Data source and destination.

![](images/8319_en-US.jpg)

Parameters:

-   Data source: The datasource in the preceding parameter description. Enter the configured data source name .
-   Import mode: The import mode is writerType.
-   Cluster configId: The configId in the preceding parameter description.
-   Cluster space number: The namespace in the preceding parameter description.
-   Threshold for automatic value compression: The compression threshold in the preceding parameter description.
-   Expiration time: The expire in the preceding parameter description.

2. Map fields:

![](images/8321_en-US.png)

NOTE: If the source or destination has a Lindon, HBase, or Tair data source, the fields do not require lines and can be edited and saved.

-   Manually edit source table field: Manually edit fields where each line indicates a field. The first and end blank lines are ignored.

3. Channel control

![](images/8322_en-US.png)

-   JVM parameter: A unit that measures the resources consumed during data integration, including CPU, memory, and network bandwidth. A JVM is the minimum operation capacity unit in a Data Integration task. It represents a unit of data synchronization processing capability given limited CPU, memory, and network resources.
-   Concurrent job count: The maximum number of threads used to concurrently read/write data into the data storage media in a data synchronization task. In Wizard Mode, configure a concurrency for the specified task on the wizard page.
-   The maximum number of errors indicates the maximum number of dirty data records.

## Introduction to Script Mode {#section_hy1_dys_q2b .section}

The following is an example of script configuration. For more information about parameters, see Parameter Description.

```
{
    "type": "job",
    "version": 2.0 ", // version number
    "steps":[
        {
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType": "Tair", // plug-in name
            "parameter":{
                "skeyList":[
                    "seller_nick", 
                    "lq3d_rate_n14d"
                ],
                "isFullImport":"0",
                "language": "java", //client language 
                "fieldDelimiter": "," //Delimiter of each column 
                "userName": "", 
                "frontLeadingKey": "",
                "timeout": "2000", // timeout 
                "compressionThreshold": “128”, // value automatic to compress threshold 
                "configId": "counter", // cluster configid 
                "expire": 19000, // expiration time 
                "namespace": "", // cluster space number 
                "writerType": "put", // import Mode 
                "deleteEmptyRecord": "false" 
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "jvmOption": "-Xms1024m -Xmx1024m",//JVM memory
        "errorLimit": {
            "record": 2315467 "// Number of error records
        },
        "speed": {
            "concurrent":29,
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
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

## Plug-in features {#section_im5_vjt_q2b .section}

-   Write operation in a Tair instance is idempotent. Previous data is overwritten to prevent redundant Write operations.
-   Task rerun and failover: Because the Write operation in a Tair instance is idempotent, failover for a single task is supported. That is, if Write operation fails, DataX restarts subtasks to retry \(a maximum of 10 retries supported\). However, the system retries only in PUT and counter modes.
-   Data limit: Indicates the length limit of the key and value data in a Tair instance. A key contains 1 to 1024 bytes, and a value contains 2 to 1,000,000 bytes. Data that exceeds the length limit is processed as dirty data. Therefore, this data needs to be intercepted through the filter function in advance.

