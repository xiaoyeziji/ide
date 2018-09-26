# 配置HBase Reader {#concept_i2s_xgj_p2b .concept}

HBase Reader插件实现了从HBase中读取数据。在底层实现上，HBase Reader通过HBase的Java客户端连接远程HBase服务，并通过Scan方式读取您指定的rowkey范围内的数据，将读取的数据使用数据集成自定义的数据类型拼装为抽象的数据集，并传递给下游Writer处理。

## 支持的功能 { .section}

-   **支持HBase0.94.x和HBase1.1.x版本**
    -   如果您的HBase版本为HBase0.94.x，Reader端的插件请选择HBase094x。

        ```
        "reader": {
                "plugin": "hbase094x"
            }
        ```

    -   如果您的HBase版本为HBase1.1.x，Reader端的插件请选择HBase11x。

        ```
        "reader": {
                "plugin": "hbase11x"
            }
        ```

-   **支持normal和multiVersionFixedColumn模式**
    -   normal模式：把HBase中的表，当成普通二维表（横表）进行读取，读取最新版本数据。

        ```
        hbase(main):017:0> scan 'users'
        ROW                                   COLUMN+CELL
        lisi                                 column=address:city, timestamp=1457101972764, value=beijing
        lisi                                 column=address:contry, timestamp=1457102773908, value=china
        lisi                                 column=address:province, timestamp=1457101972736, value=beijing
        lisi                                 column=info:age, timestamp=1457101972548, value=27
        lisi                                 column=info:birthday, timestamp=1457101972604, value=1987-06-17
        lisi                                 column=info:company, timestamp=1457101972653, value=baidu
        xiaoming                             column=address:city, timestamp=1457082196082, value=hangzhou
        xiaoming                             column=address:contry, timestamp=1457082195729, value=china
        xiaoming                             column=address:province, timestamp=1457082195773, value=zhejiang
        xiaoming                             column=info:age, timestamp=1457082218735, value=29
        xiaoming                             column=info:birthday, timestamp=1457082186830, value=1987-06-17
        xiaoming                             column=info:company, timestamp=1457082189826, value=alibaba
        2 row(s) in 0.0580 seconds }
        ```

        读取后的数据，如下所示：

        |rowKey|addres:city|address:contry|address:province|info:age|info:birthday|info:company|
        |:-----|:----------|:-------------|:---------------|:-------|:------------|:-----------|
        |lisi|beijing|china|beijing|27|1987-06-17|baidu|
        |xiaoming|hangzhou|china|zhejiang|29|1987-06-17|alibaba|

    -   multiVersionFixedColumn模式：把HBase中的表，当成竖表进行读取。读出的每条记录一定是四列形式，依次为rowKey、family:qualifier、timestamp和value。读取时需要明确指定要读取的列，把每一个cell中的值，作为一条记录（record），若有多个版本就有多条记录。

        ```
        hbase(main):018:0> scan 'users',{VERSIONS=>5}
        ROW                                   COLUMN+CELL
        lisi                                 column=address:city, timestamp=1457101972764, value=beijing
        lisi                                 column=address:contry, timestamp=1457102773908, value=china
        lisi                                 column=address:province, timestamp=1457101972736, value=beijing
        lisi                                 column=info:age, timestamp=1457101972548, value=27
        lisi                                 column=info:birthday, timestamp=1457101972604, value=1987-06-17
        lisi                                 column=info:company, timestamp=1457101972653, value=baidu
        xiaoming                             column=address:city, timestamp=1457082196082, value=hangzhou
        xiaoming                             column=address:contry, timestamp=1457082195729, value=china
        xiaoming                             column=address:province, timestamp=1457082195773, value=zhejiang
        xiaoming                             column=info:age, timestamp=1457082218735, value=29
        xiaoming                             column=info:age, timestamp=1457082178630, value=24
        xiaoming                             column=info:birthday, timestamp=1457082186830, value=1987-06-17
        xiaoming                             column=info:company, timestamp=1457082189826, value=alibaba
        2 row(s) in 0.0260 seconds }
        ```

        读取后的数据（4列）

        |rowKey|column:qualifier|timestamp|value|
        |:-----|:---------------|:--------|:----|
        |lisi|address:city|1457101972764|beijing|
        |lisi|address:contry|1457102773908|china|
        |lisi|address:province|1457101972736|beijing|
        |lisi|info:age|1457101972548|27|
        |lisi|info:birthday|1457101972604|1987-06-17|
        |lisi|info:company|1457101972653|beijing|
        |xiaoming|address:city|1457082196082|hangzhou|
        |xiaoming|address:contry|1457082195729|china|
        |xiaoming|address:province|1457082195773|zhejiang|
        |xiaoming|info:age|1457082218735|29|
        |xiaoming|info:age|1457082178630|24|
        |xiaoming|info:birthday|1457082186830|1987-06-17|
        |xiaoming|info:company|1457082189826|alibaba|


支持读取HBase数据类型，HBase Reader针对HBase类型的转换列表，如下所示。

|数据集成内部类型|HBase数据类型|
|:-------|:--------|
|Long|Int、Short和Long|
|Double|Float和Double|
|String|String和Binarystring|
|Date|Date|
|Boolean|Boolean|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|haveKerberos|haveKerberos值为true时，表示HBase集群需要kerberos认证。**说明：** 

-   如果该值配置为true，必须要配置下面五个kerberos认证相关参数。kerberosKeytabFilePath、kerberosPrincipal、hbaseMasterKerberosPrincipal、hbaseRegionserverKerberosPrincipal和hbaseRpcProtection。
-   如果HBase集群没有kerberos认证，则不需要配置以上参数。

|否|false|
|hbaseConfig|连接HBase集群需要的配置信息，JSON格式。必填的配置为hbase.zookeeper.quorum，表示HBase的ZK链接地址。同时可以补充更多HBase client的配置，例如设置scan的cache、batch来优化与服务器的交互。|是|无|
|mode|读取HBase的模式，支持normal模式、multiVersionFixedColumn模式，即normal/multiVersionFixedColumn。|是|无|
|table|读取的HBase表名（大小写敏感） 。|是|无|
|encoding|编码方式，UTF-8或是GBK，用于对二进制存储的HBase byte\[\]转为String时的编码。|否|utf-8|
|column|要读取的HBase字段，normal模式与multiVersionFixedColumn模式下必填。-   **normal模式下**

name指定读取的HBase列，除rowkey外，必须为列族:列名的格式，type指定源数据的类型，format指定日期类型的格式，value指定当前类型为常量，不从HBase读取数据，而是根据value值自动生成对应的列。配置格式如下所示：

    ```
"column": 
[
{
  "name": "rowkey",
  "type": "string"
},
{
  "value": "test",
  "type": "string"
}
]
    ```

normal模式下，对于您指定的Column信息，type必须填写，name/value必须选择其一。

-   **multiVersionFixedColumn 模式**

name指定读取的HBase列，除rowkey外，必须为列族:列名的格式，type指定源数据的类型，format指定日期类型的格式。multiVersionFixedColumn模式下不支持常量列。配置格式如下：

    ```
"column": 
[
{
  "name": "rowkey",
  "type": "string"
},
{
  "name": "info: age",
  "type": "string"
}
]
    ```


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


|否|无|
|scanCacheSize|HBase client每次rpc从服务器端读取的行数。|否|256|
|scanBatchSize|HBase client每次rpc从服务器端读取的列数。|否|100|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

暂不支持向导开发模式开发。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

配置一个从HBase抽取数据到本地的作业（normal模式）。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"hbase",//插件名
            "parameter":{
                "mode":"normal",//读取 HBase 的模式，支持 normal 模式、multiVersionFixedColumn 模式
                "scanCacheSize":"256",//HBase client 每次 rpc 从服务器端读取的行数
                "scanBatchSize":"100",//HBase client 每次 rpc 从服务器端读取的列数。 
                "hbaseVersion":"094x/11x",//hbase版本
                "column":[//字段
                    {
                        "name":"rowkey",//字段名
                        "type":"string"//数据类型
                    },
                    {
                        "name":"columnFamilyName1:columnName1",
                        "type":"string"
                    },
                    {
                        "name":"columnFamilyName2:columnName2",
                        "format":"yyyy-MM-dd",
                        "type":"date"
                    },
                    {
                        "name":"columnFamilyName3:columnName3",
                        "type":"long"
                    }
                ],
                "range":{//指定 HBase Reader 读取的 rowkey 范围。
                    "endRowkey":"",//指定结束 rowkey。
                    "isBinaryRowkey":true,//指定配置的 startRowkey 和 endRowkey 转换为 byte[] 时的方式，默认值为 false
                    "startRowkey":""//指定开始 rowkey。
                },
                "maxVersion":"",//指定在多版本模式下的 HBase Reader 读取的版本数
                "encoding":"UTF-8",//编码格式
                "table":"",//表名
                "hbaseConfig":{//连接HBase集群需要的配置信息，JSON格式。
                    "hbase.zookeeper.quorum":"hostname",
                    "hbase.rootdir":"hdfs://ip:port/database",
                    "hbase.cluster.distributed":"true"
                }
            },
            "name":"Reader",
            "category":"reader"
        },
        {//下面是关于Reader的模板，可以找相应的读插件文档
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

