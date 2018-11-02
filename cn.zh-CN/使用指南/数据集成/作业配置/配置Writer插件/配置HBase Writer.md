# 配置HBase Writer {#concept_nr5_w5l_q2b .concept}

本文为您介绍HBase Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

HBase Writer插件实现了向HBase中写入数据。在底层实现上，HBase Writer通过HBase的Java客户端连接远程HBase服务，并通过put方式写入Hbase。

## 支持的功能 {#section_wv3_x5l_q2b .section}

-   **支持HBase0.94.x和HBase1.1.x版本**
    -   如果您的HBase版本为HBase0.94.x，Writer端的插件请选择HBase094x。

        ```
        "writer": {
                "plugin": "hbase094x"
            }
        ```

    -   如果您的HBase版本为HBase1.1.x，Writer端的插件请选择HBase11x。

        ```
        "writer": {
                "plugin": "hbase11x"
            }
        ```

-   **支持源端多个字段拼接作为rowkey**

    目前HBase Writer支持源端多个字段拼接作为HBase表的rowkey，详情请参见rowkeyColumn配置。

-   **写入HBase的版本支持**

    写入HBase的时间戳（版本）支持。

    -   支持当前时间作为版本。
    -   支持指定源端列作为版本。
    -   支持指定一个时间作为版本。

支持读取HBase数据类型，HBase Writer针对HBase类型的转换列表，如下所示。

|数据集成内部类型|HBase数据类型|
|:-------|:--------|
|Long|Int、Short和Long|
|Double|Float和Double|
|String|String|
|Boolean|Boolean|

**说明：** 除上述罗列字段类型外，其他类型均不支持。

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|haveKerberos|haveKerberos值为true时，表示HBase集群需要kerberos认证。**说明：** 

-   如果该值配置为true，必须要配置下面五个kerberos认证相关参数。kerberosKeytabFilePath、kerberosPrincipal、hbaseMasterKerberosPrincipal、hbaseRegionserverKerberosPrincipal和hbaseRpcProtection。
-   如果HBase集群没有kerberos认证，则不需要配置以上参数。

|否|false|
|hbaseConfig|连接HBase集群需要的配置信息，JSON格式。必填的配置为hbase.zookeeper.quorum，表示HBase的ZK链接地址。同时可以补充更多HBase client的配置，例如设置scan的cache、batch来优化与服务器的交互。|是|无|
|mode|写入HBase的模式，目前仅支持normal模式，后续考虑动态列模式。|是|无|
|table|要写入的HBase表名（大小写敏感） 。|是|无|
|encoding|编码方式，UTF-8或GBK，用于String转HBase byte\[\]时的编码。|否|utf-8|
|column|要写入的HBase字段。-   index：指定该列对应Reader端column的索引，从0开始。
-   name：指定HBase表中的列，格式必须为列族:列名。
-   type：指定写入的数据类型，用于转换HBase byte\[\]。

|是|无|
|maxVersion|指定在多版本模式下的HBase Reader读取的版本数，取值只能为－1或大于1的数字，－1表示读取所有版本。|multiVersionFixedColumn模式下必填项|无|
|range|指定HBase Reader读取的rowkey范围。-   startRowkey：指定开始rowkey。
-   endRowkey：指定结束rowkey。
-   isBinaryRowkey：指定配置的startRowkey和endRowkey转换为 byte\[\]时的方式，默认值为false。如果为true，则调用Bytes.toBytesBinary\(rowkey\)方法进行转换。若为false，则调用 Bytes.toBytes\(rowkey\)。配置格式如下所示：

    ```
"range": {
"startRowkey": "aaa",
"endRowkey": "ccc",
"isBinaryRowkey":false
}
    ```

配置格式如下所示。

    ```
"column": [
        {
          "index":1,
          "name": "cf1:q1",
          "type": "string"
        },
        {
          "index":2,
          "name": "cf1:q2",
          "type": "string"
        }
     ］
    ```


|否|无|
|rowkeyColumn|要写入的HBase的rowkey列。-   index：指定该列对应Reader端column的索引，从0开始。如果是常量，index为-1。
-   type：指定写入的数据类型，用于转换HBase byte\[\]。
-   value：配置常量，常作为多个字段的拼接符。HBase Writer会将rowkeyColumn中所有列按照配置顺序进行拼接作为写入HBase的rowkey，不能全为常量。

配置格式如下所示。

```
"rowkeyColumn": [
          {
            "index":0,
            "type":"string"
          },
          {
            "index":-1,
            "type":"string",
            "value":"_"
          }
      ]
```

|是|无|
|versionColumn|指定写入HBase的时间戳。支持当前时间、指定时间列或指定时间（三者选一），如果不配置则表示用当前时间。-   index：指定对应Reader端column的索引，从0开始，需保证能转换为long。
-   type：如果是Date类型，会尝试用yyyy-MM-dd HH:mm:ss和yyyy-MM-dd HH:mm:ss SSS去解析。如果是指定时间，则index为－1。
-   value：指定时间的值，long值。

配置格式如下所示。

-   ```
"versionColumn":{
"index":1
}
```

-   ```
"versionColumn":{
"index":－1,
"value":123456789
}
```


 |否|无|
|nullMode|读取的数据为null值时，您可通过以下两种方式解决。-   skip：表示不向HBase写这列。
-   empty：写入HConstants.EMPTY\_BYTE\_ARRAY，即new byte \[0\]。

|否|skip|
|walFlag|HBae Client向集群中的RegionServer提交数据时（Put/Delete操作），首先会先写WAL（Write Ahead Log）日志（即HLog，一个RegionServer上的所有Region共享一个HLog），只有当WAL日志写成功后，再接着写MemStore，然后客户端被通知提交数据成功。如果写WAL日志失败，客户端则被通知提交失败。关闭（false）放弃写WAL日志，从而提高数据写入的性能。|否|false|
|writeBufferSize|设置HBae Client的写Buffer大小，单位字节，配合autoflush使用。autoflush：

-   开启（true）：表示HBase Client在写的时候有一条put就执行一次更新。
-   关闭（false）：表示HBase Client在写的时候只有当put填满客户端写缓存时，才实际向HBase服务端发起写请求。

|否|8M|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂不支持向导开发模式开发。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从本地写入hbase1.1.x的作业。

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
            "stepType":"hbase",//插件名
            "parameter":{
                "mode":"normal",//写入 HBase 的模式
                "walFlag":"false",//关闭（false）放弃写 WAL 日志
                "hbaseVersion":"094x",//Hbase版本
                "rowkeyColumn":[//要写入的 HBase 的 rowkey 列。
                    {
                        "index":"0",//序列号
                        "type":"string"//数据类型
                    },
                    {
                        "index":"-1",
                        "type":"string",
                        "value":"_"
                    }
                ],
                "nullMode":"skip",//读取的 null 值时，如何处理
                "column":[//要写入的 HBase 字段。
                    {
                        "name":"columnFamilyName1:columnName1",//字段名
                        "index":"0",//索引号
                        "type":"string"//数据类型
                    },
                    {
                        "name":"columnFamilyName2:columnName2",
                        "index":"1",
                        "type":"string"
                    },
                    {
                        "name":"columnFamilyName3:columnName3",
                        "index":"2",
                        "type":"string"
                    }
                ],
                "writeMode":"api",//写入模是
                "encoding":"utf-8",//编码格式
                "table":"",//表名
                "hbaseConfig":{//连接HBase集群需要的配置信息，JSON格式
                    "hbase.zookeeper.quorum":"hostname",
                    "hbase.rootdir":"hdfs: //ip:port/database",
                    "hbase.cluster.distributed":"true"
                }
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

