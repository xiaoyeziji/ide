# 配置Table Store\(OTS\) Writer {#concept_sdt_fb4_q2b .concept}

本文为您介绍Table Store（OTS） Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

表格存储Table Store是构建在阿里云飞天分布式系统之上的NoSQL数据库服务，提供海量结构化数据的存储和实时访问。Table Store以实例和表的形式组织数据，通过数据分片和负载均衡技术，实现规模上的无缝扩展。

简而言之，Table Store Writer通过Table Store官方Java SDK连接到Table Store服务端，并通过 SDK 写入 Table Store服务端 。Table Store Writer本身对于写入过程做了很多优化，包括写入超时重试、异常写入重试、批量提交等功能。

Table Store Writer插件实现了向Table Store写入数据，目前支持两种写入方式。

-   PutRow：对应于Table Store API PutRow，插入数据到指定的行，如果该行不存在，则新增一行。如果该行存在，则覆盖原有行。
-   UpdateRow：对应于Table Store API UpdateRow，更新指定行的数据，如果该行不存在，则新增一行。如果该行存在，则根据请求的内容在这一行中新增、修改或者删除指定列的值。

目前Table Store Writer支持所有Table Store类型，其针对Table Store类型的转换，如下表所示。

|类型分类|Table Store数据类型|
|:---|:--------------|
|整数类|Integer|
|浮点类|Double|
|字符串类|String|
|布尔类|Boolean|
|二进制类|Binary|

**说明：** 您需要将Integer类型在脚本模式中配置为Int类型，我们内部会将其转换成Integer类型。如果您直接配置为Integer类型，日志将会报错导致任务无法顺利完成。

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|endPoint|Table Store Server的EndPoint（服务地址）。|是|无|
|accessId|Table Store的accessId|是|无|
|accessKey|Table Store的accessKey|是|无|
|instanceName|Table Store的实例名称。实例是您使用和管理Table Store服务的实体，在开通Table Store服务后，需要通过管理控制台来创建实例，然后在实例内进行表的创建和管理。实例是Table Store资源管理的基础单元，Table Store对应用程序的访问控制和资源计量都在实例级别完成。

|是|无|
|table|所选取的需要抽取的表名称，这里有且只能填写一张表。在Table Store中不存在多表同步的需求。|是|无|
|primaryKey|Table Store的主键信息，使用JSON的数组描述字段信息。Table Store本身是NoSQL系统，在Table Store Writer导入数据过程中，必须指定相应的字段名称。**说明：** Table Store的PrimaryKey仅支持STRING和INT两种类型，因此Table Store Writer本身也限定填写上述两种类型。

数据同步系统本身支持类型转换的，因此对于源头数据非String/Int，Table Store Writer 会进行数据类型转换。配置示例如下：

```
"primaryKey" : [
    {"name":"pk1", "type":"string"},
    {"name":"pk2", "type":"int"}
                    ],
```

|是|无|
|column|所配置的表中需要同步的列名集合，使用JSON的数组描述字段信息。使用格式为：

```
{"name":"col2", "type":"INT"},
```

其中的name指定写入的Table Store列名，type指定写入的类型。Table Store类型支持STRING，INT，DOUBLE，BOOL和BINARY类型。

|是|无|
|writeMode| 写入过程不支持常量、函数或者自定义表达式。写入模式，目前支持以下三种类型。

 -   单行操作
    -   GetRow：读取单行数据。
    -   PutRow：对应于Table Store API PutRow，插入数据到指定的行，如果该行不存在，则新增一行。如果该行存在，则覆盖原有行。
    -   UpdateRow：对应于Table Store API UpdateRow，更新指定行的数据，如果该行不存在，则新增一行。如果该行存在，则根据请求的内容在这一行中新增、修改或者删除指定列的值。
    -   DeleteRow：删除一行。
-   批量操作

BatchGetRow：批量读取多行数据。

-   范围读取

GetRange：读取表中一个范围内的数据。


 |是|无|

## 向导开发介绍 {#section_pjc_bh5_q2b .section}

暂不支持向导模式开发。

## 脚本开发介绍 {#section_bp2_wsh_p2b .section}

配置一个写入Table Store作业。

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
            "stepType":"ots",//插件名
            "parameter":{
                "datasource":"",//数据源
                "column":[//字段
                    {
                        "name":"columnName1",//字段名
                        "type":"INT"//数据类型
                    },
                    {
                        "name":"columnName2",
                        "type":"STRING"
                    },
                    {
                        "name":"columnName3",
                        "type":"DOUBLE"
                    },
                    {
                        "name":"columnName4",
                        "type":"BOOLEAN"
                    },
                    {
                        "name":"columnName5",
                        "type":"BINARY"
                    }
                ],
                "writeMode":"",//写入模式
                "table":"",//表名
                "primaryKey":[//Table Store 的主键信息
                    {
                        "name":"pk1",
                        "type":"STRING"
                    },
                    {
                        "name":"pk2",
                        "type":"INT"
                    }
                ]
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

