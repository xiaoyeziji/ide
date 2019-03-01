# Configure Redis Writer {#concept_fdm_sxn_q2b .concept}

This topic describes how to configure a Redis Writer. The Redis Writer is a Redis writing plug-in based on the Data Integration framework. It can import data from a data warehouse or other data source to a Redis instance. The Redis Writer interacts with the Redis Server through Jedis, which is a preferred Java client development kit provided by Redis that nearly has all Redis features.

Remote Dictionary Server \(Redis\) is a high-performance log-based key-value storage system that supports both persistent or memory-based network storage. . Redis can be used as a database, high-speed cache, and message queue \(MQ\) proxy. Redis supports different types of storage values, including string, list, set, zset \(sorted set\), and hash. For more information about Redis, see [redis.io](http://redis.io/).

**Note:** 

-   For more information on how to configure the data source before configuring a Redis Writer plug-in, see [Configure Redis data source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure Redis data source.md#).
-   If the value are lists when writing data to a Redis instance through the Redis Writer, the rerun synchronization task result is not idempotent. If the value type is list, you must clear the related data on Redis when rerunning the synchronization task.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Parameter|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|datasource|The data source name. The data source name must be the same as the added data source. Script Mode supports adding data source.|Yes|None|
|keyIndexes|The keyIndexes indicates which columns of the source table are used as key \(starts with 0 in the first column\). If the key is the group of the first and second columns, the value of keyIndexes is \[0,1\].**Note:** When keyIndexes are configured, the Redis Writer will list the remaining columns as value. You can specify the column on the Reader plug-in side for column filtering to synchronize a few columns in the source table as key and a few columns as value. You do not need to synchronize all fields.

|Yes|None|
|keyFieldDelimiter|Writes a key delimiter to Redis. For example, if the keyFieldDelimiter is key=key1\\u0001id, multiple keys need to be concatenated and requires the value. If only one key exists, this configuration item can be ignored.| No|\\u0001|
|batchSize|The number of records submitted in an operation. batchSize can greatly reduce interactions between Data Integration and PostgreSQL over the network, and increases the overall throughput. However, an excessively large value may cause the running process of Data Integration to become Out of memory \(OOM\).|No|1,000|
|expireTime|The Redis value cache expiration time is permanent validity if this configuration item is left empty.-   seconds: The current time \(in seconds\)that specifies the time period in which the data expires..
-   unixtime: The Unix time calculated in number of seconds from January 1, 1970, and specifies a future time point in which data expires.

**Note:** If the expiration time is greater than 60\*60\*24\*30 \(30 days\), the server identifies the expiration time as the Unix time.


|No|0 \(0 means the value is permanent validity\)|
|timeout|The time-out \(in milliseconds\) that was written to Redis.|No|30,000 \(This value covers 30 seconds of network breakdown time\)|
|dateFormat|The time when data is written into Redis in date format: "yyyy-MM-dd HH:mm:ss".|No|None|

|Parameter|Description|Parameter type|Description|Required|Default value|
|type-|mode|Valuefielddelimiter|
|:--------|:----------|:-------------|:----------|:-------|:------------|
|:----|:---|:------------------|
|writeMode|The Redis write mode. Redis supports different value types, including string, list, set, zset, and hash. Redis Writer can write these data types into a Redis instance. The configuration of writeMode varies slightly based on the value type. The writeMode is configured as follows, only one of the following types can be selected when you configure Redis Writer:|String \(string\)```
"writeMode":{
   "type": "string",
   "name": "set",
   "valueFieldDelimiter": "\u0001"
}
```

| |Value type: string|The write mode when the value type is string.|The delimiter between values when values are strings if there are more than two columns of source data in each row \(this configuration item can be ignored if only two columns of source data exist: "key" and "value"\), for example, value1\\u0001value2\\u0001value3.| No|String|
|Required|Yes|Required: Yes. Available value: set \(store the data, and overwrite this data if it already exists\)|No|
|Default value|-|-|\\u0001|
|List of strings```
"writeMode":{
    "type": "list",
    "mode": "lpush|rpush",
    "valueFieldDelimiter": "\u0001"
}
```

| |Value Type: List|The write mode when the value type is list.| The delimiter between string type values. The following is a delimiter example: value1\\u0001value2\\u0001value3.

 |
|Required|Yes|Required: Yes. Available value: lpush \(store the data on the far left of list\) | rpush \(store the data on the far right of list\)|No|
|Default value|-|-|\\u0001|
|StringCollection \(SET\)```
"writeMode":{
    "type": "set",
    "mode": "sadd",
    "valueFieldDelimiter": "\u0001"
}
```

| |List|The write mode when the value type is set.|The delimiter between values when the value type is string. For example, value1\\u0001value2\\u0001value3|
|Required|Yes|Required: Yes. Available value: sadd \(Stores the data into set, and overwrites existing data .\)|No|
|Default Value|-|-|\\u0001|
|StringCollection \(Zset\)**Note:** If the values are Zset data, each row of the data source records must follow this rule: With the exception of the key, each row can only contain one set of Score and Value. The Score must be located before the Value, so that Redis Writer can parse the Score column and the Value column.

```
"writeMode":{
    "type": "zset",
    "mode": "zadd"
}
```

|Description|zset|The write mode when the value type is zset.|-|
|Required| Yes|Required: Yes. Available value: zadd \(stores data into zset, and overwrite s existing data .\)|-|
|Hash \(hash\)**Note:** If the values are hashed, each row of the data source records must follow this rule: With the exception of the key, each row only contains one set of parameter and value, and parameter must be located before value, so that Redis Writer can parse the parameter column and the value column.

```
"writeMode":{
    "type": "hash",
    "mode": "hset"
}
```

|Description|Value type: hash|The write mode when the value type is hash.|-|
|Required|Yes|Required: Yes. Available value: hmset \(stores data into hash, and overwrite s existing data \)|-|

-   writeMode
    -   Redis supports different types of values, including string, list, set, zset, and hash. Redis Writer can also write these data types into Redis. However, the writeMode configuration is slightly different from the value type. Only one of the following five data types can be configured for Redis Writer:
        -   String \(string\)

            ```
            "Writemode ":{
               "type": "string",
               "mode": "set",
               "valueFieldDelimiter": "\u0001"
            }
            ```

            Parameters:

        -   type 
            -   Description: value type: string
            -   Required: Yes
        -   mode
            -   Description: The write mode when the value type is string.
            -   Required: Yes. Available value: set \(stores data, and overwrites existing data\)
        -   valueFieldDelimiter
            -   Description: The delimiter between values when the values are strings. If there are more than two columns of source data in each row, the delimiter for example is: value1\\u0001value2\\u0001value3. This configuration item can be ignored if only two columns of source data exist: "key" and "value".
            -   Required: No
            -   Default value: \\u0001
        -   List of strings

            ```
            "writeMode":{
                "type": "list",
                "mode": "lpush|rpush",
                "Maid": \ u0001"
            }
            ```

            Parameters:

            -   type
                -   Description: value type: List
                -   Required: Yes
            -   mode
                -   Description: The write mode when the value type is list.
                -   Required: Yes. Available value: lpush \(stores data on the far left of list\) | rpush \(stores data on the far right of list\)
            -   valueFieldDelimiter
                -   Description: The delimiter between value types that are string. For example: value1\\u0001value2\\u0001value3.
                -   Required: No
                -   Default value: \\u0001
        -   String collection \(set\)

            ```
            "writeMode":{
                "type": "set",
                "name": "set",
                "valueFieldDelimiter": "\u0001"
            }
            ```

            Parameters:

            -   type
                -   Description: value type: set
                -   Required: Yes
            -   mode
                -   Description: The write mode when the value type is set.
                -   Required: Yes. Available value: sadd \(stores data into set, and overwrites existing data\)
            -   valueFieldDelimiter
                -   Description: The delimiter between values when the value type is string. For example: value1\\u0001value2\\u0001value3.
                -   Required: No
                -   Default value: \\u0001
        -   StringCollection \(SET\)

            **Note:** If values are Zset data, each row of data source records must follow this rule: With the exception of key, each row can only contain one set of Score and Value. The Score must be located before Value, so the Redis Writer can parse the Score column and Value column.

            ```
            "writeMode":{
                "type": "zset",
                "mode": "zadd"
            }
            ```

            Configuration item descriptions:

            -   type
                -   Description: value type: zset
                -   Required: Yes;
            -   mode
                -   Description: The write mode when values are Zset data.
                -   Required: Yes. Available value: zadd \(stores data in the Zset sorted set, and overwrites existing data.\)
        -   Hash \(hash\)

            **Note:** If values are hashed, each row of data source records must follow this rule: With the exception of key, each row only contains one set of parameter and value. The parameter must be located before value, so that Redis Writer can parse the parameter column and the value column.

            ```
            "writeMode":{
                "type": "hash",
                "mode": "hset"
            }
            ```

            Parameters:

            -   type
                -   Description: value type: hash
                -   Required: Yes
            -   mode

                -   Description: The write mode when values are hashed.
                -   Required: Yes. Optional value: hmset \(stores values in the hash sorted set, and overwrites existing data\)
                You need to specify one of the data types. If the data type is left empty, the default data type is "string".

    -   Required: No
    -   Default value: string

## Development in Wizard Mode {#section_bp2_wsh_p2b .section}

Currently, development in Wizard Mode is not supported.

## Development in Script Mode {#section_cp2_wsh_p2b .section}

Configure Data Synchronization jobs written to Redis. For more information, see parameter descriptions.

```
{
    "type": "job",
    "version": "2.0", // version number
    "steps":[
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType": "redis", // plug-in name
            "parameter":{
                "expireTime": {// redis value cache failure time
                    "seconds": 1000"
                },
                "keyFieldDelimiter": u0001", // key separator written to redis.
                "dateFormat": "yyyy-MM-dd HH:mm:ss", // time format of date when redis is written
                "datasource": "", // Data Source
                "writeMode": {// write mode
                    "mode:" ", // alue is the mode of writing for a type
                    "valueFieldDelimiter": ", the separator between // Value
                    "type": "// Value Type
                },
                "Keyindexes": [// primary key index
                    0,
                    1-
                ],
                "batchSize": "1000", // number of records submitted in one batch size
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
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
                "to":"Writer"
            }
        ]
    }
}Writer"
            }
        ]
    }
}
```

