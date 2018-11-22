# 配置OTS Reader {#concept_jkn_jhq_p2b .concept}

本文为您介绍OTS Reader支持的数据类型、读取方式、字段映射和数据源等参数及配置举例。

OTS Reader插件实现了从OTS读取数据，通过您指定抽取数据范围可方便地实现数据增量抽取的需求。目前支持以下三种抽取方式。

-   全表抽取
-   范围抽取
-   指定分片抽取

OTS是构建在阿里云飞天分布式系统之上的NoSQL数据库服务，提供海量结构化数据的存储和实时访问。OTS以实例和表的形式组织数据，通过数据分片和负载均衡技术，实现规模上的无缝扩展。

简而言之，OTS Reader通过OTS官方Java SDK连接到OTS服务端，获取并按照数据同步官方协议标准转为数据同步字段信息传递给下游Writer端。

OTS Reader会根据OTS的表范围，按照数据同步并发的数目N，将范围等分为N份Task。每个Task都会有一个OTS Reader线程来执行。

目前OTS Reader支持所有OTS类型，OTS Reader针对OTS的类型转换表，如下所示。

|类型分类|MySQL数据类型|
|:---|:--------|
|整数类|Integer|
|浮点类|Double|
|字符串类|String|
|布尔型|Boolean|
|二进制类|Binary|

**说明：** OTS本身不支持日期型类型。应用层一般使用Long报错时间的Unix TimeStamp。

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|endpoint|OTS Server的EndPoint（服务地址），详情请参见[访问控制](https://www.alibabacloud.com/help/zh/faq-detail/27296.htm)。|是|无|
|accessId|OTS的accessId。|是|无|
|accessKey|OTS的accessKey。|是|无|
|instanceName|OTS的实例名称，实例是您使用和管理OTS服务的实体。您在开通OTS服务后，需要通过管理控制台来创建实例，然后在实例内进行表的创建和管理。

实例是OTS资源管理的基础单元，OTS对应用程序的访问控制和资源计量都在实例级别完成。

|是|无|
|table|所选取的需要抽取的表名称，这里有且只能填写一张表。在OTS不存在多表同步的需求。|是|无|
|column|所配置的表中需要同步的列名集合，使用JSON的数组描述字段信息。由于OTS本身是NoSQL系统，在OTS Reader抽取数据过程中，必须指定相应的字段名称。-   支持普通的列读取，例如\{"name":"col1"\}
-   支持部分列读取，如果您不配置该列，则OTS Reader不予读取。
-   支持常量列读取，例如\{"type":"STRING", "value":"DataX"\}。使用type描述常量类型，目前支持String、Int、Double、Bool、Binary（使用Base64编码填写）、INF\_MIN（OTS的系统限定最小值，如果使用该值，您不能填写value属性，否则报错）、INF\_MAX（OTS的系统限定最大值，如果使用该值，您不能填写value属性，否则报错）。
-   不支持函数或者自定义表达式，由于OTS本身不提供类似SQL的函数或者表达式功能，OTS Reader也不能提供函数或表达式列功能。

|是|无|
|begin/end|该配置项必须配对使用，用于支持OTS表范围抽取。begin/end中描述的是OTS PrimaryKey的区间分布状态，而且必须保证区间覆盖到所有的 PrimaryKey，需要指定该表下所有的PrimaryKey范围，对于无限大小的区间，可以使用\{"type":"INF\_MIN"\}，\{"type":"INF\_MAX"\}指代。例如对一张主键为\[DeviceID, SellerID\]的OTS进行抽取任务，begin/end的配置如下所示。```
"range": {
      "begin": [
        {"type":"INF_MIN"},  //指定 deviceID 最小值
        {"type":"INT", "value":"0"}  //指定 SellerID 最小值
      ], 
      "end": [
        {"type":"INF_MAX"}, //指定 deviceID 抽取最大值
        {"type":"INT", "value":"9999"} //指定 SellerID 抽取最大值
      ]
    }
```

如果要对上述表抽取全表，可以使用如下配置。

```
"range": {
      "begin": [
        {"type":"INF_MIN"},  //指定 deviceID 最小值
        {"type":"INF_MIN"} //指定 SellerID 最小值
      ], 
      "end": [
        {"type":"INF_MAX"}, //指定 deviceID 抽取最大值
          {"type":"INF_MAX"} //指定 SellerID 抽取最大值
      ]
    }
```

|是|空|
|split|该配置项属于高级配置项，是您自己定义切分配置信息，普通情况下不建议使用。适用场景：通常在OTS数据存储发生热点，使用OTS Reader自动切分的策略不能生效的情况下，使用您自定义的切分规则。

split指定在Begin、End区间内的切分点，且只能是partitionKey的切分点信息，即在split仅配置partitionKey，而不需要指定全部的PrimaryKey。

如果对一张主键为\[DeviceID, SellerID\]的OTS进行抽取任务，配置如下：

```
"range": {
      "begin": {
        {"type":"INF_MIN"},  //指定 DeviceID 最小值
        {"type":"INF_MIN"}  //指定 SellerID 最小值
      }, 
      "end": {
        {"type":"INF_MAX"}, //指定 DeviceID 抽取最大值
        {"type":"INF_MAX"} //指定 SellerID 抽取最大值
      }，
       // 用户指定的切分点，如果指定了切分点，Job 将按照 begin、end 和 split 进行 Task 的切分，
            // 切分的列只能是 Partition Key（ParimaryKey 的第一列）
            // 支持 INF_MIN, INF_MAX, STRING, INT
            "split":[
                                {"type":"STRING", "value":"1"},
                                {"type":"STRING", "value":"2"},
                                {"type":"STRING", "value":"3"},
                                {"type":"STRING", "value":"4"},
                                {"type":"STRING", "value":"5"}
                    ]
    }
```

|否|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂时没有向导开发模式。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从OTS同步抽取数据到本地的作业。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"ots",//插件名
            "parameter":{
                "datasource":"",//数据源
                "column":[//字段
                    {
                        "name":"column1"//字段名
                    },
                    {
                        "name":"column2"
                    },
                    {
                        "name":"column3"
                    },
                    {
                        "name":"column4"
                    },
                    {
                        "name":"column5"
                    }
                ],
                "range":{
                    "split":[
                        {
                            "type":"INF_MIN"
                        },
                        {
                            "type":"STRING",
                            "value":"splitPoint1"
                        },
                        {
                            "type":"STRING",
                            "value":"splitPoint2"
                        },
                        {
                            "type":"STRING",
                            "value":"splitPoint3"
                        },
                        {
                            "type":"INF_MAX"
                        }
                    ],
                    "end":[
                        {
                            "type":"INF_MAX"
                        },
                        {
                            "type":"INF_MAX"
                        },
                        {
                            "type":"STRING",
                            "value":"end1"
                        },
                        {
                            "type":"INT",
                            "value":"100"
                        }
                    ],
                    "begin":[
                        {
                            "type":"INF_MIN"
                        },
                        {
                            "type":"INF_MIN"
                        },
                        {
                            "type":"STRING",
                            "value":"begin1"
                        },
                        {
                            "type":"INT",
                            "value":"0"
                        }
                    ]
                },
                "table":""//表名
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

