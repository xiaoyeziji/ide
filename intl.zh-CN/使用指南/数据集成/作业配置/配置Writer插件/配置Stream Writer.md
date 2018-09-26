# 配置Stream Writer {#concept_okj_c24_q2b .concept}

Stream Writer 插件实现了从 Reader 端读取数据并向屏幕上打印数据或者直接丢弃数据，主要用于数据同步的性能测试和基本的功能测试。

## 参数说明 {#section_a3m_424_q2b .section}

-   print
    -   描述：是否向屏幕打印输出。
    -   必选：否。
    -   默认值：true。

## 向导开发介绍 {#section_bhw_rj5_q2b .section}

暂不支持向导模式开发。

## 脚本开发介绍 {#section_pcz_fh4_q2b .section}

配置一个从 Reader 端读取数据并向屏幕打印的作业：

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        { //下面是关于Reader的模板，可以找相应的读插件文档
            "stepType":"stream",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType":"stream",//插件名
            "parameter":{
                "print":false,//是否向屏幕打印输出
                "fieldDelimiter":","//列分隔符
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

