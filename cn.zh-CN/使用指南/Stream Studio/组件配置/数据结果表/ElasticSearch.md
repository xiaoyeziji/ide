# ElasticSearch {#concept_bmr_hcg_xgb .concept}

更多信息请参见[创建ElasticSearch（ES）结果表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据结果表/创建ElasticSearch（ES）结果表.md#)。

## 配置面板说明 {#section_fcj_3cg_xgb .section}

|参数|注释说明|默认值|
|--|----|---|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名| |
|选择输出字段|要输出到ElasticSearch表的字段列表|无|
|endPoint|server地址，例：http://127.0.0.1:9211|无|
|accessId|访问实例ID|无|
|accessKey|访问实例密钥|无|
|index|索引名称，类似于数据库database的名称。|无|
|typeName|type 名称，类似于数据库的table名称。|无|
|bufferSize|分 batch写入的records 条数|1000|
|maxRetryTimes|异常重试次数|30|
|timeout|读超时，单位为毫秒。|600000|
|discovery|是否开启节点发现。如果开启客户端会 5 分钟刷新一次 server list。|false|
|compression|是否使用 GZIP 压缩 request bodies|true|
|multiThread|是否开启 JestClient 多线程|true|
|ignoreWriteError|是否忽略写入异常|false|
|settings|创建index的settings配置|无|

**说明：** 

-   本文档仅适用于独享模式。
-   ES支持根据primaryKey进行update，primaryKey只能为1个字段。
-   指定primaryKey后，document的id为primaryKey字段的值。
-   未指定primaryKey的document对应的id为随机，详情请参见Index API。
-   full更新模式下，后面的doc会完全覆盖之前的doc，不会原地更新字段。
-   full更新模式下，后面的doc会完全覆盖之前的doc，不会原地更新字段。
-   所有的更新默认为upsert语义，即insert or update。

