# DataHub {#concept_smp_3bg_xgb .concept}

DataHub作为一款流式数据总线，为阿里云数加平台提供了大数据的入口服务。结合阿里云众多云产品，可以构建一站式的数据处理平台。实时计算通常使用DataHub作为流式数据存储输入源和输出目的端。更多信息请参考[创建数据总线（DataHub）结果表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据结果表/创建数据总线（DataHub）结果表.md#)。

## 配置面板说明 {#section_pwv_jbg_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名| |
|选择输出字段|要输出到DataHub表的字段列表| |
|endPoint地址|endpoint地址|参见[DataHub的Endpoint地址](https://help.aliyun.com/document_detail/47442.html?spm=5176.doc47439.6.542.w2TEz3)|
|项目名|项目名|无|
|topic|topic表名|无|
|accessId|accessId|无|
|accessKey|accessKey|无|
|最大尝试插入次数|最大尝试插入次数|无|
|每次写的批次大小|每次写的批次大小|无|
|缓存数据的最大超时时间|缓存数据的最大超时时间，单位ms|无|
|每次写入的最大Block数|每次写入的最大Block数|无|
|数据质量|跳转数据质量中心查看相关监控任务|无|

## 类型映射 {#section_tbc_lbg_xgb .section}

DataHub和实时计算字段类型对应关系，强烈建议用户使用该对应关系进行DDL声明：

|DataHub字段类型|实时计算字段类型|
|-----------|--------|
|BIGINT|BIGINT|
|DOUBLE|DOUBLE|
|TIMESTAMP|BIGINT|
|BOOLEAN|BOOLEAN|
|DECIMAL|DECIMAL|

