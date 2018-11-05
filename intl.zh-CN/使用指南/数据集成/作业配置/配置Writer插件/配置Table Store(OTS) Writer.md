# 配置Table Store\(OTS\) Writer {#concept_sdt_fb4_q2b .concept}

本文为您介绍Table Store\(OTS\) Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

表格存储 Table Store 是构建在阿里云飞天分布式系统之上的 NoSQL 数据库服务，提供海量结构化数据的存储和实时访问。Table Store 以实例和表的形式组织数据，通过数据分片和负载均衡技术，实现规模上的无缝扩展。

简而言之，Table Store Writer 通过 Table Store 官方 Java SDK 连接到 Table Store 服务端，并通过 SDK 写入 Table Store 服务端 。Table Store Writer 本身对于写入过程做了很多优化，包括写入超时重试、异常写入重试、批量提交等 Feature。

Table Store Writer 插件实现了向 Table Store 写入数据，目前支持两种写入方式：

-   PutRow：对应于 Table Store API PutRow，插入数据到指定的行，如果该行不存在，则新增一行；若该行存在，则覆盖原有行。
-   UpdateRow：对应于 Table Store API UpdateRow，更新指定行的数据，如果该行不存在，则新增一行；若该行存在，则根据请求的内容在这一行中新增、修改或者删除指定列的值。

目前 Table Store Writer 支持所有 Table Store 类型，其针对 Table Store 类型的转换，如下表所示：

|类型分类|Table Store 数据类型|
|:---|:---------------|
|整数类|Integer|
|浮点类|Double|
|字符串类|String|
|布尔类|Boolean|
|二进制类|Binary|

**说明：** 

您需要将 Integer 类型在脚本模式中配置为 Int 类型，我们内部会将其转换成 Integer 类型。如果您直接配置为 Integer 类型，日志将会报错导致任务无法顺利完成。

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|datasource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|endPoint|Table Store Server 的 EndPoint（服务地址），详情请参见 访问控制。|是|无|
|accessId|Table Store 的 accessId|是|无|
|accessKey|Table Store 的 accessKey|是|无|
|instanceName|Table Store 的实例名称。实例是您使用和管理 Table Store 服务的实体，在开通Table Store 服务之后，需要通过管理控制台来创建实例，然后在实例内进行表的创建和管理。实例是 Table Store 资源管理的基础单元，Table Store 对应用程序的访问控制和资源计量都在实例级别完成。

|是|无|
|table|所选取的需要抽取的表名称，这里有且只能填写一张表。在 Table Store 中不存在多表同步的需求。|是|无|

-   primaryKey
    -   描述: Table Store 的主键信息，使用 JSON 的数组描述字段信息。Table Store 本身是 NoSQL 系统，在 Table Store Writer 导入数据过程中，必须指定相应的字段名称。
    -   必选：是。
    -   默认值：无Table Store 的 PrimaryKey 只能支持 STRING，INT 两种类型，因此 Table Store Writer 本身也限定填写上述两种类型。

数据同步系统本身支持类型转换的，因此对于源头数据非 String/Int，Table Store Writer 会进行数据类型转换。配置实例：

```
"primaryKey" : [
    {"name":"pk1", "type":"string"},
    {"name":"pk2", "type":"int"}
                    ],
```

-   column
    -   描述：所配置的表中需要同步的列名集合，使用 JSON 的数组描述字段信息。
    -   必选：是。
    -   默认值：无。

使用格式为：

```
{"name":"col2", "type":"INT"},
```

其中的 name 指定写入的 Table Store 列名，type 指定写入的类型。Table Store 类型支持 STRING，INT，DOUBLE，BOOL 和 BINARY 类型。

写入过程不支持常量、函数或者自定义表达式。

-   writeMode
    -   描述：写入模式，目前支持以下三种类型：
-   单行操作

```
GetRow：读取单行数据。
PutRow，对应于 Table Store API PutRow，插入数据到指定的行，如果该行不存在，则新增一行；若该行存在，则覆盖原有行。
UpdateRow，对应于 Table Store API UpdateRow，更新指定行的数据，如果该行不存在，则新增一行；若该行存在，则根据请求的内容在这一行中新增、修改或者删除指定列的值。
DeleteRow：删除一行。
```

-   批量操作

```
BatchGetRow：批量读取多行数据。
```

-   范围读取

    ```
    GetRange：读取表中一个范围内的数据。
    ```

    -   必选：是。
    -   默认值：无。

## 向导开发介绍 {#section_pjc_bh5_q2b .section}

暂不支持向导模式开发。

## 脚本开发介绍 {#section_bp2_wsh_p2b .section}

配置一个写入 Table Store 作业：

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

