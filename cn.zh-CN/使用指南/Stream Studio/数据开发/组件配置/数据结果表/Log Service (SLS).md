# Log Service \(SLS\) {#concept_byq_pzf_xgb .concept}

日志服务（Log Service）简称LOG，是针对实时数据一站式服务，无需开发就能快捷完成数据采集、消费、投递以及查询分析等功能，帮助提升运维、运营效率，建立DT（Data Technology）时代海量日志处理能力。更多信息请参考[创建日志服务（Log Service）结果表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据结果表/创建日志服务（Log Service）结果表.md#)。

## 配置面板说明 {#section_g41_rzf_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|选择输出字段|输出到log service的字段列表|无|
|endPoint地址|endPoint地址|[服务入口](../../../../../cn.zh-CN/API 参考/服务入口.md#)|
|项目名|项目名|无|
|topic表名|topic表名|无|
|source|日志的来源地，例如产生该日志机器的IP地址。|无|
|accessId|accessId|无|
|accessKey|accessKey|无|
|写入方式|写入方式|可选，默认为`random`模式，选择`partition`则会按分区写入。|
|分区列|分区列|如果`mode`为`partition`则必填。|
|logStore|Project下面具体的logStore|无|

