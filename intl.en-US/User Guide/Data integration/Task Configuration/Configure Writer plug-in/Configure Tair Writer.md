# Configure Tair Writer {#concept_ynf_pzm_q2b .concept}

In this article we will show you the data types and parameters supported by Tair Writer and how to configure Writer in both wizard mode and script mode.

## DataX3.0 tairwriter {#section_i4h_2d4_q2b .section}

For details about DataX2 tasks, see Tair Writer of the synchronization center and read the section about Tair configuration. For details about the relationship between DataX2 and Tair configuration, see the last part of the document.

Tair is a high-performance, distributed, scalable and highly reliable NoSQL storage system. It supports Java, C/C++, and RESTful clients.

Message Driven Bean \(MDB\) and Logic Data Bank \(LDB\) storage engines are available in the background. MDB is a memory KV database similar to MemCache. LDB is a LevelDB-based permanent storage engine.

TairWriter imports data by writing data in MDB or LDB through universal interfaces. Use fastdump if there is a need to import large amounts of data from ODPS-\> tair.

## Implementation {#section_jn2_gqh_p2b .section}

TairWriter writes data read by the reader in Tair in the unit of records one by one through four native API interfaces of a Tair client: put, prefixPuts, setCount, and prefixSetCounts.

In put and counter modes, data is input in the form of key-value pair. Counter values must be non-negative integers.  pkey, skey, and value \(pkey + skey -\> value\) are input in prefixPuts and prefixCounters modes. prefixCounter values must be non-negative integers.

## Type conversion {#section_axy_qd4_q2b .section}

Tair is not a strong NoSQL database. Although Java clients of Tair allow serializable data to be written as original, the system converts all data into strings and then imports them to Tair \(including C++ and Java\).

## Parameter description {#section_amt_td4_q2b .section}

NOTE: To avoid parameter errors, do not set default values for any parameter. Required \(Yes\) parameters must be set. Some parameters need to be set only in specific write modes \(six modes\); for example, required \(put\) parameters are required in put mode, which means such parameters need to be set only in put mode. The system reports errors if you set more or less parameters than needed.

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|languages||language|It indicates the language used to interact with a Tair client. Currently, C++ and Java languages are supported. Specify this field based on the Tair client used \(Tair C++ clients are incompatible with Java clients in data writing\). For example, after data is written into a target Tair system by using the DataX TairWriter plug-in, to access the table by using a Tair C++ client based on the subsequent business code, enter "c++"; to access the table by using a Tair Java client, enter "java". To access the table by using a RESTful client, enter "java".|Yes. You must enter either "c++" or "java" \(case-insensitive\).|None|
|Fielddelimiterc|TairWriter writes data in tables in put mode and concatenates all non-key fields \(except for the first columns\) by using characters corresponding to fieldDelimiter. Ensure that the fields corresponding to fieldDelimiter are not included in your data. Do not use common characters, such as commas.|Mandatory in put mode|None|
|skeyList||skeyList|TairWriter writes data in multiprefixput or multiprefixcounter mode. To specify a list of fields written as skey, use a JavaScript Object Notation \(JSON\) list. Note: the number of columns here should be 1 less than the number of columns taken out by reader \(because the first column is as pkey \).|Required In maid/maid mode.|None|
|expire|Indicates the expiration time. If the value is 0, data is never deleted. The expiration time is a duration in which data is stored in a Tair instance, in the unit of seconds.|Yes|None|
|compressionThreshold|Compression threshold, one of the value in Tair is compressed if the threshold size is exceeded, this is an option to sacrifice the CPU in exchange for network transmission efficiency and storage space. By default, data is compressed if the size exceeds 8 KB. If many compressed logs exist due to the bad performance of your machine and thereby results in high load of the DataX machine and even request timeout, you can increase the threshold. In addition, if you use a C++ client, as it does not support compression, you need to disable the compression function by setting the threshold to 1000000 because a Tair client supports a maximum data size of 1 MB.|Mandatory when the Java language is used.|None|
|deleteEmptyRecord|Method for processing null values in put mode. Keys and values cannot be null in a Tair instance; that is, except for column 0 as a key, the other columns cannot all be null. If all values are null, the data is skipped as dirty data. However, if you set deleteEmptyRecord to true, all null data triggers the Delete operation so that corresponding keys are deleted from the Tair instance.\( Valid only for put Mode\)|Required when write mode is put.|None|
|timeout|The time-out for each request of the built-in Tair client, if the network is not good, to avoid too many time-out failures, you need to be properly configured to be large.|No, the unit is milliseconds, and the default 2000 MS.|None|
|frontLeadingKey| Suffixes all keys or pkeys read from the source and then writes them in a Tair instance|None|None|

-   frontLeadingKey

    -   Description: Suffixes all keys or pkeys read from the source and then writes them in a Tair instance.
    -   Required: No
    For example:

    |id|name|age|
    |:-|:---|:--|
    |1|lilei|24|

-   Writer type
    -   Description: TairWriter supports the preceding six write modes. You need to select a mode based on the business demand. The following describes all the modes. As data is stored in key or value form rather than in table mode in a Tair instance, we set the first field of each record as the \(master\) key.
    -   Mandatory: Yes; you must select put, prefixput, multiprefixput, counter, prefixcounter, or multiprefixcounter.

1. In put mode, the first field of the source table is used as a key of the Tair table, the other fields are concatenated in order into a string, and the string is written in put mode. The concatenation connectors are characters specified based on fieldDelimiter configuration. For example:

|id|name|age|
|::|:--:|:-:|
|1|lilei|24|

In put mode, the key is "1" and the value is "lilei,24" \("," is a delimiter configured by you\).

2. In prefixput mode, data lines in a 3 \(column\) x N \(line\) table are written one by one through a prefixput interface; three columns of each row are used as pkey, skey, and value respectively. That is, in this mode, a table must have three columns with the first column as pkey, the second as skey, and the third as value. In addition, you can convert a sparse table into a three-column table and store it in a Tair instance. This mode is also applicable to scenarios in which custom skeys are required.

For example,

|id|Name|age|
|:-|:---|:--|
|1|20150101|hanmeimei|

pkey is "1", skey is "20150101", and value is "hanmeimei".

3. In multiprefixput mode, M x N values in an M \(column\) x N \(line\) two-dimensional table are imported to a Tair instance; that is, the imported values can be found in the Tair instance through the line ID \(pkey\) and column name \(skey\). Data is written through a prefixPuts interface with the first field as pkey, the column name of the second field as skey, and the value of the second column as value.

For example, for a line of data:

|id|name|age|
|:-|:---|:--|
|1|lilei|24|

In multiprefixput mode, two skey values are written under one pkey value.

pkey-\>1, skey-\>name, value-\>lilei

pkey-\>1, skey-\>age, value-\>24

4. Counter mode: Data imported to a Tair instance in the preceding modes is strings. Tair also supports counter data. After counter data is imported, Increase/Decrease operation can be performed through the incr/decr interface. Such data is also supported by common counters and prefix counters. A counter supports only two columns of data, for example,

|Page|pv|
|:---|:-|
|www.taobao.com|1,000|

Write to Tair is key-\> page\_a'pv value-\> 100 \(both negative and non-numeric are skipped as dirty data\), and must have only 2 columns \).

5. The prefixcounter mode is similar to the prefixput mode, in which three columns of data in a table are imported as pkey, skey, and value \(count\) respectively, for example:

|data|Page|pv|
|:---|:---|:-|
|2015-03-20 18:00:00|www.taobao.com|1,000|

pkey is "2015-03-20 18:00:00", skey is "www.taobao.com", and value is "1000".

6. The chain pattern is similar to the row ID \(the first column of each row\) as pkey, respectively, 2nd, 3 .. per line .. The column names of the n columns are respectively skey, with each row 2nd, 3 .. The values of the N column are as value \(count\), for example:

|taobaoid|age|money|
|:-------|:--|:----|
|111xxx3|24|1,000|

Two pieces of data are written into a Tair instance:

pkey-\>1112223, skey-\>age, value-\>24

pkey-\>1112223, skey-\>money, value-\>1000

## Introduction to wizard mode {#section_cp2_wsh_p2b .section}

1. Synchronization configuration: Data source and destination.

![](images/8319_en-US.jpg)

Parameters:

-   Data source: The datasource in the preceding parameter description. Enter the data source name you configured.
-   Import mode: writerType
-   Cluster configId: configId in the preceding parameter description
-   Cluster space number: namespace in the preceding parameter description
-   Threshold for automatic value compression: compressionThreshold in the preceding parameter description
-   Expiration time: expire in the preceding parameter description

2. Map fields:

![](images/8321_en-US.png)

NOTE: If the source or destination end has a Lindon, HBase, or Tair data source, the fields do not need to be lined and can be edited and saved directly.

-   Manually edit source table field: Manually edit the fields. Each line indicates a field. The first and end blank lines are ignored.

3. Channel control

![](images/8322_en-US.png)

-   JVM parameter: A unit which measures the resources \(including CPU, memory, and network bandwidth\) consumed during data integration. A JVM is the minimum operation capacity unit in a Data Integration task. It represents a unit of data synchronization processing capability given limited CPU, memory, and network resources.
-   Concurrent job count: Maximum number of threads used to concurrently read data from or write data into the data storage media in a data synchronization task. In wizard mode, configure a concurrency for the specified task on the wizard page.
-   The maximum number of errors indicates the maximum number of dirty data records.

## Introduction to script mode {#section_hy1_dys_q2b .section}

The following is a script configuration sample. For details about parameters, see Parameter Description.

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

-   Write operation in a Tair instance is idempotent. Data previously written is overwritten in case of multiple Write operations.
-   Task re-run and failover: As Write operation in a Tair instance is idempotent, failover for a single task is supported.  That is, if Write operation fails, DataX restarts subtasks to retry \(a maximum of 10 retries supported\). However, the system retries only in put and counter modes.
-   Indicates the length limit for key and value data in a Tair instance. A key contains 1 to 1,024 bytes, and a value contains 2 to 1,000,000 bytes. Data exceeding the length limit is processed as dirty data. Therefore, such data needs to be intercepted through the filter function in advance.

