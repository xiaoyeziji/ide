# 配置DataHub Writer {#concept_kyh_52l_q2b .concept}

DataHub是实时数据分发平台，流式数据（Streaming Data）的处理平台，提供对流式数据的发布（Publish）、订阅（Subscribe）和分发功能，让您可以轻松构建基于流式数据的分析和应用。

DataHub服务基于阿里云自研的飞天平台，具有高可用、低延迟、高可扩展和高吞吐的特点，它与阿里云流计算引擎StreamCompute无缝连接，您可以轻松使用SQL进行流数据分析。它也提供分发流式数据到各种云产品的功能，目前支持分发到MaxCompute（原ODPS）、OSS等。

**说明：** String字符串仅支持UTF-8编码，单个String列最长允许1MB。

## 参数配置 {#section_mdl_dfl_q2b .section}

通过Channel将Source与Sink连接起来，所以在Writer端的Channel要对应Reader端的Channel类型，一般Channel有Memory-Channel和File-channel两种类型，如下配置就是file通道。

```
"agent.sinks.dataXSinkWrapper.channel": "file"
```

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|accessId|DataHub的accessId。|是|无|
|accessKey|DataHub的accessKey。|是|无|
|endpoint| ，对DataHub资源的访问请求，需根据资源所属服务，选择正确的域名。

 |是|无|
|maxRetryCount|任务失败的重试的最多次数。|否|无|
|mode|value是String类型时，写入的模式。|是|无|
|parseContent|解析内容。|是|无|
|project|项目（Project）是DataHub数据的基本组织单元，下面包含多个Topic。**说明：** DataHub的项目空间与MaxCompute的项目空间是相互独立的，您在MaxCompute中创建的项目不能复用于DataHub，需要独立创建。

|是|无|
|topic| Topic是DataHub订阅和发布的最小单位，您可以用Topic来表示一类或者一种流数据。

 |是|无|
|maxCommitSize|为提高写出效率，DataX-On-Flume会攒Buffer数据，待攒数据大小达到maxCommitSize大小（单位MB）时，批量提交到目的端。默认是1048576 ，即1MB数据。|否|1MB|
|batchSize|为提高写出效率，DataX-On-Flume会攒Buffer数据，待攒数据条数达到batchSize大小（单位条数）时，批量提交到目的端。默认是1024，即1024条数据。|否|1024|
|maxCommitInterval|为提高写出效率，DataX-On-Flume会攒Buffer数据，待攒数据条数达到maxCommitSize、batchSize大小限制时，批量提交到目的端。如果数据采集源头长时间没有产出数据，为了保证数据的及时投递，增加了maxCommitInterval参数（单位毫秒），即Buffer数据的最长时间，超过此时间会强制投递。默认是30000，即30秒。|否|30|
|parseMode|日志解析模式，目前有不解析default模式和csv模式，所谓不解析即采集到的一行日志，直接作为DataX的Record单列Column写出。csv模式支持配置一个列分隔符，一行日志通过分隔符分隔成DataX的Record的多列。|否|default|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂时不支持向导开发模式。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从内存中读数据的同步作业。

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
            "stepType": "datahub",//插件名
            "parameter": {
                "datasource": "",//数据源
                "topic": "",//Topic 是 DataHub 订阅和发布的最小单位，您可以用 Topic 来表示一类或者一种流数据。
                "maxRetryCount": 500,//任务失败的重试的最多次数。
                "maxCommitSize": 1048576//待攒数据到Buffer大小达到 maxCommitSize 大小（单位 MB）时，批量提交到目的端
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
            "concurrent": 20,//并发线程数
            "throttle": false,//false代表不限流，下面的限流的速度不生效，true代表限流
            "dmu": 20//DMU值
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

