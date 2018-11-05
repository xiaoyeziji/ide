# Configure Memcache \(OCS\) Writer {#concept_b31_vtm_q2b .concept}

In this article we will show you the data types and parameters supported by Memcache \(OCS\) Writer and how to configure Writer in script mode.

ApsaraDB for Memcache \(formerly known as OCS\) is a seamlessly scalable distributed memory database service with high performance and reliability. Based on the Apsara distributed system and high performance storage, ApsaraDB for Memcache provides a complete set of solutions for master/slave hot standby, disaster recovery, business monitoring, data migration, and other scenarios.

ApsaraDB for Memcache supports the out-of-the-box deployment mode, and relieves the database load for dynamic web applications using the cache service, thus accelerating the overall response of the website.

Similar to the local Memcache databases, ApsaraDB for Memcache is compatible with the Memcached protocol. You can use it directly in your operating environment. The difference is that the hardware and data of ApsaraDB for Memcache are deployed in the cloud, providing complete infrastructure, network security, and system maintenance services. All these services are billed on a Pay-As-You-Go basis.

Memcache Writer writes data into Memcache channels based on the Memcached protocol.

Currently, Memcache Writer supports only one write mode. Data types written in different modes are converted differently:

-   text: Memcache Writer serializes source data to the String type, and uses your fieldDelimiter as the delimiter.
-   Binary: not supported.

## Parameter description​ {#section_ls4_n5m_q2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|None|
|writeMode|Memcache Writer writes data in the following modes:-   set: Store the data.
-   add: Store the data only when this key does not exist \(not supported currently\).
-   replace: Store the data only when this key exists \(not supported currently\).
-   append: Store data after the existing key, and ignore exptime \(not supported currently\).
-   prepend: Store data before the existing key, and ignore exptime \(not supported currently\).

|Yes|None |
|writeFormat|Currently, Memcache Writer supports writing data in only one format:TEXT: Serialize the source data to the text format with the first field being the key written into Memcache, and all subsequent fields to the String type. Use fieldDelimiter you specified as the delimiter to concatenate the text data into a complete string and write it into Memcache.

For example, source data is:

```
| ID | NAME | COUNT|
| ---- |:------|:-----|
| 23 | "AMC" | 100 |
```

If fieldDelimiter is specified as \\^, the data format written into Memcache is:

```
| KEY (OCS) | VALUE(OCS) |
| --------- |:---------- |
| 23 | CDP\^100 |
```

|No|None |
|ExpireTime|The cache invalidation time for the Memcache value. Currently, Memcache supports two types of the invalidation time.-   Unix time \(number of seconds since January 1, 1970\) indicates that data is invalid at a certain time point in the future.
-   The relative time \(in seconds\) starting from the current time point, which indicates the time length from the current time before data is invalid.

**Note:** If the invalidation time is larger than 60\\\*60\\\*24\*30 \(30 days\), the server identifies the invalidation time as the Unix time.

 |No|0. 0 permanently valid|
|batchSize|Description: The quantity of records submitted in one operation. Setting this parameter can greatly reduce the interactions between CDP and Memcache over the network, and increase the overall throughput. However, an excessively large value may cause the running process of CDP to become out of memory. \(Writing in batches is not supported for the current Memcache version.\)|No|1,024|

## Development in wizard mode {#section_b5s_hwm_q2b .section}

Currently, development in wizard mode is not supported.

## Development in script mode {#section_fgb_jwm_q2b .section}

Use the data generated from memory and imported into Memcache.

```
{
    "type": "job",
    "version": 2.0 ", // version number
    "steps":[
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType": "Oss", // plug-in name
            "parameter": {
                "Writeformat": "text", // memcache writer writes data format
                "expireTime": 1000, // memcache value cache failure time
                "indexes":0,
                "datasource": "", // Data Source
                "writeMode": "insert",//Write mode
                "batchSize": "1000", // number of records submitted in one batch size
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "Setting ":{
        "errorLimit": {
            "record": "0"//Number of error records
        },
        "speed": {
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "concurrent": "1",//Number of concurrent tasks
            "dmu": 1 // DMU Value
        }
    },
    "order":{
        "hops":[
            {
                "from":"Reader",
                "To": "Writer"
            }
        ]
    }
}
```

