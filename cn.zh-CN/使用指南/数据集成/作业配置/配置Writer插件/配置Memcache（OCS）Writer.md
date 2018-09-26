# 配置Memcache（OCS）Writer {#concept_b31_vtm_q2b .concept}

云数据库Memcache版（ApsaraDB for Memcache，原简称OCS）是一种高性能、高可靠、可平滑扩容的分布式内存数据库服务。基于飞天分布式系统及高性能存储，并提供了双机热备、故障恢复、业务监控和数据迁移等方面的全套数据库解决方案。

云数据库Memcache版支持即开即用的方式快速部署，对于动态Web、APP应用，可通过缓存服务减轻对数据库的压力，从而提高网站整体的响应速度。

云数据库Memcache版与本地MemCache的相同之处在于：云数据库Memcache版兼容Memcached协议，与您的环境兼容，可直接用于云数据库Memcache版服务。不同之处在于硬件和数据部署在云端，有完善的基础设施、网络安全保障和系统维护等服务。所有的这些服务，都不需要投资，只需根据使用量进行付费即可。

Memcache Writer基于Memcached协议的数据写入Memcache通道。

Memcache Writer目前支持一种格式的写入方式，不同写入方式的类型转换方式不一致。

-   text：Memcache Writer将来源数据序列化为String类型格式，并使用您的fieldDelimiter作为间隔符。
-   binary：目前暂不支持。

## 参数说明 {#section_ls4_n5m_q2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|writeMode|Memcache Writer写入方式，具体如下：-   set：存储这个数据。
-   add：存储这个数据，当且仅当这个key不存在时（目前不支持）。
-   replace：存储这个数据，当且仅当这个key存在时（目前不支持）。
-   append：将数据存放在已存在的key对应的内容后面，忽略exptime（目前不支持）。
-   prepend：将数据存放在已存在的 key 对应的内容的前面，忽略 exptime（目前不支持）。

|是|无|
|writeFormat|Memcache Writer写出数据格式，目前仅支持TEXT数据写入方式。TEXT：将源端数据序列化为文本格式，其中第一个字段作为Memcache写入的key，后续所有字段序列化为String类型，使用您指定的fieldDelimiter作为间隔符，将文本拼接为完整的字符串再写入Memcache。

例如源头数据如下所示。

```
| ID   | NAME  | COUNT|
| ---- |:------|:-----|
| 23   | "CDP" | 100  |
```

如果您指定fieldDelimiter为\\^，那么写入Memcache的格式如下。

```
| KEY (OCS) | VALUE(OCS) |
| --------- |:---------- |
| 23        | CDP\^100   |
```

|否|无|
|expireTime|Memcache值缓存失效时间，目前MemCache支持两类过期时间。-   Unix时间（自1970.1.1开始到现在的秒数），该时间指定了到未来某个时刻的数据失效。
-   相对当前时间的秒数，该时间指定了从现在开始多长时间后数据失效。

**说明：** 如果过期时间的秒数大于60\*60\*24\*30（即30天），则服务端认为是Unix时间。

 |否|0，0永久有效|
|batchSize|一次性批量提交的记录数大小，该值可以极大减少CDP与Memcache的网络交互次数，并提升整体吞吐量。但该值设置过大可能会造成CDP运行进程OOM情况（Memcache版本暂不支持批量写）。|否|1024|

## 向导开发介绍 {#section_b5s_hwm_q2b .section}

暂时不支持向导模式开发。

## 脚本开发介绍 {#section_fgb_jwm_q2b .section}

使用一份从内存产生到Memcache导入的数据。

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
            "stepType":"ocs",//插件名
            "parameter":{
                "writeFormat":"text",//Memcache Writer 写出数据格式
                "expireTime":1000,//Memcache 值缓存失效时间
                "indexes":0,
                "datasource":"",//数据源
                "writeMode":"set",//写入模式
                "batchSize":"256"//一次性批量提交的记录数大小
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
            "throttle":false,//false代表不限流，下面的限流的速度不生效，true代表限流
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
}
```

