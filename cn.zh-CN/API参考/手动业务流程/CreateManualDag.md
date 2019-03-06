# CreateManualDag {#concept_ub3_gsv_kfb .concept}

CreateManualDag：触发手动业务流程。

## 描述 {#section_c2z_1xn_fgb .section}

CreateManualDag用于触发生成新的手动业务流程实例。

## 请求参数 {#section_e3h_ttv_kfb .section}

|参数名|参数位置|类型|是否必选|顺序|示例值|描述|
|:--|:---|:-|:---|:-|:--|:-|
|**ProjectName**|Query|String|是|200|test\_project|项目英文名称|
|**FowName**|Query|String|是|200|test\_flow|手动业务流程名|
|**Bizdate**|Query|String|是|200|2018-12-12 00:00:00|业务日期，格式:`"yyyy-MM-dd hh:mm:ss"`|
|**DagPara**|Query|String|否|200|param\_k1:param\_v1 param\_k2:param\_v2|业务流程参数，格式: `{"key1":"value1"，"key2":"value2"}`|
|**NodePara**|Query|String|否|200|\{"103180025": "test=$\[yyyy-mm-dd\]"\}|节点参数，节点ID到节点参数的map。节点有多个参数的，在节点参数值里用空格分开。格式:`"节点ID": "key1=value1 key2=value2"`|

## 请求示例 {#section_ydn_b5v_kfb .section}

```
/?Bizdate=2018-12-12 00:00:00
&FlowName=test_flow
&ProjectName=test_project
&DagPara=param_k1:param_v1 param_k2:param_v2
&NodePara={"103180025": "test=$[yyyy-mm-dd]"}
&<公共请求参数>
```

## 返回参数 {#section_whb_zc4_fgb .section}

|参数名|结构类型|数据类型|顺序|示例值|描述|
|:--|:---|:---|:-|:--|:-|
|**RequestId**|Normal|String|200|2d9ced66-38ef-4923-baf6-391dd3a7e656|请求ID|
|**ReturnCode**|Normal|String|200|0|响应代码编号，0标识成功|
|**ReturnErrorSolution**|Normal|String|200|test|如果异常，这里会显示异常的解决方案信息。|
|**ReturnMessage**|Normal|String|200|test|异常信息|
|**ReturnValue**|Normal|Long|200|1244311235|返回值，这里是返回的手动业务流程的运行实例ID。|

## 成功返回示例JSON {#section_npn_kd4_fgb .section}

```
{
      "RequestId": "2d9ced66-38ef-4923-baf6-391dd3a7e656",
      "ReturnCode": "0",
      "ReturnErrorSolution": "",
      "ReturnMessage": "",
      "ReturnValue": 1244311235
}
```

