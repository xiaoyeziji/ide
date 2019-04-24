# 配置OSS Writer {#concept_fw1_2ln_q2b .concept}

本文为您介绍OSS Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

OSS Writer插件提供了向OSS写入类CSV格式的一个或者多个表文件的功能。

**说明：** 开始配置OSS Writer插件前，请首先配置好数据源，详情请参见[配置OSS数据源](intl.zh-CN/使用指南/数据集成/数据源配置/配置OSS数据源.md#)。

写入OSS内容存放的是一张逻辑意义上的二维表，例如CSV格式的文本信息。如果您想对OSS产品有更深入的了解，请参见[OSS产品概述](../../../../intl.zh-CN/产品简介/什么是对象存储 OSS.md#)。

OSS Writer实现了从数据同步协议转为OSS中的文本文件功能，OSS本身是无结构化数据存储，目前OSS Writer支持的功能如下所示：

-   支持且仅支持写入文本文件，并要求文本文件中的shema为一张二维表。
-   支持类CSV格式文件，自定义分隔符。
-   支持多线程写入，每个线程写入不同子文件。
-   文件支持滚动，当文件大于某个size值时，支持文件切换。当文件大于某个行数值时，支持文件切换。

OSS Writer暂时不能实现以下功能：

-   单个文件不能支持并发写入。
-   OSS本身不提供数据类型，OSS Writer均以String类型写入OSS。

OSS本身不提供数据类型，下表中的类型是Data

|类型分类|OSS数据类型|
|:---|:------|
|整数类|Long|
|浮点类|Double|
|字符串类|String|
|布尔类|Bool|
|日期时间类|Date|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|ject|OSS Writer写入的文件名，OSS使用文件名模拟目录的实现。例如数据同步到OSS数据源中的bucket为test118的test文件夹。 则Object只需填写test，不要带上bucket名称，同步到OSS端的文件名与源端填写的文件名相同。

 使用"ject": "test/DI"，写入的Object以test/DI开头。test为文件夹，DI为文件名称的开头，后缀随机添加字符串，/是OSS模拟目录的分隔符。

 |是|无|
|writeMode|OSS Writer写入前数据清理处理。 -   truncate：写入前清理Object名称前缀匹配的所有Object。例如`"object":"abc"`，将清理所有abc开头的Object。
-   append：写入前不进行任何处理，数据集成OSS Writer直接使用Object名称写入，并使用随机UUID的后缀名来保证文件名不冲突。例如您指定的Object名为数据集成，实际写入为DI\_\*\*\*\*\_\*\*\*\*\_\*\*\*\*。
-   nonConflict：如果指定路径出现前缀匹配的Object，直接报错。例如`"object":"abc"`，如果存在abc123的Object，将直接报错。

 |是|无|
|fileFormat|文件写出的格式，包括csv和text两种。 -   csv是严格的csv格式，如果待写数据包括列分隔符，则会按照csv的转义语法转义，转义符号为双引号（"）。
-   text格式是用列分隔符简单分割待写数据，对于待写数据包括列分隔符情况下不做转义。

 |否|text|
|fieldDelimiter|读取的字段分隔符。|否|,|
|encoding|写出文件的编码配置。|否|utf-8|
|nullFormat|文本文件中无法使用标准字符串定义null（空指针），数据同步系统提供nullFormat定义哪些字符串可以表示为null。例如您配置`nullFormat="null"`，那么如果源头数据是"null"，数据同步系统会视作null字段。|否|无|
|header（高级配置，向导模式不支持）|OSS写出时的表头，例如\['id', 'name', 'age'\]。|否|无|
|maxFileSize（高级配置，向导模式不支持）|OSS写出时单个Object文件的最大值，默认为10000\*10MB，类似于log4j日志打印时根据日志文件大小轮转。OSS分块上传时，每个分块大小为10MB（也是日志轮转文件最小粒度，即小于10MB的maxFileSize会被作为10MB），每个OSS InitiateMultipartUploadRequest支持的分块最大数量为10000。 轮转发生时，Object名字规则是在原有Object前缀加UUID随机数的基础上，拼接\_1,\_2,\_3等后缀。

 |否|100000MB|
|suffix（高级配置，向导模式不支持）|数据同步写出时，生成的文件名后缀，比如配置suffix为.csv，则最终写出的文件名为fileName\*\*\*\*.csv。|否|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15561010367815_zh-CN.png)

    |配置|说明|
    |--|--|
    |**数据源**|即上述参数说明中的datasource，一般填写您配置的数据源名称。|
    |**Object前缀**|即上述参数说明中的Object，填写OSS文件夹的路径，其中不要填写bucket的名称。|
    |**文本类型**|包括csv和text两种文本类型。|
    |**列分隔符**|即上述参数说明中的fieldDelimiter，默认值为（,）。|
    |**编码格式**|即上述参数说明中的encoding，默认值为utf-8。|
    |**null值**|即上述参数说明中的nullFormat，将要表示为空的字段填入文本框，如果源端存在则将对应的部分转换为空。|
    |**时间格式**|日期类型的数据序列化到Object时的格式，例如 "dateFormat": "yyyy-MM-dd"。|
    |**前缀冲突**|有同样的文件时，可以选择替换、保留或报错。|

2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，鼠标放至需要删除的字段上，即可单击**删除**图标进行删除 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15561010387818_zh-CN.png)

    同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。

3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15561010387675_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**DMU**|数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。|
    |**作业并发数**|可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度。|
    |**同步速率**|设置同步速率可保护读取端数据库，以避免抽取速度过大，给源库造成太大的压力。同步速率建议限流，结合源库的配置，请合理配置抽取速率。|
    |**错误记录数**|表示脏数据的最大容忍条数。|
    |**任务资源组**|任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增任务资源](intl.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。|


## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

脚本配置样例如下所示，具体参数填写请参见参数说明。

```
{
    "type":"job",
    "version":"2.0",
    "steps":[
        { //下面是关于Reader的模板，可以找相应的读插件文档。
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType":"oss",//插件名
            "parameter":{
                "nullFormat":"",//数据同步系统提供nullFormat，定义哪些字符串可以表示为null。
                "dateFormat":"",//日期格式
                "datasource":"",//数据源
                "writeMode":"",//写入模式
                "encoding":"",//编码格式
                "fieldDelimiter":","//字段分隔符
                "fileFormat":"",//文本类型
                "object":""//Object前缀
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record":"0"//错误记录数
        },
        "speed":{
            "throttle":false,//false代表不限流，下面的限流的速度不生效，true代表限流。
            "concurrent":1,//作业并发数
            "dmu":1//DMU值
        }
    },
    "order":{
        "hops":[
            {
                "from":"Reader",
                "to":"Writer"
            }
        ]
    }
}
```

