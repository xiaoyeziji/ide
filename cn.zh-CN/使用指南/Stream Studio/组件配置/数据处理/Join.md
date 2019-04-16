# Join {#concept_glb_pvf_xgb .concept}

Join组件即SQL中的[Join](../../../../../cn.zh-CN/用户指南/SQL/SELECT操作/JOIN.md#)子句。

## 配置面板说明 {#section_gbr_wvf_xgb .section}

![Join](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/131685/155123403039596_zh-CN.png)

|参数|注释说明|
|--|----|
|Join模式|选择要采用的JOIN模式，支持INNER JOIN、LEFT OUTER JOIN、RIGHT OUTER JOIN、FULL OUTER JOIN|
|表达式|JOIN表达式，只支持等值连接，不支持不等连接，参考格式：leftId = rightId AND limit = 0|
|选择字段|选择要输出的字段，相当于要Select的字段|

