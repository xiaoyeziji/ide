# MaxCompute \(ODPS\) {#concept_z4q_pbg_xgb .concept}

更多信息请参见[创建MaxCompute（ODPS）结果表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据结果表/创建MaxCompute（ODPS）结果表.md#)。

## 配置面板说明 {#section_dpr_qbg_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|表名|表名|无|
|选择输出字段|要输出到MaxCompute表的字段列表|无|
|endPoint|地址|参见[配置Endpoint](../../../../../cn.zh-CN/准备工作/配置Endpoint.md#)。|
|project|项目|无|
|accessId|填写自己的ID|无|
|accessKey|填写自己的KEY|无|
|partition|写入分区|分区表必填，具体分区信息到[数据管理](cn.zh-CN/使用指南/数据管理/数据管理概述.md#)查看。例如: 一个表的分区信息为`ds=20180905`，则可以写 `partition` = 'ds=20180905'`。多级分区之间用逗号分隔，示例:`partition`= 'ds=20180912,dt=xxxyyy'`|

**说明：** 实时计算数据写入odps的方式是每次做checkPoint的时候将缓存数据进行输入。

## 类型映射 {#section_llb_tbg_xgb .section}

|MaxCompute|Blink|
|----------|-----|
|TINYINT|TINYINT|
|SMALLINT|SMALLINT|
|INT|INT|
|BIGINT|BIGINT|
|FLOAT|FLOAT|
|DOUBLE|DOUBLE|
|BOOLEAN|BOOLEAN|
|DATETIME|TIMESTAMP|
|TIMESTAMP|TIMESTAMP|
|VARCHAR|VARCHAR|
|STRING|STRING|
|DECIMAL|DECIMAL|
|BINARY|VARBINARY|

## 常见问题 {#section_kql_5bg_xgb .section}

Q: Stream模式的ODPS Sink是否支持`isOverwrite`为`true`的情况？

A：`isOverwrite`为`true`，写入sink之前会把结果表或者结果数据清空。具体来说，就是每次启动后和暂停恢复后，写入之前会把原来结果表或者结果分区里的内容删除掉。流上暂停恢复后清空数据会导致丢数据。

