# 配置MongoDB Reader {#concept_lsz_f2p_p2b .concept}

MongoDB Reader插件利用MongoDB的Java客户端MongoClient进行MongoDB的读操作。最新版本的Mongo已经将DB锁的粒度从DB级别降低到document级别，配合MongoDB强大的索引功能，即可达到高性能读取MongoDB的需求。

**说明：** 

-   如果您使用的是云数据库MongoDB版，MongoDB默认会有root账号。出于安全策略的考虑，数据集成仅支持使用MongoDB数据库对应账号进行连接，您添加使用MongoDB数据源时，也请避免使用root作为访问账号。
-   query不支持JS语法。

MongoDB Reader通过数据集成框架从MongoDB并行地读取数据，通过主控的Job程序按照指定规则对MongoDB中的数据进行分片并行读取，然后将MongoDB支持的类型通过逐一判断转换成数据集成支持的类型。

## 类型转换列表 {#section_m2v_hxm_q2b .section}

MongoDB Writer支持大部分MongoDB类型，但也存在部分没有支持的情况，请注意检查您的类型。

MongoDB Writer针对MongoDB类型的转换列表，如下所示。

|类型分类|MongoDB数据类型|
|:---|:----------|
|整数类|Int和Long|
|浮点类|Double|
|字符串类|String|
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
-   splitter：因为MongoDB支持数组类型，但CDP框架本身不支持数组类型，所以MongoDB读出来的数组类型要通过这个分隔符合并成字符串。

|是|无|
|query|您可以通过该配置型来限制返回MongoDB数据范围，例如您可以配置`"query":"{'operationTime':{'$gte':ISODate('${last_day}T00:00:00.424+0800')}}"`，限制返回operationTime大于等于$\{last\_day\}零点的数据，这里$\{last\_day\}为 DataWorks调度参数，格式为$\[yyyy-mm-dd\]。您可以根据需要具体使用其他MongoDB支持的条件操作符号（$gt、$lt、$gte和$lte等），逻辑操作符（and和or等），函数（max、min、sum、avg和ISODate等），详情请参见MongoDB查询语法。|否|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂时没有向导开发模式。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从MongoDB抽取数据到本地的作业，详情请参见上述参数说明。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"hdfs",//插件名
            "parameter":{
                "path":"",//路径
                "datasource":"",//数据源
                "query":"",
                "column":[
                    {
                        "index":0,//序列号
                        "type":"string"//字段类型
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
                        "format":"yyyy-MM-dd HH:mm:ss",//日期格式
                        "index":4,
                        "type":"date"
                    }
                ],
                "fieldDelimiter":",",//列分隔符
                "encoding":"UTF-8",//编码格式
                "fileType":"text"//文件类型
            },
            "name":"Reader",
            "category":"reader"
        },
        { //下面是关于Writer的模板，可以找相应的写插件文档
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

