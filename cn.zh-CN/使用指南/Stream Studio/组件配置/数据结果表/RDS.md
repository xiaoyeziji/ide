# RDS {#concept_bb1_41g_xgb .concept}

阿里云关系型数据库（Relational Database Service）简称RDS是一种稳定可靠、可弹性伸缩的在线数据库服务。更多信息请参考[创建云数据库（RDS和DRDS）结果表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据结果表/创建云数据库（RDS和DRDS）结果表.md#)。

## 配置面板说明 {#section_ynb_q1g_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|选择输出字段|要输出到RDS表的字段列表|无|
|地址|地址|地址请参见[RDS的URL地址](../../../../../cn.zh-CN/RDS for MySQL 快速入门/初始化配置/申请外网地址.md#)及[DRDS的URL地址](https://help.aliyun.com/document_detail/50084.html?spm=a2c4g.11186623.6.553.wR7Itn)|
|表名|表名|无|
|用户名|用户名|无|
|密码|密码|无|
|最大尝试插入次数|最大尝试插入次数|无|
|每次写的批次大小|每次写的批次大小|表示每次写多少条|
|去重的buffer大小|去重的buffer大小，需要指定主键才生效|表示输入的数据达到指定条就开始输出|
|写超时时间|写超时时间，单位ms|表示数据超过了指定ms，还没有写过，就会将缓存的数据都写一次。|
|excludeUpdateColumns|对相同key的值更新时排除掉相应的column|无|
|是否忽略delete操作|是否忽略delete操作|无|
|partitionBy|写入Sink节点前，会根据该值做hash。数据会流向相应的hash节点。|无|

## 类型映射 {#section_pn4_r1g_xgb .section}

|RDS字段类型|实时计算字段类型|
|-------|--------|
|text|varchar|
|byte|varchar|
|integer|int|
|long|bigint|
|double|double|
|date|varchar|
|datetime|varchar|
|timestamp|varchar|
|time|varchar|
|year|varchar|
|float|float|
|decimal|decimal|
|char|varchar|

## JDBC 连接参数 {#section_gny_s1g_xgb .section}

|参数名称|参数说明|缺省值|最低版本要求|
|----|----|---|------|
|useUnicode|是否使用Unicode字符集，如果参数characterEncoding设置为gb2312或gbk，本参数值必须设置为true。|false|1.1g|
|characterEncoding|当useUnicode设置为true时，指定字符编码。比如可设置为gb2312或gbk。|false|1.1g|
|autoReconnect|当数据库连接异常中断时，是否自动重新连接。|false|1.1|
|autoReconnectForPools|是否使用针对数据库连接池的重连策略。|false|3.1.3|
|failOverReadOnly|自动重连成功后，连接是否设置为只读。|true|3.0.12|
|maxReconnects|autoReconnect设置为true时，重试连接的次数。|3|1.1|
|initialTimeout|autoReconnect设置为true时，两次重连之间的时间间隔，单位：秒。|2|1.1|
|connectTimeout|和数据库服务器建立socket连接时的超时，单位：毫秒。 0表示永不超时，适用于JDK 1.4及更高版本。|0|3.0.1|
|socketTimeout|socket操作（读写）超时，单位：毫秒。 0表示永不超时。|0|3.0.1|

## FAQ {#section_nvn_51g_xgb .section}

-   实时计算的结果数据写入RDS表，是按主键更新的，还是新生成一条记录？

    A：如果在DDL中定义了主键，会采用`insert into on duplicate key update`的方式更新记录，也就意味着对于不存在的主键字段会直接插入，存在的主键字段则更新相应的值。 如果DDL中没有声明primary key，则会用`insert into` 方式插入记录，追加数据。

-   Q：使用RDS表中的唯一索引做GROUP BY需要注意什么？

    A：RDS中只有一个自增主键，实时计算作业中不能声明为Primary Key；如果需要使用RDS表中的唯一索引做GROUP BY，需要在作业中的Primary Key中声明这些唯一索引。


