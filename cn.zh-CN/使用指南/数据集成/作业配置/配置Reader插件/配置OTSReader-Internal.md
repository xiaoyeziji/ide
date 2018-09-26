# 配置OTSReader-Internal {#concept_cpg_nzq_p2b .concept}

表格存储（Table Store，简称OTS）是构建在阿里云飞天分布式系统之上的NoSQL数据库服务，提供海量结构化数据的存储和实时访问。OTS以实例和表的形式组成数据，通过数据分片和负载均衡技术，实现规模上的无缝扩展。

OTSReader-Internal主要用于OTS Internal模型的表数据导出，而另外一个插件OTS Reader则用于OTS Public模型的数据导出。

OTS Internal模型支持多版本列，所以该插件也提供两种模式数据的导出。

-   多版本模式：因为OTS本身支持多版本，特此提供一个多版本模式，将多版本的数据导出。

    导出方案：Reader插件将OTS的一个Cell展开为一个一维表的4元组，分别是主键（PrimaryKey，包含1-4列）、ColumnName、Timestamp和Value（原理和HBase Reader的多版本模式类似），将这个4元组作为Datax record中的4个Column传输给消费端（Writer）。

-   普通模式：和HBase Reader普通模式一致，只需导出每行数据中每列的最新版本的值，详情请参见[配置HBase Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置HBase Reader.md#)中HBase Reader支持的normal模式内容。

简而言之，OTS Reader通过OTS官方Java SDK连接到OTS服务端，并通过SDK读取数据。OTS Reader本身对读取过程做了很多优化，包括读取超时重试、异常读取重试等Feature。

目前OTS Reader支持所有OTS类型，OTSReader-Internal针对OTS类型的转换列表，如下所示。

|数据集成内部类型|OTS数据类型|
|:-------|:------|
|Long|Integer|
|Double|Double|
|String|String|
|Boolean|Boolean|
|Bytes|Binary|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|mode|插件的运行方式，支持normal和multiVersion，分别表示普通模式和多版本模式。|是|无|
|endpoint|OTS Server的EndPoint（服务地址）。|是|无|
|accessId|OTS的accessId|是|无|
|accessKey|OTS的AccessKey|是|无|
|instanceName|OTS的实例名称，实例是您使用和管理OTS服务的实体。您在开通OTS服务后，需要通过管理控制台来创建实例，然后在实例内进行表的创建和管理。实例是OTS资源管理的基础单元，OTS对应用程序的访问控制和资源计量都在实例级别完成。

|是|无|
|table|选取的需要抽取的表名称，这里有且只能填写一张表。在OTS中不存在多表同步的需求。|是|无|
|range|导出的范围，读取的范围是\[begin,end\)，左闭右开的区间。-   begin小于end，表示正序读取数据。
-   begin大于end，表示反序读取数据。
-   begin和end不能相等。
-   type支持的类型有string、int和binary，binary输入的方式采用二进制的Base64字符串形式传入，INF\_MIN表示无限小，INF\_MAX表示无限大。

|否|从表的开始位置读取到表的结束位置|
|range:\{"begin"\}|导出的起始范围，这个值的输入可以填写空数组或PK前缀，也可以填写完整的 PK。正序读取数据时，默认填充PK后缀为INF\_MIN，反序为INF\_MAX，示例如下。如果您的表有2个PrimaryKey，类型分别为string和int，则有以下3种输入方法。

-   \[\]表示从表的开始位置读取。
-   \[\{“type”:”string”, “value”:”a”\}\] 表示从\[\{“type”:”string”, “value”:”a”\},\{“type”:”INF\_MIN”\}\]。
-   \[\{“type”:”string”, “value”:”a”\},\{“type”:”INF\_MIN”\}\]

binary类型的PrimaryKey列比较特殊，因为JSON不支持直接输入二进制数，所以系统定义：如果您要传入二进制，必须使用（Java）Base64.encodeBase64String方法，将二进制转换为一个可视化的字符串，然后将这个字符串填入value中，Java示例如下。

-   `byte[] bytes = “hello”.getBytes();`：构造一个二进制数据，这里使用字符串hello的byte值。
-   `String inputValue = Base64.encodeBase64String(bytes)`：调用Base64方法，将二进制转换为可视化的字符串。

上面的代码执行之后，可以获得inputValue为"aGVsbG8="。

最终写入配置\{“type”:”binary”,”value” : “aGVsbG8=”\}。

|否|从表的开始位置读取数据|
|range:\{“end”\}|导出的结束范围，这个值的输入可以填写空数组或PK前缀，也可以填写完整的 PK。正序读取数据时，默认填充PK后缀为INF\_MAX，反序为INF\_MIN。如果您的表有2个PK，类型分别为string、int，那么如下3种输入都合法，如下所示。

-   \[\]表示从表的开始位置读取。
-   \[\{“type”:”string”, “value”:”a”\}\] 表示从\[\{“type”:”string”, “value”:”a”\},\{“type”:”INF\_MIN”\}\]。
-   \[\{“type”:”string”, “value”:”a”\},\{“type”:”INF\_MIN”\}\]。

binary类型的PrimaryKey列比较特殊，因为JSON不支持直接输入二进制数，所以系统定义：如果您要传入二进制，必须使用（Java）Base64.encodeBase64String方法，将二进制转换为一个可视化的字符串，然后将这个字符串填入value中，Java示例如下。-   `byte[] bytes = “hello”.getBytes();`：构造一个二进制数据，这里使用字符串hello的byte值。
-   `String inputValue = Base64.encodeBase64String(bytes)`：调用Base64方法，将二进制转换为可视化的字符串。

上面的代码执行之后，可以获得inputValue为"aGVsbG8="。

最终写入配置：\{"type":"binary", "value":"aGVsbG8="\}。

|否|读取到表的结束位置|
|range:\{"split"\}|当前用户数据较多时，需要开启并发导出，Split可以将当前范围的的数据按照切分点切分为多个并发任务。**说明：** 

-   split中的输入值只能PK的第一列（分片建），且值的类型必须和PartitionKey一致。
-   值的范围必须在begin和end之间。
-   split内部的值必须根据begin和end的正反序关系而递增或者递减。

|否|空切分点|
|column|指定要导出的列，支持普通列和常量列。格式（支持多版本模式）：

普通列格式：\{“name”:”\{your column name\}”\}

| | |
|timeRange（仅支持多版本模式）|请求数据的Time Range，读取的范围是 \[begin,end\)，左闭右开的区间。**说明：** begin必须小于end。

|否|默认读取全部版本|
|timeRange:\{"begin"\}（仅支持多版本模式）|请求数据的Time Range开始时间，取值范围是0~LONG\_MAX。|否|默认为0|
|timeRange:\{"end"\} （仅支持多版本模式）|请求数据的Time Range结束时间，取值范围是0~LONG\_MAX。|否|默认为 Long Max\(9223372036854775806L\)|
|maxVersion （仅支持多版本模式）|请求的指定Version，取值范围是1~INT32\_MAX。|否|默认读取所有版本|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂不支持向导模式开发。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

多版本模式

```
{
    "type": "job",
    "version": "1.0",
    "configuration": {
        "reader": {
            "plugin": "otsreader-internalreader",
            "parameter": {
                "mode": "multiVersion",
                "endpoint": "",
                "accessId": "",
                "accessKey": "",
                "instanceName": "",
                "table": "",
                "range": {
                    "begin": [
                        {
                            "type": "string",
                            "value": "a"
                        },
                        {
                            "type": "INF_MIN"
                        }
                    ],
                    "end": [
                        {
                            "type": "string",
                            "value": "g"
                        },
                        {
                            "type": "INF_MAX"
                        }
                    ],
                    "split": [
                        {
                            "type": "string",
                            "value": "b"
                        },
                        {
                            "type": "string",
                            "value": "c"
                        }
                    ]
                },
                "column": [
                    {
                        "name": "attr1"
                    }
                ],
                "timeRange": {
                    "begin": 1400000000,
                    "end": 1600000000
                },
                "maxVersion": 10
            }
        }
    },
    "writer": {}
}
```

普通模式

```
{
    "type": "job",
    "version": "1.0",
    "configuration": {
        "reader": {
            "plugin": "otsreader-internalreader",
            "parameter": {
                "mode": "normal",
                "endpoint": "",
                "accessId": "",
                "accessKey": "",
                "instanceName": "",
                "table": "",
                "range": {
                    "begin": [
                        {
                            "type": "string",
                            "value": "a"
                        },
                        {
                            "type": "INF_MIN"
                        }
                    ],
                    "end": [
                        {
                            "type": "string",
                            "value": "g"
                        },
                        {
                            "type": "INF_MAX"
                        }
                    ],
                    "split": [
                        {
                            "type": "string",
                            "value": "b"
                        },
                        {
                            "type": "string",
                            "value": "c"
                        }
                    ]
                },
                "column": [
                    {
                        "name": "pk1"
                    },
                    {
                        "name": "pk2"
                    },
                    {
                        "name": "attr1"
                    },
                    {
                        "type": "string",
                        "value": ""
                    },
                    {
                        "type": "int",
                        "value": ""
                    },
                    {
                        "type": "double",
                        "value": ""
                    },
                    {
                        "type": "binary",
                        "value": "aGVsbG8="
                    }
                ]
            }
        }
    },
    "writer": {}
}
```

