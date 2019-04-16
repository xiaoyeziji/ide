# UDTF {#concept_gxj_lwf_xgb .concept}

UDTF组件即自定义函数组件，相当于SQL中的[UDTF](../../../../../cn.zh-CN/用户指南/SQL/UDF/UDF概述.md#)子句。

**说明：** UDTF组件仅实时计算服务的**独享模式**才支持。

## 配置面板说明 {#section_ws5_qwf_xgb .section}

![UDTF](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/131687/155134193839597_zh-CN.png)

|参数|注释说明|
|--|----|
|JOIN模式|支持INNER JOIN和LEFT OUTER JOIN。-   INNER JOIN：UDTF有结果才返回
-   LEFT OUTER JOIN：UDTF无结果补NULL

|
|选择函数|选择函数名称。您需要先在资源引用中上传程序包，然后在资源引用面板中勾选程序包，以表达当前任务引用了此程序包。|
|参数表达式|选择函数后填写对应的输入、输出参数。|
|选择输出字段|选择要输出的字段，这里可以定义字段名、字段别名和字段表达式。|

