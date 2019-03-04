# 配置Redis Writer {#concept_fdm_sxn_q2b .concept}

Redis Writer是基于数据集成框架实现的Redis写入插件，可以借助Redis Writer从数仓或者其它数据源导入数据到Redis。Redis Writer与Redis Server之间的交互是基于Jedis实现的，Jedis是Redis官方首选的Java客户端开发包，几乎实现了Redis的所有功能。

Redis（REmote DIctionary Server）是一个支持网络、可基于内存也可持久化的日志型、高性能的key-value存储系统，可用作数据库、高速缓存和消息队列代理。Redis支持较丰富的存储value类型，包括String （字符串）、List（链表） 、Set（集合） 、ZSet（sorted set 有序集合）和 Hash（哈希类型）。Redis详情请参见[redis.io](http://redis.io/)。

**说明：** 

-   开始配置Redis Writer插件前，请首先配置好数据源，详情请参见[配置Redis数据源](intl.zh-CN/使用指南/数据集成/数据源配置/配置Redis数据源.md#)。
-   使用Redis Writer向Redis写入数据时，如果value类型是list，重跑同步任务同步结果不是幂等的。因此如果value类型是list ，重跑同步任务时，需要您手动清空Redis上相应的数据。

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|keyIndexes|keyIndexes表示源端哪几列需要作为key（第一列是从0开始）。如果是第一列和第二列需要组合作为key，那么keyIndexes的值则为\[0,1\]。**说明：** 配置keyIndexes后，Redis Writer会将其余的列作为value。如果您只想同步源表的某几列作为key，某几列作为value，不需要同步所有字段，那么在Reader插件端指定好column进行列筛选即可。

|是|无|
|keyFieldDelimiter|写入Redis的key分隔符。例如key=key1\\u0001id，如果key有多个需要拼接时，该值为必填项，如果key只有一个则可以忽略该配置项。|否|\\u0001|
|batchSize|一次性批量提交的记录数大小，该值可以极大减少数据集成与PostgreSQL的网络交互次数，并提升整体吞吐量。但是该值设置过大可能会造成数据集成运行进程OOM情况。|否|1000|
|expireTime|Redis value值缓存失效时间（如果需要永久有效则可以不填该配置项），单位为秒。-   seconds：相对当前时间的秒数，该时间指定了从现在开始多长时间后数据失效。
-   unixtime：Unix时间（自1970.1.1开始到现在的秒数），该时间指定了到未来某个时刻数据失效。

**说明：** 如果过期时间的秒数大于60\*60\*24\*30（即30天），则服务端认为是Unix时间。


|否|0（0表示永久有效）|
|timeout|写入Redis的超时时间，单位为毫秒。|否|30000（即可以cover住30秒的网络断连时间）|
|dateFormat|写入Redis时，Date的时间格式为"yyyy-MM-dd HH:mm:ss"。|否|无|
|writeMode|Redis的一大亮点是支持丰富的value类型，包括字符串（string）、字符串列表（list）、字符串集合（set）、有序字符串集合（zset）和哈希（hash）。Redis Writer也支持该五种类型的写入，根据不同的value类型，writeMode配置会略有差异，writeMode详细配置如下。**说明：** 您在配置Redis Writer时，只能配置以下五种类型中的一种。

-   字符串（string）

    ```
"writeMode":{
   "type": "string",
   "mode": "set",
   "valueFieldDelimiter": "\u0001"
}
    ```

配置项说明：

    -   type
        -   描述：value为string类型
        -   必选：是
    -   mode
        -   描述：value为string类型时，写入的模式
        -   必选：是，可选值为set（存储这个数据，如果已经存在则覆盖）
    -   valueFieldDelimiter
        -   描述：该配置项是考虑了当源数据每行超过两列的情况（如果您的源数据只有两列即key和value时，则可忽略该配置项，不用填写），value类型为string时，value之间的分隔符，比如 value1\\u0001value2\\u0001value3。
        -   必选：否
        -   默认值：\\u0001
-   字符串列表（list）

    ```
"writeMode":{
    "type": "list",
    "mode": "lpush|rpush",
    "valueFieldDelimiter": "\u0001"
}
    ```

配置项说明：

    -   type
        -   描述：value为list类型
        -   必选：是
    -   mode
        -   描述：value为list类型时，写入的模式。
        -   必选：是，可选值为lpush（在list最左边存储这个数据）和rpush（在list最右边存储这个数据）。
    -   valueFieldDelimiter
        -   描述：value为string类型时，value之间的分隔符，比如value1\\u0001value2\\u0001value3。
        -   必选：否
        -   默认值：\\u0001
-   字符串集合（set）

    ```
"writeMode":{
    "type": "set",
    "mode": "sadd",
    "valueFieldDelimiter": "\u0001"
}
    ```

配置项说明：

    -   type
        -   描述：value为set类型
        -   必选：是
    -   mode
        -   描述：value为set类型时，写入的模式。
        -   必选：是，可选值：sadd（向set集合中存储这个数据，如果已经存在则覆盖）。
    -   valueFieldDelimiter
        -   描述：value类型是string时，value之间的分隔符，比如value1\\u0001value2\\u0001value3。
        -   必选：否
        -   默认值：\\u0001
-   有序字符串集合（zset）

**说明：** 当value类型为zset时，数据源的每一行记录需要遵循相应的规范，即每一行记录除key以外，只能有一对score和value，并且score必须在value前面，rediswriter方能解析出哪一个column是score，哪一个column是value。

    ```
"writeMode":{
    "type": "zset",
    "mode": "zadd"
}
    ```

配置项说明：

    -   type
        -   描述：value为zset类型
        -   必选：是
    -   mode
        -   描述：value为zset类型时，写入的模式。
        -   必选：是，可选值为zadd（向zset有序集合中存储这个数据，如果已经存在则覆盖）。
-   哈希（hash）

**说明：** 当value类型为hash时，数据源的每一行记录需要遵循相应的规范，即每一行记录除key以外，只能有一对attribute和value，并且attribute必须在value前面，Rediswriter方能解析出哪一个column是attribute，哪一个column是value。

    ```
"writeMode":{
    "type": "hash",
    "mode": "hset"
}
    ```

配置项说明：

    -   type
        -   描述：value为hash类型
        -   必选：是
    -   mode

        -   描述：value为hash类型时，写入的模式。
        -   必选：是，可选值为hmset（向hash有序集合中存储这个数据，如果已经存在则覆盖）。
您需要配置其中一种写入数据类型，如果您不填，默认数据类型是string。


|否|string|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂不支持向导开发模式。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置写入Redis的数据同步作业，具体参数填写请参见参数说明。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        { //下面是关于Reader的模板，可以找相应的读插件文档
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType":"redis",//插件名
            "parameter":{
                "expireTime":{//Redis value 值缓存失效时间
                    "seconds":"1000"
                },
                "keyFieldDelimiter":"u0001",//写入 Redis 的 key 分隔符
                "dateFormat":"yyyy-MM-dd HH:mm:ss",//写入 Redis 时，Date 的时间格式
                "datasource":"",//数据源
                "writeMode":{//写入模式
                    "mode":"",//alue 是某类型时的写入的模式
                    "valueFieldDelimiter":"",//value 之间的分隔符
                    "type":""//value 类型
                },
                "keyIndexes":[//主键索引
                    0,
                    1
                ],
                "batchSize":"1000"//一次性批量提交的记录数大小
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record":"0"//错误记录数
        },
        "speed":{
            "throttle":false,////false代表不限流，下面的限流的速度不生效，true代表限流
            "concurrent":1,//作业并发数
            "dmu":1//DMU值
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

