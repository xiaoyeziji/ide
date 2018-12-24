# 配置FTP Reader {#concept_zzv_ww3_p2b .concept}

本文为您介绍FTP Reader支持的数据类型、字段映射和数据源等参数及配置举例。

FTP Reader提供了读取远程FTP文件系统数据存储的能力。在底层实现上， FTP Reader获取远程FTP文件数据，并转换为数据同步传输协议传递给Writer。

本地文件内容存放的是一张逻辑意义上的二维表，例如CSV格式的文本信息。

FTP Reader实现了从远程FTP文件读取数据并转为数据同步协议的功能，远程FTP文件本身是无结构化数据存储，对于数据同步而言，目前FTP Reader支持的功能如下所示。

-   支持且仅支持读取TXT的文件，且要求TXT中shema为一张二维表。
-   支持类CSV格式文件，自定义分隔符。
-   支持多种类型数据读取（使用String表示），支持列裁剪，支持列常量。
-   支持递归读取、支持文件名过滤。
-   支持文本压缩，现有压缩格式为gzip、bzip2、zip、lzo、lzo\_deflate。
-   多个File可以支持并发读取。

暂时不支持以下两种功能。

-   单个File支持多线程并发读取，这里涉及到单个File内部切分算法。
-   单个File在压缩情况下，从技术上无法支持多线程并发读取。

远程FTP文件本身不提供数据类型，该类型是DataX FtpReader定义。

|DataX内部类型|远程FTP文件数据类型|
|:--------|:----------|
|Long|Long|
|Double|Double|
|String|String|
|Boolean|Boolean|
|Date|Date|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|path|远程FTP文件系统的路径信息，这里可以支持填写多个路径。-   当指定单个远程FTP文件，FTP Reader暂时只能使用单线程进行数据抽取。后期会在非压缩文件情况下针对单个File进行多线程并发读取。
-   当指定多个远程FTP文件，FTP Reader支持使用多线程进行数据抽取。线程并发数通过通道数指定。
-   当指定通配符，FTP Reader尝试遍历出多个文件信息。例如指定/代表读取/目录下所有的文件，指定/bazhen/\\代表读取bazhen目录下游所有的文件。FTP Reader目前只支持\*作为文件通配符。

**说明：** 

-   数据同步会将一个作业下同步的所有Text File视作同一张数据表。您必须自己保证所有的File能够适配同一套schema信息。
-   您必须保证读取文件为类CSV格式，并且提供给数据同步系统权限可读。
-   如果Path指定的路径下没有符合匹配的文件抽取，同步任务将报错。

 |是|无|
|column| 读取字段列表，type指定源数据的类型，index指定当前列来自于文本第几列（以0开始），value指定当前类型为常量，不从源头文件读取数据，而是根据value值自动生成对应的列。

 默认情况下，您可以全部按照String类型读取数据，配置为`"column": ["*"]`。您可以指定Column字段信息，配置如下：

```
{
    "type": "long",
    "index": 0    //从远程FTP文件文本第一列获取int字段
  },
  {
    "type": "string",
    "value": "alibaba"  //从FtpReader内部生成alibaba的字符串字段作为当前字段
  }
```

 对于您指定的Column信息，type必须填写，index/value必须选择其一。

 |是|全部按照String类型读取|
|fieldDelimiter|读取的字段分隔符。**说明：** FTP Reader在读取数据时，需要指定字段分割符，如果不指定会默认为“,”，界面配置也会默认填写“,”。

|是|,|
|skipHeader|类CSV格式文件可能存在表头为标题情况，需要跳过。默认不跳过，压缩文件模式下不支持skipHeader。|否|false|
|encoding|读取文件的编码配置。|否|utf-8|
|nullFormat|文本文件中无法使用标准字符串定义null（空指针），数据同步提供nullFormat定义哪些字符串可以表示为null。例如您配置`nullFormat:“null”`，那么如果源头数据是null，数据同步视作null字段。

|否|无|
|markDoneFileName|标档文件名，数据同步前检查标档文件。如果标档文件不存在，等待一段时间重新检查标档文件，如果检查到标档文件开始执行同步任务。|否|无|
|maxRetryTime|表示检查标档文件重试次数，默认重试60次，每一次重试间隔为1分钟，共60分钟。|否|60|
|csvReaderConfig|读取CSV类型文件参数配置，Map类型。读取CSV类型文件使用的CsvReader进行读取，会有很多配置，不配置则使用默认值。|否|无|
|fileFormat|读取的文件类型，默认情况下文件作为csv格式文件进行读取，内容被解析未逻辑上的二维表结构处理。如果您配置成binary，则表示按照纯粹二进制格式进行拷贝传输，这在FTP、OSS等存储之间进行目录结构对等拷贝时有用。您一般不需要配置此配置项。|否|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16222/15408845767697_zh-CN.png)

    配置项说明如下：

    -   数据源：即上述参数说明中的datasource，一般填写您配置的数据源名称。
    -   文件路径：即上述参数说明中的path。
    -   列分隔符：即上述参数说明中的fieldDelimiter，默认值为"，"。
    -   编码格式：即上述参数说明中的encoding，默认值为utf-8。
    -   null值：即上述参数说明中的nullFormat，定义表示null值的字符串。
    -   压缩格式：即上述参数说明中的compress，默认值为不压缩。
    -   是否包含表头：即上述参数说明中的skipHeader，默认值为No。
2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16222/15408845767701_zh-CN.png)

    -   同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。
    -   手动编辑源表字段：请手动编辑字段，一行表示一个字段，首尾空行会被采用，其他空行会被忽略。
3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16222/15408845767704_zh-CN.png)

    配置项说明如下：

    -   DMU：数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
    -   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度。
    -   错误记录数：错误记录数，表示脏数据的最大容忍条数。
    -   任务资源组：任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从FTP数据库同步抽取数据作业。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"ftp",//插件名
            "parameter":{
                "path":[],//文件路径
                "nullFormat":"",//null值
                "compress":"",//压缩格式
                "datasource":"",//数据源
                "column":[//字段
                    {
                        "index":0,//序列号
                        "type":""//字段类型
                    }
                ],
                "skipHeader":"",//是否包含表头
                "fieldDelimiter":",",//列分隔符
                "encoding":"UTF-8",//编码格式
                "fileFormat":"csv"//文本类型
            },
            "name":"Reader",
            "category":"reader"
        },
        {//下面是关于Reader的模板，可以找相应的读插件文档
            "stepType":"stream",
            "parameter":{},
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record":"0"//错误记录数
        },
        "speed":{
            "throttle":false,//false代表不限流，下面的限流的速度不生效，true代表限流
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

