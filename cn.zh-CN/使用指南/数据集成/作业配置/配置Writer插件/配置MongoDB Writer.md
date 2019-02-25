# 配置MongoDB Writer {#concept_gmn_qwm_q2b .concept}

本文为您介绍MongoDB Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

MongoDB Writer插件利用MongoDB的Java客户端MongoClient进行MongoDB的写操作。最新版本的Mongo已经将DB锁的粒度从DB级别降低到Document级别，配合MongoDB强大的索引功能，基本可以满足数据源向MongoDB写入数据的需求。针对数据更新的需求，通过配置业务主键的方式也可以实现。

**说明：** 

-   在开始配置MongoDB Writer插件前，请首先配置好数据源，详情请参见[配置MongoDB数据源](intl.zh-CN/使用指南/数据集成/数据源配置/配置MongoDB数据源.md#)。
-   如果您使用的是云数据库MongoDB版，MongoDB默认会有root账号。
-   出于安全策略的考虑，数据集成仅支持使用 MongoDB数据库对应账号进行连接，您添加使用MongoDB数据源时，也请避免使用root作为访问账号。

MongoDB Writer通过数据集成框架获取Reader生成的协议数据，然后将支持的类型通过逐一判断转换成MongoDB支持的类型。数据集成本身不支持数组类型，但MongoDB支持数组类型，并且数组类型的索引很强大。

为了使用MongoDB的数组类型，您可以通过参数的特殊配置，将字符串可以转换成MongoDB中的数组，类型转换之后，便可并行写入MongoDB。

## 类型转换列表 {#section_m2v_hxm_q2b .section}

MongoDB Writer支持大部分MongoDB类型，但也存在部分没有支持的情况，请注意检查您的类型。

MongoDB Writer针对MongoDB类型的转换列表，如下所示。

|类型分类|MongoDB数据类型|
|:---|:----------|
|整数类|Int和Long|
|浮点类|Double|
|字符串类|String和Array|
|日期时间类|Date|
|布尔型|Bool|
|二进制类|Bytes|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|collectionName|MonogoDB的集合名。|是|无|
|column|MongoDB的文档列名，配置为数组形式表示MongoDB的多个列。-   name：Column的名字。
-   type：Column的类型。
-   splitter：特殊分隔符，当且仅当要处理的字符串要用分隔符分隔为字符数组Array时，才使用此参数。通过此参数指定的分隔符，将字符串分隔存储到MongoDB的数组中。

|是|无|
|writeMode|指定了传输数据时是否覆盖的信息。-   isReplace：当设置为true时，表示针对相同的replaceKey做覆盖操作。当设置为false时，表示不覆盖。
-   replaceKey：replaceKey指定了每行记录的业务主键，用来做覆盖时使用（不支持replaceKey为多个键，一般是指Monogo中的主键）。

|否|无|
|preSql|支持清空集合的preSql配置："preSql":\{"type":"remove"\}|否|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂不支持向导开发模式。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置写入MongoDB的数据同步作业，详情请参见上述参数说明。

```language-json
{
    "type": "job",
    "version": "2.0",//版本号
    "steps": [//下面是关于Reader的模板，可以找相应的读插件文档
        {
            "stepType": "stream",
            "parameter": {},
            "name": "Reader",
            "category": "reader"
        },
        {
            "stepType": "mongodb",//插件名
            "parameter": {
                "datasource": "",//数据源名
                "column": [
                    {
                        "name": "name",//列名
                        "type": "string"//数据类型
                    },
                    {
                        "name": "age",
                        "type": "int"
                    },
                    {
                        "name": "id",
                        "type": "long"
                    },
                    {
                        "name": "wealth",
                        "type": "double"
                    },
                    {
                        "name": "hobby",
                        "type": "array",
                        "splitter": " "
                    },
                    {
                        "name": "valid",
                        "type": "boolean"
                    },
                    {
                        "name": "date_of_join",
                        "format": "yyyy-MM-dd HH:mm:ss",
                        "type": "date"
                    }
                ],
                "writeMode": {//写入模式
                    "isReplace": "true",
                    "replaceKey": "id"
                },
                "collectionName": "datax_test"//连接名称
            },
            "name": "Writer",
            "category": "writer"
        }
    ],
    "setting": {
        "errorLimit": {//错误记录数
            "record": "0"
        },
        "speed": {
            "jvmOption": "-Xms1024m -Xmx1024m",
            "throttle": true,//false代表不限流，下面的限流的速度不生效，true代表限流
            "concurrent": 1,//作业并发数
            "mbps": "1"//限流的速度
        }
    },
    "order": {
        "hops": [//从reader同步writer
            {
                "from": "Reader",
                "to": "Writer"
            }
        ]
    }
}
```

