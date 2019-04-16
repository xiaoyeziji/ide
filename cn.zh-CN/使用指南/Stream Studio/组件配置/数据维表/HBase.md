# HBase {#concept_omw_cxf_xgb .concept}

更多信息请参考[实时计算云数据库（HBase）维表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据维表/创建云数据库（HBase）维表.md#)。

## 配置面板说明 {#section_rjw_fxf_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|表名|HBase表名|无|
|和数据集成同步|从元数据中心读取指定表名的元数据帮助填充输出字段和其他元信息表单项|无|
|列族名|列族名|目前只支持插入同一列族|
|Rowkey字段名|指定输出字段中作为主键的字段|无|
|HBase集群对应的ZooKeeper地址|HBase集群配置的zk地址,是以`,`分隔的主机列表。|可以在hbase-site.xml文件中找到hbase.zookeeper.quorum相关配置。|
|HBase集群配置在zk上的路径|HBase集群配置在zk上的路径|可以在hbase-site.xml文件中找到hbase.zookeeper.quorum相关配置。|
|选择输出字段|要输出到HBase表的字段列表|无|
|缓存策略|缓存策略|可选None、LRU、ALL|
|缓存大小|缓存大小|当选择 `LRU` 缓存策略后，可以设置缓存大小|
|缓存超时时间|缓存超时时间，单位为毫秒|选择 `LRU` 缓存策略后，可以设置缓存失效的超时时间，默认不过期。当选择 `ALL` 策略，则为缓存reload 的间隔时间，默认不重新加载|
|更新时间黑名单|缓存策略选择`ALL`时启用。更新时间黑名单，防止在此时间内做cache更新。（如双11场景）|自定义输入格式为2017-10-24 14:00 -\> 2017-10-24 15:00, 2017-11-10 23:30 -\> 2017-11-11 08:00|
|cacheScanLimit|缓存策略选择`ALL`时启用。load全量HBase数据，服务端一次RPC返回给客户端的行数|无|
|用户名|用户名|无|
|密码|密码|无|
|最大尝试次数|最大尝试次数|默认10次|
|partitionedJoin|设置为true之后会在用joinKey做partition，将数据分发到join节点，提高缓存命中率。|可选，默认关闭。|
|shuffleEmptyKey|设置为true之后遇到空key会随机往下游做shuffle，否则往0号下游发。|建议打开|

## 说明 {#section_pmt_3xf_xgb .section}

-   声明维表时，必须要指名主键。维表JOIN的时候，ON的条件必须包含所有主键的等值条件。HBase中的主键即rowkey。
-   目前RDS和DRDS提供如下三种缓存策略：None：无缓存；LRU：最近使用策略缓存。需要配置相关参数：缓存大小（\*\*cacheSize**cacheTTLMs**cacheTTLMs**cacheReloadTimeBlackList**）。
-   因为会异步reload，使用cache all的时候，需要将维表JOIN的节点增加一些内存，增加的内存大小为远程表2倍的数据量。

