# 配置LogHub Reader {#concept_ihy_fvq_p2b .concept}

日志服务（Log Service，简称LOG/原SLS）是针对实时数据的一站式服务，在阿里集团经历大量大数据场景锤炼而成。提供日志类数据采集、消费、投递及查询分析功能，全面提升海量日志处理/分析能力。LogHub Reader是使用日志服务的Java SDK消费LogHub中的实时日志数据，并将日志数据转换为数据集成传输协议传递给Writer。

## 实现原理 {#section_ig1_4vq_p2b .section}

LogHub Reader通过日志服务Java SDK消费LogHub中的实时日志数据，具体使用的日志服务Java SDK版本，如下所示。

```
<dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>aliyun-log</artifactId>
    <version>0.6.7</version>
</dependency>
```

日志库（Logstore）是日志服务中日志数据的采集、存储和查询单元，Logstore读写日志必定保存在某一个分区（Shard）上。每个日志库分若干个分区，每个分区由MD5左闭右开区间组成，每个区间范围不会相互覆盖，并且所有的区间的范围是MD5整个取值范围，每个分区可提供一定的服务能力。

-   写入：5MB/s，2000次/s。
-   读取：10MB/s，100次/s。

LogHub Reader消费Shard中的日志，具体消费过程（GetCursor、BatchGetLog相关API）如下所示。

-   根据时间区间范围获得游标。
-   通过游标、步长参数读取日志，同时返回下一个位置游标。
-   不断移动游标进行日志消费。
-   根据Shard进行任务的切分并发执行。

LogHub Reader针对LogHub类型的转换列表，如下所示。

|DataX内部类型|LogHub数据类型|
|:--------|:---------|
|String|String|

## 参数说明 {#section_jn2_gqh_p2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|endpoint| 日志服务入口endpoint是访问一个项目（Project）及其内部日志数据的URL。它和Project所在的阿里云区域（Region）及Project名称相关。各Region的服务入口请参见[服务入口](https://www.alibabacloud.com/help/zh/doc-detail/29008.htm)。

 |是|无|
|accessId|访问日志服务的访问密钥，用于标识用户。|是|无|
|accessKey|访问日志服务的访问密钥，用来验证用户的密钥。|是|无|
|project|目标日志服务的项目名字，是日志服务中的资源管理单元，用于资源隔离和控制。|是|无|
|logstore|目标日志库的名字，logstore是日志服务中日志数据的采集、存储和查询单元。|是|无|
|batchSize|一次从日志服务查询的数据条数。|否|128|
|column|每条数据中column的名字，这里可以配置日志服务中的元数据作为同步列，支持的元数据有"C\_Topic", "C\_MachineUUID", "C\_HostName", "C\_Path", "C\_LogTime"等。分表表示日志主题、采集机器唯一标识，主机名，路径，日志时间等。

**说明：** 列名区分大小写。

|是|无|
|beginDateTime|数据消费的开始时间位点，为时间范围（左闭右开）的左边界，为yyyyMMddHHmmss格式的时间字符串（例如20180111013000），可以和DataWorks的调度时间参数配合使用。**说明：** beginDateTime和endDateTime组合配套使用。

|和endTimestampMillis选择一种|空|
|endDateTime|数据消费的结束时间位点，为时间范围（左闭右开）的右边界，为yyyyMMddHHmmss格式的时间字符串（例如20180111013010），可以和DataWorks的调度时间参数配合使用。**说明：** endDateTime和beginDateTime组合配套使用。

|否|无|
|beginTimestampMillis|数据消费的开始时间位点，为时间范围（左闭右开）的左边界，单位毫秒。**说明：** beginTimestampMillis和endTimestampMillis组合配套使用。

-1表示日志服务游标的最开始CursorMode.BEGIN。推荐使用beginDateTime模式。

|和beginDateTime选择一种|无|
|endTimestampMillis|数据消费的开始时间位点，为时间范围（左闭右开）的右边界，单位毫秒。**说明：** endTimestampMillis和beginTimestampMillis组合配套使用。

-1表示日志服务游标的最后位置CursorMode.END。推荐使用endDateTime模式。

|和endDateTime选择一种|无|

## 向导开发介绍 {#section_bp2_wsh_p2b .section}

1.  选择数据源

    配置同步任务的数据来源和数据去向。

    配置项说明如下：

    -   数据源：即上述参数说明中的datasource，一般填写您配置的数据源名称。
    -   日志开始时间：数据消费的开始时间位点，为时间范围（左闭右开）的左边界，为yyyyMMddHHmmss格式的时间字符串（例如20180111013000），可以和DataWorks的调度时间参数配合使用。
    -   日志结束时间：数据消费的开始时间位点，为时间范围（左闭右开）的右边界，为yyyyMMddHHmmss格式的时间字符串（例如20180111013010），可以和DataWorks的调度时间参数配合使用。
2.  字段映射，即上述参数说明中的column。

    左侧的源头表字段和右侧的目标表字段为一一对应的关系，单击**添加一行**可增加单个字段，单击**删除**即可删除当前字段 。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16233/15367219357879_zh-CN.png)

    -   同行映射：单击**同行映射**可以在同行建立相应的映射关系，请注意匹配数据类型。
    -   自动排版：可以根据相应的规律自动排版。
    -   手动编辑源表字段：请手动编辑字段，一行表示一个字段，首尾空行会被采用，其他空行会被忽略。
    添加一行的功能如下所示：

    -   可以输入常量，输入的值需要使用英文单引号，如'abc'、'123'等。
    -   可以配合调度参数使用，如$\{bizdate\}等。
    -   可以输入关系数据库支持的函数，如now\(\)、count\(1\)等。
    -   如果您输入的值无法解析，则类型显示为未识别。
3.  通道控制

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16221/15367219357675_zh-CN.png)

    配置项说明如下：

    -   DMU：数据集成消耗资源（包括CPU、内存、网络等资源分配）的度量单位。一个DMU描述了一个数据集成作业最小运行能力，即在限定的CPU、内存、网络等资源情况下对于数据同步的处理能力。
    -   作业并发数：可将此属性视为数据同步任务内，可从源并行读取或并行写入数据存储端的最大线程数。向导模式通过界面化配置并发数，指定任务所使用的并行度
    -   错误记录数：错误记录数，表示脏数据的最大容忍条数。
    -   任务资源组：任务运行的机器，如果任务数比较多，使用默认资源组出现等待资源的情况，建议添加自定义资源组（目前只有华东1，华东2支持添加自定义资源组），详情请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。

## 脚本开发介绍 {#section_cp2_wsh_p2b .section}

脚本配置样例如下所示，具体参数填写请参见参数说明。

```
{
 "type":"job",
 "version":"2.0",//版本号
 "steps":[
     {
         "stepType":"loghub",//插件名
         "parameter":{
             "datasource":"",//数据源
             "column":[//字段
                 "col0",
                 "col1",
                 "col2",
                 "col3",
                 "col4",
                 "C_Topic",//日志主题
                 "C_HostName",//主机名
                 "C_Path",//路径
                 "C_LogTime"//日志时间
             ],
             "beginDateTime":"",//数据消费的开始时间位点
             "batchSize":"",//一次从日志服务查询的数据条数
             "endDateTime":"",//数据消费的结束时间位点
             "fieldDelimiter":",",//列分隔符
             "encoding":"UTF-8",//编码格式
             "logstore":""//：目标日志库的名字
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

