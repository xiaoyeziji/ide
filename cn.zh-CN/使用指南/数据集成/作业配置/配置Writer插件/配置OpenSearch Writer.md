# 配置OpenSearch Writer {#concept_kwq_vnt_q2b .concept}

本文为您介绍OpenSearch Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

OpenSearch Writer插件用于向OpenSearch中插入或者更新数据，主要提供给数据开发者，将处理好的数据导入到OpenSearch，以搜索的方式输出。数据传输的速率取决于OpenSearch表对应的帐号的qps。

实现原理

在底层实现上，OpenSearch Writer通过OpenSearch对外提供的开放搜索接口。

-   V3版本使用二方包，依赖pom为：com.aliyun.opensearch aliyun-sdk-opensearch 2.1.3。

**说明：** 

-   如果您需要使用OpenSearchWriter插件，请务必使用JDK 1.6-32及以上版本，使用java -version查看Java版本号。
-   目前默认资源组还不支持连接VPC环境，如果是VPC环境可能会存在网络问题。

## 插件特点 {#section_jn2_gqh_p2b .section}

关于列顺序的问题

OpenSearch的列是无序的，因此OpenSearch Writer写入的时候，要严格按照指定的列的顺序写入，不能多也不能少。若指定的列比OpenSearch的列少，则其余列使用默认值或null。

例如需要导入的字段列表有b，c两个字段，但OpenSearch表中的字段有a，b，c三列，在列配置中可以写成”column”: \[“c”,”b”\]，表示会把Reader的第一列和第二列导入OpenSearch的c字段和b字段，而OpenSearch表中新插入纪的录的a字段会被置为默认值或null。

-   **列配置错误的处理**

    为保证写入数据的可靠性，避免多余列数据丢失造成数据质量故障。对于写入多余的列，OpenSearch Writer将报错。例如OpenSearch表字段为a，b，c，但是OpenSearch Writer写入的字段为多于3列的话，OpenSearch Writer将报错。

-   **表配置注意事项**

    OpenSearch Writer一次只能写入一个表。

-   **任务重跑和failover**

    重跑后会自动根据ID覆盖。所以插入OpenSearch的列中，必须要有一个ID，这个ID是OpenSearch的一行记录的唯一标识。唯一标识一样的数据，会被覆盖掉。

-   **任务重跑和failover**

    重跑后会自动根据ID覆盖。


OpenSearch Writer支持大部分OpenSearch类型，但也存在部分没有支持的情况，请注意检查您的类型，OpenSearch Writer针对OpenSearch类型的转换，如下表所示。

|类型分类|OpenSearch数据类型|
|:---|:-------------|
|整数类|Int|
|浮点类|Double/Float|
|字符串类|TEXT/Literal/SHORT\_TEXT|
|日期时间类|Int|
|布尔类|Literal|

## 参数说明 {#section_amt_td4_q2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|accessId|aliyun系统登录ID。|是|无|
|accessKey|aliyun系统登录Key。|是|无|
|host| |是|无|
|indexName|OpenSearch项目的名称。|是|无|
|table|写入数据的表名，不能填写多张表，因为DataX不支持同时导入多张表。|是|无|
|column|需要导入的字段列表，当导入全部字段时，可以配置为”column”: \[“\*”\]，当需要插入部分OpenSearch列填写部分列，例如：”column”: \[“id”, “name”\]。OpenSearch支持列筛选、列换序，例如：表有a，b，c三个字段，您只同步c，b两个字段。可以配置成\[“c”,”b”\]，在导入过程中，字段a自动补空，设置为null。|是|无|
|batchSize|单次写入的数据条数。OpenSearch写入为批量写入，一般来说OpenSearch的优势在查询，写入的tps不高，请根据自己的帐号申请的资源设置。一般情况OpenSearch的单条数据小于1MB，单次写入小于2MB。|如果是分区表，该选项必填，如果非分区表，该选项不可填写。|300|
|writeMode|OpenSearch Writer通过配置”writeMode”: “add/update”，保证写入的幂等性。-   “add”：当出现写入失败再次运行时，OpenSearch Writer 将清理该条数据，并导入新数据（原子操作）。
-   “update”：表示该条插入数据是以修改的方式插入的（原子操作）。

**说明：** 

OpenSearch的批量插入并非原子操作，有可能会部分成功，部分失败。所以writeMode是个非常关键的选项，对于version=v3，暂不支持update操作\*\*。


|是|无|
|ignoreWriteError|忽略写错误。配置示例：”ignoreWriteError”: true。OpenSearch的写是批量写入的，当前批次的写失败是否忽略。若忽略，则继续执行其它的写操作。若不忽略，则直接结束当前任务，并返回错误。建议使用默认值。

|否|false|
|version|opensearch的版本信息。配置示例”version”: “v3”，由于v2版本对于push操作限制比较多，建议首选v3版本。|否|v2|

## 脚本开发介绍 {#section_hy1_dys_q2b .section}

配置写入OpenSearch的数据同步作业。

\{ "type": "job", "version": "1.0", "configuration": \{ "reader": \{\}, "writer": \{ "plugin": "opensearch", "parameter": \{ "accessId": "\*\*\*\*\*\*\*\*\*", "accessKey": "\*\*\*\*\*\*\*\*", "host": "http://yyyy.aliyuncs.com", "indexName": "datax\_xxx", "table": "datax\_yyy", "column": \[ "appkey", "id", "title", "gmt\_create", "pic\_default" \], "batchSize": 500, "writeMode": add, "version":"v2", "ignoreWriteError": false \} \} \} \}

