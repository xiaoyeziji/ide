# 配置OTSStream Reader {#concept_m4h_5gr_p2b .concept}

本文为您介绍OTSStream Reader支持的数据类型、读取方式、字段映射和数据源等参数及配置举例。

OTSStream Reader插件主要用于Table Store增量数据的导出，增量数据可以看作操作日志，除数据本身外还附有操作信息。

与全量导出插件不同，增量导出插件只有多版本模式，且不支持指定列，这与增量导出的原理有关，导出格式的详细介绍请参见下文。

使用插件前必须确保表上已经开启Stream功能，您可以在建表的时候指定开启，也可以使用SDK的UpdateTable接口开启。

开启Stream的方法，如下所示。

```
SyncClient client = new SyncClient("", "", "", "");
建表的时候开启：
CreateTableRequest createTableRequest = new CreateTableRequest(tableMeta);
createTableRequest.setStreamSpecification(new StreamSpecification(true, 24)); // 24代表增量数据保留24小时
client.createTable(createTableRequest);
如果建表时未开启，可以通过UpdateTable开启:
UpdateTableRequest updateTableRequest = new UpdateTableRequest("tableName");
updateTableRequest.setStreamSpecification(new StreamSpecification(true, 24)); 
client.updateTable(updateTableRequest);
```

## 实现原理 {#section_ngl_bhr_p2b .section}

您使用SDK的UpdateTable功能，指定开启Stream并设置过期时间，即开启了增量功能。开启后，Table Store服务端就会将您的操作日志额外保存起来，每个分区有一个有序的操作日志队列，每条操作日志会在一定时间后被垃圾回收，这个时间即为您指定的过期时间。

Table Store的SDK提供了几个Stream相关的API用于将这部分操作日志读取出来，增量插件也是通过Table Store SDK的接口获取到增量数据的，并将增量数据转化为多个6元组的形式（pk、colName、version、colValue、opType和sequenceInfo）导入到MaxCompute中。

## 导出的数据格式 {#section_n3v_khr_p2b .section}

在Table Store多版本模型下，表中的数据组织为行\>列\>版本三级的模式， 一行可以有任意列，列名也并非固定的，每一列可以含有多个版本，每个版本都有一个特定的时间戳（版本号）。

您可以通过Table Store的API进行一系列读写操作，Table Store通过记录您最近对表的一系列写操作（或数据更改操作）来实现记录增量数据的目的，所以您也可以把增量数据看作一批操作记录。

Table Store有PutRow、UpdateRow和DeleteRow三类数据更改操作。

-   PutRow：写入一行，若该行已存在即覆盖该行。
-   UpdateRow：更新一行，对原行其他数据不做更改， 更新可能包括新增或覆盖（若对应列的对应版本已存在）一些列值、删除某一列的全部版本、删除某一列的某个版本。
-   DeleteRow：删除一行。

Table Store会根据每种操作生成对应的增量数据记录，Reader插件会读出这些记录，并导出为Datax的数据格式。

同时，由于Table Store具有动态列、多版本的特性，所以Reader插件导出的一行不对应Table Store中的一行，而是对应Table Store中的一列的一个版本。即Table Store中的一行可能会导出很多行，每行包含主键值、该列的列名、该列下该版本的时间戳（版本号）、该版本的值、操作类型。如果设置isExportSequenceInfo为true，还会包括时序信息。

转换为Datax的数据格式后，定义了四种操作类型，如下所示。

-   U（UPDATE）：写入一列的一个版本。
-   DO（DELETE\_ONE\_VERSION）：删除某一列的某个版本。
-   DA（DELETE\_ALL\_VERSION）：删除某一列的全部版本，此时需要根据主键和列名，将对应列的全部版本删除。
-   DR（DELETE\_ROW）：删除某一行，此时需要根据主键，将该行数据全部删除。

假设该表有两个主键列，主键列名分别为pkName1， pkName2，示例如下。

|pkName1|pkName2|columnName|timestamp|columnValue|opType|
|-------|-------|----------|---------|-----------|------|
|pk1\_V1|pk2\_V1|col\_a|1441803688001|col\_val1|U|
|pk1\_V1|pk2\_V1|col\_a|1441803688002|col\_val2|U|
|pk1\_V1|pk2\_V1|col\_b|1441803688003|col\_val3|U|
|pk1\_V2|pk2\_V2|col\_a|1441803688000|—|DO|
|pk1\_V2|pk2\_V2|col\_b|—|—|DA|
|pk1\_V3|pk2\_V3|—|—|—|DR|
|pk1\_V3|pk2\_V3|col\_a|1441803688005|col\_val1|U|

假设导出的数据如上，共7行，对应Table Store表内的3行，主键分别是（pk1\_V1，pk2\_V1），（pk1\_V2， pk2\_V2），（pk1\_V3， pk2\_V3）。

-   对于主键为（pk1\_V1，pk2\_V1）的一行，包含三个操作，分别是写入col\_a列的两个版本和col\_b列的一个版本。
-   对于主键为（pk1\_V2，pk2\_V2）的一行，包含两个操作，分别是删除col\_a列的一个版本、删除col\_b列的全部版本。
-   对于主键为（pk1\_V3， pk2\_V3）的一行，包含两个操作，分别是删除整行、写入col\_a列的一个版本。


目前OTSStream Reader支持所有的OTS类型，其针对OTS类型的转换列表，如下所示。

|类型分类|OTSStream数据类型|
|:---|:------------|
|整数类|Integer|
|浮点类|Double|
|字符串类|String|
|布尔类|Boolean|
|二进制类|Binary|

## 参数说明 {#section_nc1_jjr_p2b .section}

|参数|描述|必选|默认值|
|:-|:-|:-|:--|
|dataSource|数据源名称，脚本模式支持添加数据源，此配置项填写的内容必须要与添加的数据源名称保持一致。|是|无|
|dataTable|导出增量数据的表的名称。该表需要开启Stream，可以在建表时开启，或者使用UpdateTable接口开启。|是|无|
|statusTable|Reader插件用于记录状态的表的名称，这些状态可用于减少对非目标范围内的数据的扫描，从而加快导出速度。statusTable是Reader用于保存状态的表，如果该表不存在，Reader会自动创建该表，一次离线导出任务完成后，您不需删除该表，该表中记录的状态可用于下次导出任务中。-   您不需要创建该表，只需要给出一个表名。Reader插件会尝试在您的instance下创建该表，如果该表不存在即创建新表，如果该表已存在，会判断该表的Meta是否与期望一致，如果不一致会抛出异常。
-   在一次导出完成之后，您不需删除该表，该表的状态可用于下次导出任务。
-   该表会开启TTL，数据自动过期，因此可认为其数据量很小。
-   针对同一个instance下的多个不同的dataTable的Reader配置，可以使用同一个statusTable，记录的状态信息互不影响。

综上所述，您配置一个类似TableStoreStreamReaderStatusTable的名称即可，请注意不要与业务相关的表重名。|是|无|
|startTimestampMillis|增量数据的时间范围（左闭右开）的左边界，单位毫秒。-   Reader插件会从statusTable中找对应startTimestampMillis的位点，从该点开始读取开始导出数据。
-   如果statusTable中找不到对应的位点，则从系统保留的增量数据的第一条开始读取，并跳过写入时间小于startTimestampMillis的数据。

|否|无|
|endTimestampMillis|增量数据的时间范围（左闭右开）的右边界，单位毫秒。-   Reader插件从startTimestampMillis位置开始导出数据后，当遇到第一条时间戳大于等于endTimestampMillis的数据时，结束导出数据，导出完成。
-   当读取完当前全部的增量数据时，结束读取，即使未达到endTimestampMillis。

|否|无|
|date|日期格式为yyyyMMdd，如20151111，表示导出该日的数据。如果没有指定date，则必须指定startTimestampMillis和endTimestampMillis，反之也成立。例如采云间调度只支持天级别，所以提供该配置，作用与startTimestampMillis和endTimestampMillis类似。|否|无|
|isExportSequenceInfo|是否导出时序信息，时序信息包含了数据的写入时间等。默认该值为false，即不导出。|否|无|
|maxRetries|从TableStore中读增量数据时，每次请求的最大重试次数，默认为30，重试之间有间隔，重试30次的总时间约为5分钟，一般无需更改。|否|无|
|startTimeString|增量数据的时间范围（左闭右开）的左边界，格式为yyyymmddhh24miss，单位毫秒。|否|无|
|endTimeString|增量数据的时间范围（左闭右开）的右边界，格式为yyyymmddhh24miss，单位毫秒。|否|无|

## 向导开发介绍 {#section_q1t_vkr_p2b .section}

暂不支持向导模式开发。

## 脚本开发介绍 {#section_ewq_xkr_p2b .section}

脚本配置样例如下所示，具体参数填写请参见参数说明。

```
{
    "type":"job",
    "version":"2.0",//版本号
    "steps":[
        {
            "stepType":"otsstream",//插件名
            "parameter":{
                "statusTable":"TableStoreStreamReaderStatusTable",//用于记录状态的表的名称
                "maxRetries":30,//从 TableStore 中读增量数据时，每次请求的最大重试次数，默认为 30
                "isExportSequenceInfo":false,//是否导出时序信息
                "datasource":"$srcDatasource",//数据源
                "startTimeString":"${startTime}",//增量数据的时间范围（左闭右开）的左边界
                "table":"",//表名
                "endTimeString":"${endTime}"//增量数据的时间范围（左闭右开）的右边界
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

