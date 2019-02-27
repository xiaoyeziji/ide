# Log Service（SLS） {#concept_dsw_xls_wgb .concept}

日志服务（Log Service），简称LOG，原SLS。是针对实时数据的一站式服务，经历大量大数据场景锤炼而成。无需开发就能快捷完成数据采集、消费、投递以及查询分析等功能，帮助提升运维、运营效率，建立DT时代海量日志处理能力。 日志服务本身是流数据存储，实时计算能将其作为流式数据输入。更多信息请参考[创建日志服务（Log Service）源表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据源表/创建日志服务（Log Service）源表.md#)

对于日志服务而言，数据格式类似JSON，示例如下。

```
{
	"a": 1000,
	"b": 1234,
	"c": "li"
}
```

## 配置面板说明 {#section_q13_zpf_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|列定义|要读取的Log Service内容字段和属性字段列表|属性字段说明见下文|
|消费端点信息|消费端点信息|[服务入口](../../../../../cn.zh-CN/API 参考/服务入口.md#)|
|AccessKey ID|AccessKey ID|无|
|AccessKey Secret|AccessKey Secret|无|
|读取的sls项目|读取的sls项目|无|
|logStore|project下的具体的logStore|无|
|消费组名|消费组名|用户可以自定义消费组名\(没有固定格式\)|
|消费日志开始的时间点|消费日志开始的时间点|无|
|消费客户端心跳间隔时间|可选，消费客户端心跳间隔时间，单位ms|无|
|读取最大尝试次数|读取最大尝试次数|无|
|调试开关|如果选中，会把解析异常的log打印出来|无|

## 类型映射 {#section_jyd_jqf_xgb .section}

日志服务和实时计算字段类型对应关系，建议您使用该对应关系进行DDL声明:

|日志服务字段类型|实时计算字段类型|
|--------|--------|
|STRING|VARCHAR|

## 属性字段 {#section_pnf_qqf_xgb .section}

目前默认支持三个属性字段的获取，也支持其他自定义写入的字段。

|字段名|注释说明|
|---|----|
|`__source__`|消息源|
|`__topic__`|消息主题|
|`__timestamp__`|日志时间|

**说明：** 

-   SLS暂不支持MAP类型的数据。
-   字段顺序支持无序（建议字段顺序和表中定义一致）。
-   输入数据源为JSON形式时，注意定义分隔符，并且需要采用内置函数分析JSON\_VALUE，否则就会解析失败。报错如下：

    ```
    2017-12-25 15:24:43,467 WARN [Topology-0 (1/1)] com.alibaba.blink.streaming.connectors.common.source.parse.DefaultSourceCollector - Field missing error, table column number: 3, data column number: 3, data filed number: 1, data: [{"lg_order_code":"LP00000005","activity_code":"TEST_CODE1","occur_time":"2017-12-10 00:00:01"}]
    ```

-   batchGetSize设置不能超过1000，否则会报错。
-   batchGetSize指明的是logGroup获取的数量，如果单条logItem的大小和 batchGetSize都很大，很有可能会导致频繁的GC，这种情况下该参数应调小。

