# 配置LogHub Writer {#concept_byy_4xt_q2b .concept}

LogHub Writer使用SLS的Java SDK，可以将DataX Reader中的数据推送到指定的SLS LogHub上，供其他程序消费。

**说明：** 

由于LogHub无法实现幂等，FailOver重跑任务时会引起数据重复。

## 实现原理 {#section_a3m_424_q2b .section}

LogHub Writer通过Datax框架获取Reader生成的数据，然后将Datax支持的类型通过逐一判断转换成String类型。当达到您指定的batchSize时，会使用SLS Java SDK一次性推送至LogHub。默认情况下，一次推送1024条数据。batchSize值最大为4096。

LogHub Writer针对LogHub类型的转换，如下表所示：

|DataX内部类型|LogHub数据类型|
|:--------|:---------|
|Long|String|
|Double|String|
|String|String|
|Date|String|
|Boolean|String|
|Bytes|String|

## 参数说明 {#section_v2z_pzt_q2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|endpoint|sls地址|是|无|
|accessKeyId|访问SLS的AccessKeyId|是|无|
|accessKeySecret|访问SLS的AccessKeySecret|是|无|
|project|目标SLS的Project名字|是|无|
|logstore|目标SLS LogStore的名字|是|无|
|topic|选取topic|否|空字符串|
|batchSize|一次推送发送的数据条数|否，默认为1024|无|
|column|每条数据中column名字|是|无|

## 向导模式介绍 {#section_pcz_fh4_q2b .section}

暂不支持向导模式配置，您可以接点击链接转换为脚本模式或者选择导入脚本模板进行开发。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16257/15353704428506_zh-CN.png)

## 脚本模式介绍 {#section_gg5_1b5_q2b .section}

提供脚本配置样例如下所示，具体参数填写请参见上述的参数说明。

```
{
    "type": "job",
    "version": "2.0",//版本号
    "steps": [
        { //下面是关于Reader的模板，可以找相应的读插件文档
            "stepType": "stream",
            "parameter": {},
            "name": "Reader",
            "category": "reader"
        },
        {
            "stepType": "loghub",//插件名
            "parameter": {
                "datasource": "",//数据源
                "column": [//字段
                    "col0",
                    "col1",
                    "col2",
                    "col3",
                    "col4",
                    "col5"
                ],
                "topic": "",//选取topic
                "batchSize": "1024",//一次性批量提交的记录数大小
                "logstore": ""//目标SLS LogStore的名字
            },
            "name": "Writer",
            "category": "writer"
        }
    ],
    "setting": {
        "errorLimit": {
            "record": ""//错误记录数
        },
        "speed": {
            "concurrent": 3,//作业并发数
            "throttle": false,//false代表不限流，下面的限流的速度不生效，true代表限流
            "dmu": 1//DMU值
        }
    },
    "order": {
        "hops": [
            {
                "from": "Reader",
                "to": "Writer"
            }
        ]
    }
}
```

