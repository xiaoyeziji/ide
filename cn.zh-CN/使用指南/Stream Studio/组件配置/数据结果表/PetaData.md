# PetaData {#concept_jl4_zzf_xgb .concept}

云数据库HybridDB for MySQL（原名PetaData）是同时支持在线事务（OLTP）和在线分析（OLAP）的关系型 HTAP 类数据库。HTAP是Hybrid Transaction/Analytical Processing的简写，意为将数据的事务处理（TP）与分析（AP）混合处理，从而实现对数据的实时处理分析HybridDB for MySQL兼容MySQL的语法及函数，并且增加了对Oracle常用分析函数的支持，完全兼容TPC-H和TPC-DS测试标准。更多信息请参见[创建云数据库（HybridDB for MySQL）结果表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据结果表/创建云数据库（HybridDB for MySQL）结果表.md#)。

## 配置面板说明 {#section_wrf_c1g_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|选择输出字段|要输出到云数据库表的字段列表|无|
|地址|地址|[切换网络类型](../../../../../cn.zh-CN/用户指南/管理实例/切换网络类型.md#)|
|表名|表名|无|
|用户名|用户名|无|
|密码|密码|无|
|最大尝试插入次数|最大尝试插入次数|无|
|每次写的批次大小|每次写的批次大小|表示每次写多少条|
|去重的buffer大小，需要指定主键才生效|去重的buffer大小，需要指定主键才生效|无|
|写超时时间|写超时时间，单位ms|表示数据超过了指定ms，还没有写过，就会将缓存的数据写入一次。|
|是否忽略delete操作|是否忽略delete操作|无|
|primaryKey|指定输出字段中作为主键的字段|可多选|

**说明：** 

-   本文档仅适用于独享模式。
-   实时计算写入PetaData数据库结果表原理:针对实时计算每行结果数据，拼接成一行SQL向目标端数据库进行执行。
-   bufferSize默认值是1000，如果到达bufferSize阈值的话（buffer hashmap size），也会触发写出。因此您配置batchSize的同时还需要配上bufferSize。bufferSize和batchSize大小相同即可。
-   建议设置batchSize='4096'，bathcSize数值不建议设置过大。

