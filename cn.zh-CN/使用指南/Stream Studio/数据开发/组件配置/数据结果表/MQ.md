# MQ {#concept_opc_cbg_xgb .concept}

消息队列（Message Queue）简称MQ，是阿里云商用的专业消息中间件，是企业级互联网架构的核心产品。消息列队是基于高可用分布式集群技术，搭建了包括发布订阅、消息轨迹、资源统计、定时（延时）、监控报警等一套完整的消息云服务。 实时计算可以将消息队列作为流式数据输入。更多信息请参见[创建消息队列（MQ）结果表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据结果表/创建消息队列（MQ）结果表.md#)。

## 配置面板说明 {#section_wkl_dbg_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|选择输出字段|要输出到DataHub表的字段列表|无|
|写入的MetaQ队列名|写入的Message Queue队列名|无|
|写入的群组|地址|*公共云内网接入（阿里云经典网络/VPC）：华东1、华东2、华北1、华北2、华南1香港的区域endpoint的地址是：`onsaddr-internal.aliyun.com:8080` 。*公共云公网接入地址是：`http://onsaddr-internet.aliyun.com/rocketmq/nsaddr4client-internet`。|
|accessID|填写自己的ID|无|
|accessKey|填写自己的Key|无|
|写入的群组|写入的群组|无|
|写入的标签|写入的标签|无|
|字段分割符|字段分割符|支持以\\u开头接四位十六进制数字表示任意unicode字符，例如\\u0001表示Crtl+A|
|编码|编码|无|
|retryTimes|写入重试次数|无|
|sleepTimeMs|重试间隔时间，单位ms|无|

