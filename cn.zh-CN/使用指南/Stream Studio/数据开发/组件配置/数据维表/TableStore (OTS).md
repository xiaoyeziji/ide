# TableStore \(OTS\) {#concept_grh_mxf_xgb .concept}

表格存储（Table Store）简称OTS，是根据99.99%的高可用以及11个9的数据可靠性的设计标准，构建在阿里云飞天分布式系统之上的分布式NoSQL数据存储服务。更多信息请参考[创建表格存储（Table Store）维表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据维表/创建表格存储（Table Store）维表.md#)。

## 配置面板说明 {#section_orh_4xf_xgb .section}

|参数|注释说明|
|--|----|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|
|选择输出字段|要输出到Table Store表的字段列表|
|实例名|实例名|
|表名|表名|
|和数据集成同步|从元数据中心读取指定表名的元数据帮助填充输出字段和其他元信息表单项|
|选择输出字段|从维表中读取输出到下游组件的字段列表|
|实例访问地址|实例访问地址|
|AccessKey ID|AccessKey ID|
|AccessKey Secret|AccessKey Secret|
|缓存策略|可选 LRU|
|缓存大小|当选择 LRU缓存策略后，可以设置缓存大小|
|缓存超时时间，单位毫秒|缓存超时时间，单位毫秒|
|primaryKey|指定输出字段中作为主键的字段|

