# OTSStream配置同步任务 {#concept_llg_dpx_pfb .concept}

OTSStream插件主要用于Table Store增量数据的导出，增量数据可以看作操作日志，除数据本身外还附有操作信息。

与全量导出插件不同，增量导出插件只有多版本模式，且不支持指定列，这与增量导出的原理有关，详情请参见[配置OTSStream Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置OTSStream Reader.md#)。

**说明：** OTSStream配置同步任务时，请注意以下几点：

-   当前时间的前5分钟之前和24小时之内是可读数据。
-   设置的结束时间不能超过系统显示的时间（即您设置的结束时间要比运行时间早5分钟）。
-   配置日调度会有数据丢失。
-   不可配置周期调度和月调度。

示例如下：

开始时间和结束时间，要包含操作Table Store表的时间，例如20171019162000您有向Table Store插入2条数据，那您的时间可以设置为开始时间：20171019161000，结束时间：20171019162600。

## 新增数据源 {#section_zvh_wrx_pfb .section}

1.  以项目管理员身份登录[DataWorks控制台](https://workbench.data.aliyun.com/console)，单击对应项目下的**进入数据集成**。
2.  进入**同步资源管理** \> **数据源**页面，单击右上角的**新增数据源**。
3.  选择数据源类型为**Table Store（OTS）**，填写对话框中的配置项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40324/155140877121071_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述。|
    |**Endpoint**|Table Store Server的Endpoint，格式为`http://yyy.com`。|
    |**Table Store实例ID**|Table Store服务对应的实例ID。|
    |**AccessID/AceessKey**|访问密匙（AccessKeyID和AccessKeySecret），相当于登录密码。|

4.  单击**测试连通性**。
5.  测试连通性通过后，单击**确定**。

## 通过向导模式配置同步任务 {#section_xxh_45x_pfb .section}

1.  进入**项目空间概览** \> **任务列表**页面，单击右上角的**新建任务**。
2.  填写新建节点对话框中的配置，单击**提交**，进入数据同步任务配置页面。
3.  选择数据来源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40324/155140877121076_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源**|填写LogHub数据源的名称。|
    |**表**|导出增量数据的表的名称。该表需要开启Stream，可以在建表时开启，或者使用UpdateTable接口开启。|
    |**开始时间**|增量数据的时间范围（左闭右开）的左边界，格式yyyymmddhh24miss，单位毫秒。|
    |**结束时间**|增量数据的时间范围（左闭右开）的右边界，格式yyyymmddhh24miss，单位毫秒。|
    |**状态表**|用于记录状态的表的名称。|
    |**最大重试次数**|从TableStore中读增量数据时，每次请求的最大重试次数，默认是30。|
    |**导出时序信息**|是否导出时序信息，时序信息包含了数据的写入时间等。|

4.  选择数据去向。

    选择MaxCompute数据源及目标表。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40324/155140877121078_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源**|填写配置的数据源名称。|
    |**表**|选择需要同步的表。|
    |**分区信息**|此处需同步的表是非分区表，所以无分区信息。|
    |**清理规则**|     -   **写入前清理已有数据**：导数据之前，清空表或者分区的所有数据，相当于insert overwrite。
    -   **写入前保留已有数据**：导数据前不清理任何数据，每次运行数据都追加进去，相当于insert into。
 |
    |**压缩**|默认选择不压缩。|
    |**空字符串作为null**|默认选择否。|

5.  字段映射。

    选择字段的映射关系。需对字段映射关系进行配置，左侧源头表字段和右侧目标表字段为一一对应的关系。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40324/155140877121080_zh-CN.png)

6.  通道控制。

    配置作业速率上限和脏数据检查规则。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/155140877114354_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**DMU**|数据集成的计费单位。**说明：** 设置DMU时，需注意DMU的值限制了最大并发数的值，请合理配置。

 |
    |**作业并发数**|配置时会结合读取端指定的切分建，将数据分成多个Task，多个Task同时运行，以达到提速的效果。|
    |**同步速率**|设置同步速率可保护读取端数据库，以避免抽取速度过大，给源库造成太大的压力。同步速率建议限流，结合源库的配置，请合理配置抽取速率。|
    |**错误记录数超过**|脏数据，类似源端是Varchar类型的数据，写到Int类型的目标列中，导致因为转换不合理而无法写入的数据。同步脏数据的设置，主要在于控制同步数据的质量问题。建议根据业务情况，合理配置脏数据条数。|
    |**任务资源组**|配置同步任务时，指定任务运行所在的资源组，默认运行在默认资源组上。当项目调度资源紧张时，也可以通过新增自定义资源组的方式来给调度资源进行扩容，然后将同步任务指定在自定义资源组上运行，新增自定义资源组的操作请参见[新增任务资源](intl.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。您可根据数据源网络情况、项目调度资源情况和业务重要程度，进行合理配置。

|

7.  **保存**并**运行**任务。

    单击任务上方的**运行**按钮，将直接在数据集成页面运行任务，运行之前需要配置自定义参数的具体数值。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40324/155140877121083_zh-CN.png)


## 通过脚本模式配置同步任务 {#section_cbl_n3y_pfb .section}

如果您需要通过脚本模式配置此任务，单击工具栏中的转换脚本，选择**确认**即可进入脚本模式。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24565/155140877114347_zh-CN.png)

您可根据自身进行配置，示例脚本如下。

```
{
  "type": "job",
  "version": "1.0",
  "configuration": {
    "reader": {
      "plugin": "otsstream",
      "parameter": {
        "datasource": "otsstream",//数据源名，保持跟您添加的数据源名一致
        "dataTable": "person",//导出增量数据的表的名称。该表需要开启 Stream，可以在建表时开启，或者使用 UpdateTable 接口开启
        "startTimeString": "${startTime}",//增量数据的时间范围（左闭右开）的左边界，格式yyyymmddhh24miss，单位毫秒
        "endTimeString": "${endTime}",//运行时间
        "statusTable": "TableStoreStreamReaderStatusTable",//用于记录状态的表的名称
        "maxRetries": 30,//请求的最大重试次数
        "isExportSequenceInfo": false,
      }
    },
    "writer": {
      "plugin": "odps",
      "parameter": {
        "datasource": "odps_first",//数据源名
        "table": "person",//目标表名
        "truncate": true,
        "partition": "pt=${bdp.system.bizdate}",//分区信息
        "column": [//目标列名
          "id",
          "colname",
          "version",
          "colvalue",
          "optype",
          "sequenceinfo"
        ]
      }
    },
    "setting": {
      "speed": {
        "mbps": 7,//作业速率上限
        "concurrent": 7//并发数
      }
    }
  }
}
```

**说明：** 

-   关于运行时间参数和结束时间参数，有两种表现形式（配置任务选择其中一种）。
    -   `"startTimeString": "${startTime}"`

        增量数据的时间范围（左闭右开）的左边界，格式yyyymmddhh24miss，单位毫秒。

        `"endTimeString": "${endTime}"`

        增量数据的时间范围（左闭右开）的右边界，格式yyyymmddhh24miss，单位毫秒。

    -   `"startTimestampMillis":""`

        增量数据的时间范围（左闭右开）的左边界，单位毫秒。

        Reader插件会从statusTable中找对应startTimestampMillis的位点，从该点开始读取开始导出数据。

        若statusTable中找不到对应的位点，则从系统保留的增量数据的第一条开始读取，并跳过写入时间小于startTimestampMillis的数据。

        `"endTimestampMillis":" "`

        增量数据的时间范围（左闭右开）的右边界，单位毫秒。

        Reade插件startTimestampMilli位置开始导出数据后，当遇到第一条时间戳大于等于endTimestampMilli的数据时，结束导出数据，导出完成。

        当读取完当前全部的增量数据时，结束读取，即使未达endTimestampMillis。

        这个格式是时间式戳形式，单位毫秒。

-   如果配置isExportSequenceInfo项为true，如“isExportSequenceInfo”: true则会导出时序信息，目标会多出一行，目标column列则要多一列。时序信息包含了数据的写入时间等，默认该值为false，即不导出。

