# DataHub通过数据集成批量导入数据 {#concept_mpp_nmw_pfb .concept}

本文将为您介绍如何通过数据集成对离线DataHub进行数据的导入操作。

[数据集成](intl.zh-CN/使用指南/数据集成/数据集成简介/数据集成概述.md#)是阿里巴巴集团提供的数据同步平台。该平台具备可跨异构数据存储系统、可靠、安全、低成本、可弹性扩展等特点，可为20多种数据源提供不同网络环境下的离线（全量/增量）数据进出通道。数据源类型的详情请参见[支持的数据源](intl.zh-CN/使用指南/数据集成/数据源配置/支持的数据源.md#)。

## 准备工作 {#section_ght_gnw_pfb .section}

1.  [准备阿里云账号](../../../../../intl.zh-CN/准备工作/管理员使用云账号/准备阿里云账号.md#)，并创建账号的访问秘钥，即AccessID和AccessKey。
2.  开通MaxCompute，这样会自动产生一个默认的MaxCompute数据源，并使用主账号登录DataWorks。
3.  [创建工作空间](../../../../../intl.zh-CN/准备工作/管理员使用云账号/创建工作空间.md#)。您可以在项目中协作完成工作流，共同维护数据和任务等，因此使用DataWorks之前需要先创建一个项目。

    **说明：** 如果您想通过子账号创建数据集成任务，可以赋予其相应的权限。详情请参见[准备RAM子账号](../../../../../intl.zh-CN/准备工作/管理员使用云账号/准备RAM子账号.md#)和[成员管理](intl.zh-CN/使用指南/项目管理/成员管理.md#)。


## 操作步骤 {#section_myv_r4w_pfb .section}

以脚本模式将Stream数据同步到DataHub为例，操作如下。

1.  以开发者身份登录[DataWorks控制台](https://workbench.data.aliyun.com/console)，单击对应项目下的**进入数据集成**。
2.  进入**项目空间概览** \> **任务列表**页面，单击右上角的**新建任务**。
3.  填写新建节点对话框中的配置，单击**提交**，进入数据同步任务配置页面。
4.  单击新建同步节点右上角的**转换脚本**，选择**确认**即可进入脚本模式。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24565/155142071714347_zh-CN.png)

5.  单击工具栏中的**导入模板**，填写导入模板对话框中的配置。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40315/155142071721027_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**来源类型**|此处选择**Stream**类型。|
    |**目标类型**|此处选择**Elasticsearch**类型。|
    |**数据源**|选择配置好的数据源。**说明：** 如果没有提前配置数据源，可单击**新增数据源**进行新增操作。

|

6.  单击**确认**生成初始脚本，可根据自身情况进行配置。

    ```
    {
    "type": "job",
    "version": "1.0",
    "configuration": {
     "setting": {
       "errorLimit": {
         "record": "0"
       },
       "speed": {
         "mbps": "1",
         "concurrent": 1,//作业并发数
         "dmu": 1,//数据移动单位 (DMU) 是数据集成消耗资源（包含 CPU、内存、网络等资源分配）的度量单位
         "throttle": false
       }
     },
     "reader": {
       "plugin": "stream",
       "parameter": {
         "column": [//源端列名
           {
             "value": "field",//列属性
             "type": "string"
           },
           {
             "value": true,
             "type": "bool"
           },
           {
             "value": "byte string",
             "type": "bytes"
           }
         ],
         "sliceRecordCount": "100000"
       }
     },
     "writer": {
       "plugin": "datahub",
       "parameter": {
         "datasource": "datahub",//数据源名
         "topic": "xxxx",//Topic是DataHub订阅和发布的最小单位，您可以用Topic来表示一类或者一种流数据。
         "mode": "random",//随机写入。
         "shardId": "0",//Shard 表示对一个Topic进行数据传输的并发通道，每个Shard会有对应的ID。
         "maxCommitSize": 524288,//为了提高写出效率，待攒数据大小达到maxCommitSize大小（单位MB）时，批量提交到目的端。默认是1048576 ，即1MB数据。
         "maxRetryCount": 500
       }
     }
    }
    }
    ```

7.  单击**保存**并**运行**。

    **说明：** 

    -   DataHub仅支持以脚本模式导入数据。
    -   如果想选择新模板，可单击工具栏中的**导入模板**，一旦导入新模板，原有内容将会被全部覆盖。
    -   同步任务保存后，直接单击**运行**，任务会立刻运行。

        您也可单击**提交**，将同步任务提交到调度系统中，调度系统会按照配置属性在从第二天开始自动定时执行。


## 参考文档 {#section_cvw_n2b_pfb .section}

其他的配置同步任务详细信息请参见下述文档。

-   [配置Reader插件](https://www.alibabacloud.com/help/faq-list/74300.htm)。
-   [配置Writer插件](https://www.alibabacloud.com/help/faq-list/74301.htm)。

