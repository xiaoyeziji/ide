# 配置OSS Reader {#concept_d33_12q_p2b .concept}

OSS Reader插件提供了读取OSS数据存储的能力。在底层实现上，OSS Reader使用OSS官方Java SDK获取OSS数据，并转换为数据同步传输协议传递给Writer。

-   如果您想对OSS产品有更深了解，请参见[OSS产品概述](https://www.alibabacloud.com/help/doc-detail/31817.htm)。
-   OSS Java SDK的详细介绍，请参见[阿里云OSS Java SDK](http://oss.aliyuncs.com/aliyun_portal_storage/help/oss/OSS_Java_SDK_Dev_Guide_20141113.pdf)。
-   处理OSS等非结构化数据的详细介绍，请参见[处理非结构化数据](https://www.alibabacloud.com/help/doc-detail/45389.htm)。

OSS Reader实现了从OSS读取数据并转为数据集成/datax协议的功能，OSS本身是无结构化数据存储，对于数据集成/datax而言，目前OSS Reader支持的功能如下所示。

-   支持且仅支持读取TXT格式的文件，且要求TXT中shema为一张二维表。
-   支持类CSV格式文件，自定义分隔符。
-   支持多种类型数据读取（使用String表示），支持列裁剪，支持列常量。
-   支持递归读取、支持文件名过滤。
-   支持文本压缩，现有压缩格式为gzip、bzip2和zip。

    **说明：** 一个压缩包不允许多文件打包压缩。

-   多个Object可以支持并发读取。

OSS Reader暂时不能实现以下功能。

-   单个Object（File）支持多线程并发读取。
-   单个Object在压缩情况下，从技术上无法支持多线程并发读取。

OSS Reader支持OSS中的BIGINT、DOUBLE、STRING、DATATIME和BOOLEAN数据类型。

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|Object|OSS的Object信息，此处可以支持填写多个Object。例如xxx的bucket中有yunshi文件夹，文件夹中有ll.txt文件，则Object直接填yunshi/ll.txt。-   当指定单个OSS Object时，OSS Reader暂时只能使用单线程进行数据抽取。后期将考虑在非压缩文件情况下针对单个Object可以进行多线程并发读取。
-   当指定多个OSS Object时，OSS Reader支持使用多线程进行数据抽取。线程并发数通过通道数指定。
-   当指定通配符时，OSS Reader尝试遍历出多个Object信息。详情请参见[OSS产品概述](https://www.alibabacloud.com/help/doc-detail/31827.htm)。

**说明：** 数据同步系统会将一个作业下同步的所有Object视作同一张数据表。您必须保证所有的Object能够适配同一套schema信息。

|是|无|
|column|读取字段列表，type指定源数据的类型，index指定当前列来自于文本第几列（以0开始），value指定当前类型为常量，不是从源头文件读取数据，而是根据value值自动生成对应的列。默认情况下，您可以全部按照String类型读取数据，配置如下：

```
json
"column": ["*"]
```

您可以指定Column字段信息，配置如下：

```
json
"column":
    {
       "type": "long",
       "index": 0    //从OSS文本第一列获取int字段
    },
    {
       "type": "string",
       "value": "alibaba"  //从 OSSReader 内部生成 alibaba 的字符串字段作为当前字段
    }
```

**说明：** 对于您指定的Column信息，type必须填写，index/value必须选择其一。

|是|全部按照String类型读取|
|fieldDelimiter|读取的字段分隔符。**说明：** OSS Reader在读取数据时，需要指定字段分割符，如果不指定默认为',;，界面配置中也会默认填写为','。

|是|,|
|compress|文本压缩类型，默认不填写（即不压缩）。支持压缩类型为gzip、bzip2和zip。|否|不压缩|
|encoding|读取文件的编码配置。|否|utf-8|
|nullFormat|文本文件中无法使用标准字符串定义null（空指针），数据同步系统提供nullFormat定义哪些字符串可以表示为null。例如您配置`nullFormat="null"`，那么如果源头数据是"null"，数据同步系统会视作null字段。|否|无|
|skipHeader|类CSV格式文件可能存在表头为标题情况，需要跳过。默认不跳过，压缩文件模式下不支持skipHeader。|否|false|
|csvReaderConfig|读取CSV类型文件参数配置，Map类型。读取CSV类型文件使用的CsvReader进行读取，会有很多配置，不配置则使用默认值。|否|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15367217887815_zh-CN.png)

    配置项说明如下：

    -   数据源：即上述参数说明中的datasource，一般填写您配置的数据源名称。
    -   Object前缀：即上述参数说明中的Object。

        **说明：** 假如您的OSS文件名有根据每天的时间命名的部分，例如aaa/20171024abc.txt，关于Object系统参数就可以设置aaa/$\{bdp.system.bizdate\}abc.txt。

    -   列分隔符：即上述参数说明中的fieldDelimiter，默认值为","。
    -   编码格式：即上述参数说明中的encoding，默认值为utf-8。
    -   null值：即上述参数说明中的nullFormat，将要表示为空的字段填入文本框，如果源端存在则将对应的部分转换为空。
    -   压缩格式：即上述参数说明中的compress，默认值为不压缩。
    -   是否包含表头：即上述参数说明中的skipHeader，默认值为No。
2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16229/15367217887818_zh-CN.png)

    -   同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。
    -   手动编辑源表字段：请手动编辑字段，一行表示一个字段，首尾空行会被采用，其他空行会被忽略。
3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15367217887675_zh-CN.png)

    配置项说明如下：

    -   DMU：数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
    -   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度
    -   错误记录数：错误记录数，表示脏数据的最大容忍条数。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

脚本配置样例如下所示，具体参数填写请参见参数说明。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"oss",//插件名
            "parameter":{
                "nullFormat":"",//nullFormat 定义哪些字符串可以表示为 null
                "compress":"",//文本压缩类型
                "datasource":"",//数据源
                "column":[//字段
                    {
                        "index":0,//列序号
                        "type":"string"//数据类型
                    },
                    {
                        "index":1,
                        "type":"long"
                    },
                    {
                        "index":2,
                        "type":"double"
                    },
                    {
                        "index":3,
                        "type":"boolean"
                    },
                    {
                        "format":"yyyy-MM-dd HH:mm:ss", //时间格式
                        "index":4,
                        "type":"date"
                    }
                ],
                "skipHeader":"",//类 CSV 格式文件可能存在表头为标题情况，需要跳过
                "encoding":"",//编码格式
                "fieldDelimiter":",",//字段分隔符
                "fileFormat": "",//文本类型
                "object":[]//object前缀
            },
            "name":"Reader",
            "category":"reader"
        },
        {//下面是关于Writer的模板，可以找相应的写插件文档
            "stepType":"stream",
            "parameter":{},
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record":""//错误记录数
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

