# HBase {#concept_tpr_zbg_xgb .concept}

更多信息请参见[创建云数据库（HBase）结果表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据结果表/创建云数据库（HBase）结果表.md#)。

## **配置面板介绍** {#section_fzj_1cg_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|选择输出字段|要输出到HBase表的字段列表|无|
|hbase集群配置的zk地址|HBase集群配置的zk地址|可以在`hbase-site.xml`文件中找到`hbase.zookeeper.quorum`相关配置。|
|集群配置在zk上的路径|集群配置在zk上的路径|可以在`hbase-site.xml`文件中找到`hbase.zookeeper.quorum`相关配置。|
|hbase 表名|HBase表名|无|
|列族名|列族名|无|
|用户名|用户名|无|
|密码|密码|无|
|partitionBy|设置为true之后会在用joinKey做partition，将数据分发到join节点，提高缓存命中率。|无|
|shuffleEmptyKey|设置为true之后遇到空key会随机往下游做shuffle，否则往0号下游发。|建议打开|
|插入尝试次数|插入尝试次数|无|
|流入多少条数据后进行去重|流入多少条数据后进行去重|无|
|批次写入大小|批次写入大小|无|
|最长插入时间|最长插入时间，单位ms|无|
|是否写入主键值|是否写入主键值|无|
|是否都按照string插入|是否都按照string插入|无|
|rowKey的分隔符|rowKey的分隔符|无|
|是否为动态表|是否为动态表|无|
|primaryKey|从输出字段中选择主键字段|无|

**说明：** 

-   本文档只适用于独享模式。
-   primary key支持定义多个字段。多个字段会按照rowkeyDelimiter（默认为:）拼接起来作为row\_key。
-   HBase做撤回删除操作时，如果column定义了多版本，会把所有版本的值清空。

