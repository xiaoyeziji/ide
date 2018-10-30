# 配置Stream Reader {#concept_sjr_nlr_p2b .concept}

本文为您介绍Stream Reader支持的数据类型、读取方式、字段映射和数据源等参数及配置举例。

Stream Reader插件实现了从内存中自动产生数据的功能，主要用于数据同步的性能测试和基本的功能测试。

Stream Reader支持的数据类型，如下所示。

|数据类型|类型描述|
|:---|:---|
|string|字符型|
|long|长整型|
|date|日期类型|
|bool|布尔型|
|bytes|bytes型|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|column|产生的源数据的列数据和类型，可以配置多列。可以配置产生随机字符串，并制定范围，示例如下。```
"column" : [
      {
          "random": "8,15"
      },
      {
          "random": "10,10"
      }
]
```

配置项说明如下：

-   "random": "8, 15"：表示随机产生8~15位长度的字符串。
-   "random": "10, 10"：表示随机产生10位长度的字符串。

|是|无|
|sliceRecordCount|表示循环产生column的份数。|是|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂时不支持向导开发模式。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从内存中读数据的同步作业。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"stream",//插件名
            "parameter":{
                "column":[//字段
                    {
                        "type":"string",//数据类型
                        "value":"field"//值
                    },
                    {
                        "type":"long",
                        "value":100
                    },
                    {
                        "dateFormat":"yyyy-MM-dd HH:mm:ss",//时间格式
                        "type":"date",
                        "value":"2014-12-12 12:12:12"
                    },
                    {
                        "type":"bool",
                        "value":true
                    },
                    {
                        "type":"bytes",
                        "value":"byte string"
                    }
                ],
                "sliceRecordCount":"100000"//表示循环产生 column 的份数
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

