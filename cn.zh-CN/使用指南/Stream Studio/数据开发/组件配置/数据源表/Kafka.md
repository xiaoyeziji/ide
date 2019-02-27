# Kafka {#concept_yxg_5ls_wgb .concept}

Kafka源表的实现迁移自社区的kafka版本实现。 Kafka源表数据解析流程：Kafka Source Table \> UDTF \> Flink \> SINK，从Kakfa读入的数据，都是VARBINARY（二进制）格式，对读入的每条数据，都需要用UDTF将其解析成格式化数据。更多信息请参考[创建消息队列（Kafka）源表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据源表/创建消息队列（Kafka）源表.md#)。

## 配置面板说明 {#section_rqt_j4f_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|列定义|定义要读取的kafka字段列表|kafka的列定义必须依次为messageKey varbinary、message varbinary、topic varchar、partition int、offset bigint，五个字段顺序务必按照以上描述排列。|
|kafka对应版本|kafka对应版本|无|
|读取的单个topic|读取的单个topic|可选项kafka08、kafka09、kafka10、kafka11|
|读取一批topic的表达式|读取一批topic的表达式|无|
|启动位点|启动位点|EARLISET 从kafka最早分区开始读 Group\_OFFSETS 根据Group读取 LATEST 从kafka最新位点开始读取数据 TIMESTAMP 从指定的时间点读取数据\(kafk010、kafka011支持\)|
|定时检查是否有新分区产生|检查是否有新分区产生的时间间隔，单位ms|无|
|group.id|消费组id|可选，未在可选配置项中额外期望配置|
|zk链接地址|zk链接地址|适用于kafka08|
|kafka集群地址|kafka集群地址|适用于kafka09、kafka10、kafka11|
|添加扩展参数|添加kafka支持的可选配置项|无|

## 可选扩展参数 {#section_mvf_n4f_xgb .section}

-   **kafka08**

    "consumer.id","socket.timeout.ms","fetch.message.max.bytes","num.consumer.fetchers","auto.commit.enable","auto.commit.interval.ms","queued.max.message.chunks", "rebalance.max.retries","fetch.min.bytes","fetch.wait.max.ms","rebalance.backoff.ms","refresh.leader.backoff.ms","auto.offset.reset","consumer.timeout.ms","exclude.internal.topics","partition.assignment.strategy","client.id","zookeeper.session.timeout.ms","zookeeper.connection.timeout.ms","zookeeper.sync.time.ms","offsets.storage","offsets.channel.backoff.ms","offsets.channel.socket.timeout.ms","offsets.commit.max.retries","dual.commit.enabled","partition.assignment.strategy","socket.receive.buffer.bytes","fetch.min.bytes"

-   [**kafka09**](https://kafka.apache.org/0110/documentation.html#consumerconfigs)
-   [**kafka010**](https://kafka.apache.org/090/documentation.html#newconsumerconfigs)
-   [**kafka011**](https://kafka.apache.org/0102/documentation.html#newconsumerconfigs)

## kafka版本对应关系 {#section_qql_fpf_xgb .section}

|type|kafka 版本|
|----|--------|
|kafka08|0.8.22|
|kafka09|0.9.0.1|
|kafka010|0.10.2.1|
|kafka011|0.11.0.2|

