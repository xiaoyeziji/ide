# Configure Redis Writer {#concept_fdm_sxn_q2b .concept}

Redis \(Remote Dictionary Server\) is a high-performance persistent log-based key-value storage system supporting network and based on memory, which can be used as a database, high-speed cache, and message queue \(MQ\) proxy. Redis supports diverse types of storage values, including string, list, set, zset \(sorted set\), and hash. For details about Redis, see [redis.io](http://redis.io/).

Redis Writer is a Redis writing plug-in based on the Data Integration framework. It can import data from a data warehouse or other data sources to a Redis instance. Redis Writer interacts with Redis Server via Jedis. As a preferred Java client development kit provided by Redis, Jedis has almost all Redis features.

**Note:** 

-   Configure the data source before configuring a Redis Writer plug-in. For details, see [Configure Redis data source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure Redis data source.md#)Configure the Redis Data Source.
-   When data is written to a Redis instance through Redis Writer, if values are lists, the result of the re-run synchronization task is not idempotent. So if the value type is list, you must manually clear the corresponding data on Redis when re-running the synchronization task.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|datasource|Data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|None|
|Keyindexes|Description: keyIndexes indicates which columns of the source table are used as key \(starts with 0 for the first column\). If the key is the combination of the first and second columns, the value of keyIndexes is \[0,1\].**Note:** When keyindexes is configured, The redis writer takes the remaining columns as value. If you want to synchronize only a few columns of the source table as key, a few columns as value, you do not need to synchronize all the fields, so you can specify column on the Reader plug-in side for column filtering.

|Yes|None|
|Keyfielddelimiter|Writes a key separator to redis. Take key=key1\\u0001id as an example. If multiple keys need to be concatenated, the value is required; if only one key exists, this configuration item can be ignored.| No|\\u0001|
|batchSize|Description: The quantity of records submitted in one operation. This parameter can greatly reduce the interactions between Data Integration and PostgreSQL over the network, and increase the overall throughput. However, an excessively large value may cause the running process of Data Integration to become out of memory \(OOM\).|No|1,000|
|expireTime|Description: The Redis value cache expiration time \(which is valid permanently if this configuration item is left empty\).-   seconds: The relative time \(in seconds\) starting from the current time point, which indicates the time length from the current time before data is invalid.
-   unixtime: The Unix time \(number of seconds from January 1, 1970\), specifying a future time point at which data becomes invalid.

**Note:** If the invalidation time is larger than 60\*60\*24\*30 \(30 days\), the server identifies the invalidation time as the Unix time.


|No|0 \(0 indicates permanent validity\)|
|timeout|The time-out, in milliseconds, that was written to redis.|No|30000 \(that is, cover 30 seconds of network break time\)|
|dateFormat|The time when data is written into Redis in date format: "yyyy-MM-dd HH:mm:ss".|No|None|

|Attribute|Description|Parameter type|Description|Required|Default Value|
|type|mode|Valuefielddelimiter|
|:--------|:----------|:-------------|:----------|:-------|:------------|
|:---|:---|:------------------|
|writeMode|Description: Redis supports diverse types of values, including string, list, set, zset, and hash. Redis Writer can write these types of data into a Redis instance. Configuration of writeMode varies slightly based on the value type. writeMode is configured as follows. Only one of the following types can be selected when you configure Redis Writer:|String \(string\)```
"writeMode":{
   "type": "string",
   "name": "set",
   "valueFieldDelimiter": "\u0001"
}
```

|Description|Description: value type: string|Description: The write mode when the value type is string.|Description: The delimiter between values when values are strings if there are more than two columns of source data in each row \(this configuration item can be ignored if only two columns of source data exist: "key" and "value"\), for example, value1\\u0001value2\\u0001value3.| No|Default value: string|
|Required|Yes|Required: Yes. Available value: set \(store the data, and overwrite this data if it already exists\)|No|
|Default Value|-|-|\\u0001|
|List of strings```
"writeMode":{
    "type": "list",
    "mode": "lpush|rpush",
    "valueFieldDelimiter": "\u0001"
}
```

|Description|Value Type: List|Description: The write mode when the value type is list.|Description: The delimiter between values when the value type is string. For example, value1\\u0001value2\\u0001value3.|
|Required|Yes|Required: Yes. Available value: lpush \(store the data on the far left of list\) | rpush \(store the data on the far right of list\)|No|
|Default Value|-|-|\\u0001|
|String collection \(SET\)```
"writeMode":{
    "type": "set",
    "mode": "sadd",
    "valueFieldDelimiter": "\u0001"
}
```

|Description|Value Type: List|Description: The write mode when the value type is set.|Description: The delimiter between values when the value type is string. For example, value1\\u0001value2\\u0001value3|
|Required|Yes|Required: Yes. Available value: sadd \(store the data into set, and overwrite this data if it already exists\)|No|
|Default Value|-|-|\\u0001|
|String collection \(zset\)**Note:** NOTE: If values are Zset data, each row of records of the data source must follow this rule: apart from the key, each row only contains one pair of score and value, and score must be located before value, so that Redis Writer can parse the score column and the value column.

```
"writeMode":{
    "type": "zset",
    "mode": "zadd"
}
```

|Description|Value Type: zset|Description: The write mode when the value type is zset|-|
|Required| Yes|Required: Yes. Available value: zadd \(store the data into zset, and overwrite this data if it already exists\)|-|
|Hash \(hash\)**Note:** NOTE: If values are hashed, each row of records of the data source must follow this rule: apart from the key, each row only contains one pair of attribute and value, and attribute must be located before value, so that Redis Writer can parse the attribute column and the value column.

```
"writeMode":{
    "type": "hash",
    "mode": "hset"
}
```

|Description|value Type: hash|Description: The write mode when the value type is hash.|-|
|Required|Yes|Required: Yes. Available value: hmset \(store the data into hash, and overwrite this data if it already exists\)|-|

-   writeMode
    -   Description: Redis supports diverse types of values, including string, list, set, zset, and hash. Redis Writer can also write these types of data into Redis. However, the configuration of writeMode varies slightly with the value type. writeMode is configured as follows. Only one of the following five types can be configured when you configure Redis Writer:
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
            -   Required: Yes. Available value: set \(store the data, and overwrite this data if it already exists\)
        -   valueFieldDelimiter
            -   Description: The delimiter between values when values are strings if there are more than two columns of source data in each row \(this configuration item can be ignored if only two columns of source data exist: "key" and "value"\), for example, value1\\u0001value2\\u0001value3.
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
                -   Description: value type: list
                -   Required: Yes
            -   mode
                -   Description: The write mode when the value type is list.
                -   Required: Yes. Available value: lpush \(store the data on the far left of list\) | rpush \(store the data on the far right of list\)
            -   valueFieldDelimiter
                -   Description: The delimiter between values when the value type is string. For example, value1\\u0001value2\\u0001value3.
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
                -   Required: Yes. Available value: sadd \(store the data into set, and overwrite this data if it already exists\)
            -   valueFieldDelimiter
                -   Description: The delimiter between values when the value type is string. For example, value1\\u0001value2\\u0001value3.
                -   Required: No
                -   Default value: \\u0001
        -   String collection \(SET\)

            **Note:** NOTE: If values are Zset data, each row of records of the data source must follow this rule: apart from the key, each row only contains one pair of score and value, and score must be located before value, so that Redis Writer can parse the score column and the value column.

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
                -   Mandatory: Yes; available value: zadd \(stored in the Zset sorted set, and overwritten if it already exists\)
        -   Hash \(hash\)

            **Note:** NOTE: If values are hashed, each row of records of the data source must follow this rule: apart from the key, each row only contains one pair of attribute and value, and attribute must be located before value, so that Redis Writer can parse the attribute column and the value column.

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

                -   Description: The write mode when values are hashed
                -   Mandatory: Yes. Optional value: hmset \(stored in the hash sorted set, and overwritten if it already exists\)
                You need to specify one of the data types. If you leave it empty, the data type is "string" by default.

    -   Required: No
    -   Default value: string

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Currently, development in wizard mode is not supported.

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure Data Synchronization jobs written to redis, see parameter descriptions for details.

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

