# 配置Tair Writer {#concept_ynf_pzm_q2b .concept}

本文为您介绍Tair Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

## DataX3.0 tairwriter说明 {#section_i4h_2d4_q2b .section}

如果您使用Datax2 任务迁移同步中心的tair writer，请先阅读完tair 配置介绍。Datax2和同步中心tair 配置对应关系请移步到文档最后。

Tair 是一个高性能，分布式，可扩展，高可靠的nosql存储系统。现支持Java，C/C++版本和restful的客户端。

Tair后台有mdb/ldb两种存储引擎，mdb是类似memcache的内存kv，ldb是基于leveldb的持久化存储引擎，请根据你们的业务场景选择。

TairWriter的导入是使用通用的接口写入mdb/ldb的方式实现的。如果有从odps-\>tair的大数据量导入需求，请使用fastdump。

## 实现原理 {#section_jn2_gqh_p2b .section}

TairWriter实现的比较简单，主要就是使用tair客户端的put/prefixPuts/setCount/prefixSetCounts四个原生Api接口，将reader读到的数据以record为单位一条一条地写入tair。

put、counter输入都是一个key:value对\(即key-\>value\), 其中counter的value必须是非负整数。prefixPuts和prefixCounters的输入则有3项pkey,skey和value\(即pkey + skey -\> value\)， 同上prefixCounter的value也必须是非负整数。

## 类型转换 {#section_axy_qd4_q2b .section}

tair不是强类型的NoSQL数据库，虽然Tair的java客户端本身支持任何Serilizable数据类型原样写入，但是这里是简单先统一全部转化为String类型后再导入tair的（包括c++和java）。

## 参数说明 {#section_amt_td4_q2b .section}

注意为了避免参数使用错误，每个配置参数都不设置默认值，对于 必选：是 的参数是肯定都需要配置的，有些配置项是特定的写入模式（共6种）下才需要的配制的，如 必选：put模式下是必填 表示只有使用put模式才需要配置，多配置或者少配置均会报错。

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|language|使用Tair客户端交互的语言，目前支持两类语言c++或者java。请根据你后续使用访问Tair客户端来选择填写该字段（tair 的c++和java客户端写入数据是不兼容的），比如用DataX TairWriter插件将据写入目的Tair系统后，后续业务代码如果使用Tair C++客户端访问该表，那么这里需要填写c++，如果后续使用Tair Java客户端访问该表，那么这需要填写java。restful客户端也填java。|是，只能为“c++”或者“java” ，不区分大小写。|无|
|fieldDelimiterc|在TairWriter使用put模式写入表模式，使用fieldDelimiter代表的字符将所有非key字段（即除第一列外）拼接一起。请确保fieldDelimiter所代表的字段在你的数据不会在，尽量不要使用类似逗号等常见字符。|在put模式下必填。|无|
|skeyList|在TairWriter使用multiprefixput或者multiprefixcounter模式写入，指定作为skey写入的字段名称列表，请使用json的列表格式。注意：这里的列数应该比reader取出的列数少1（因为第一列作为pkey了）。|在mutilprefixput/multiprefixcounter模式下必填。|无|
|expire|过期时间，如果expire为0表示永不自动删除 ，过期时间是指数据在tair中的保存时间，单位是秒。|是|无|
|compressionThreshold|压缩阈值，tair中的一个value如果超过阈值大小就会进行压缩，这是牺牲cpu换取网络传输效率和存储空间的选择。默认超过8K压缩，如果你的机器性能较差发现有很多压缩日志导致datax机器load很高，甚至导致请求超时可以调大这个阈值，另外如果使用c++客户端由于不支持解压，所以请关闭压缩功能（也就是把这个值设置成为1000000，这个值是因为tair最大支持近似1M的value）。|当language是java时必填。|无|
|deleteEmptyRecord|在put模式下对null的处理方式。Tair本身是不允许写入的key和value为null（即除作为key的第0列不能为null外，其他列不能全部都是null：后面至少要有一个非null的列），如果出现value全为null的一般会标记为脏数据跳过。但如果把deleteEmptyRecord设置为true，那么所有为null的记录数据都会触发一个Delete操作，从tair里删除对应的这个key。（仅仅对put模式有效）|当写入模式为put时必填。|无|
|timeout|内置的tair客户端每次请求的超时时间，如果网络不好，为了避免太多超时失败才需要适当配置大点。|否，单位是毫秒，默认2000ms。|无|
|frontLeadingKey|给从源读出的所有key或者pkey前加上统一配置的前缀串后，再写入tairc。|无|无|

-   frontLeadingKey

    -   描述：给从源读出的所有key或者pkey前加上统一配置的前缀串后，再写入tairp。
    -   必选：否。
    比如：

    |id|name|age|
    |:-|:---|:--|
    |1|lilei|24|

-   writerType
    -   描述：TairWriter 目前支持上面列出的6种写入模式，需要根据业务使用tair的需要来选择，下面依次具体描述各个使用模式。因为Tair是按照key/value存储的，没有表模式，为了方便统一将每个record的第一个字段作为\(主\)key。
    -   必选：是，只能填put、prefixput、multiprefixput、counter、prefixcounter及multiprefixcounter。

1、put模式将来源表的第一个字段作为Tair表中的key，剩余所有字段顺序拼接为一个字符串,最后用put写入，拼接连接符使用fieldDelimiter配置项指定的字符。 例如:

|id|name|age|
|::|:--:|:-:|
|1|lilei|24|

put写入后是一条数据 key是”1”, value是“lilei,24” \(其中”,”是你配置的分隔符\)。

2、prefixput 是专门将一个3列xN行的表通过prefixput接口一行行写入，每行的3列分别作为pkey，skey和value。 也就是说该模式要求表中有且仅有三列，并且第一列做pkey，第二列做skey，第三列做value 这个选项还适用于业务方将一个很稀疏的表转化成一个三列格式的表存入tair，或者一些需要自定义skey的场景。

比如一张表数据如下：

|id|name|age|
|:-|:---|:--|
|1|20150101|hanmeimei|

pkey是串”1”, skey是串”20150101”, value是串”hanmeimei”。

3、multiprefixput 模式是将一张M列xN行的二维表中的M\*N个值各自单独导入tair，即导入后可以通过行ID（pkey）和列名（skey）就能从tair中查到它的值\(value\)，该模式也是使用prefixPuts接口写入tair，第一个字段作为pkey，第二个字段列名作为skey，第二列的列值作为value。

例如对于一行数据：

|id|name|age|
|:-|:---|:--|
|1|lilei|24|

multiprefixput 写入后是一个pkey下的两项skey的数据：

pkey-\>1, skey-\>name, value-\>lilei

pkey-\>1, skey-\>age, value-\>24

4、counter模式 前面3种模式导入tair的数据都是string，tair还支持另一种数据类型：计数器，该类型的数据导入后可以用incr/decr接口进行自增/减操作，同样这种数据也支持普通的计数器和带prefix的计数器，counter只接受两列的数据，比如：

|page|pv|
|:---|:-|
|www.taobao.com|1000|

写入tair就是 key-\>page\_a\_pv value-\>100（负数和非数字都会作为脏数据跳过, 且必须只有2列）。

5、prefixcounter模式同prefixput类似，专门导入3列的表里数据，分别作为pkey，skey和value\(count计数\) 比如：

|data|page|pv|
|:---|:---|:-|
|2015-03-20 18:00:00|www.taobao.com|1000|

写入tair就是 pkey是串”2015-03-20 18:00:00”, skey是串”www.taobao.com”, value是串”1000”。

6、multiprefixcounter模式同mutilprefixput类似，分别将行ID（每行第一列）作为pkey，每行第2,3..N列的列名分别作为skey，每行第2,3..N列的值分别作为value\(count计数\) 比如：

|taobaoid|age|money|
|:-------|:--|:----|
|111xxx3|24|1000|

写入tair也是两项数据：

pkey-\>1112223, skey-\>age, value-\>24

pkey-\>1112223, skey-\>money, value-\>1000

## 向导模式介绍 {#section_cp2_wsh_p2b .section}

1.同步配置：数据来源和数据去向。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16262/15413912018319_zh-CN.jpg)

配置项说明：

-   数据源：即上述参数说明中的 datasource，一般填写您配置的数据源名称。
-   导入模式：即参数writerType。
-   集群configId：即上述参数说明中的 configId。
-   集群空间号：即上述参数说明中的namespace。
-   Value自动以压缩阈值：即上述参数说明中的 compressionThreshold。
-   过期时间：即上述参数说明中的 expire。

2.字段映射：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16262/15413912018321_zh-CN.png)

来源或目标拥有Lindon,Hbase,Tair等数据源，字段不需要连线，直接编辑即可保存。

-   手动编辑源表字段：请手动编辑字段,一行表示一个字段,首尾空行会被撷取,空行会被忽略。

3.通道控制

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16262/15413912018322_zh-CN.png)

-   jvm参数：数据集成消耗资源（包含 CPU、内存、网络等资源分配）的度量单位。一个jvm描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
-   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度。
-   错误记录数：表示脏数据的最大容忍条数。

## 脚本模式介绍 {#section_hy1_dys_q2b .section}

提供脚本配置样例如下，具体参数填写请参见 参数说明。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"tair",
            "parameter":{},
            "name":"Reader",
            "category":"reader"
        },
        {
            "stepType":"tair",//插件名
            "parameter":{
                "skeyList":[
                    "seller_nick", 
                    "lq3d_rate_n14d"
                ],
                "isFullImport":"0",
                "language": "java",//客户端语言 
                "fieldDelimiter": ",",//列分隔符 
                "userName": "", 
                "frontLeadingKey": "",
                "timeout": "2000",//超时时间 
                "compressionThreshold": "128",//Value自动以压缩阈值 
                "configId": "counter",//集群configId 
                "expire": "19000",//过期时间 
                "namespace": "",//集群空间号 
                "writerType": "put",//导入模式 
                "deleteEmptyRecord": "false" 
            },
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "jvmOption":"-Xms1024m -Xmx1026m",//JVM内存
        "errorLimit":{
            "record": "2315467"//错误记录数
        },
        "speed":{
            "concurrent":29,
            "throttle":false//false代表不限流，下面的限流的速度不生效，true代表限流
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

## 插件特点 {#section_im5_vjt_q2b .section}

-   写入幂等性：Tair本身的写入是幂等的，多次写入会覆盖前面写的。
-   任务重跑和failover：由于Tair写入本身是幂等性的，因此可以支持单任务FailOver。即一旦写入Fail，DataX会重新启动相关子任务进行重试（最多重试10次），但是重试只支持put和counter模式的写入。
-   数据限制：为tair中有key和value的长度限制，key需要在\(0, 1024Byte\] 范围内， value需要在\(1-1000000Byte\]之间。超过长度的数据会被记为脏数据，所以请应用放最好事先做好过滤的截断。

