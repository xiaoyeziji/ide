# 通过数据集成导入数据到Elasticsearch {#concept_ckd_32b_pfb .concept}

本文将为您介绍如何通过数据集成对离线Elasticsearch进行数据导入的操作。

[数据集成](intl.zh-CN/使用指南/数据集成/数据集成简介/数据集成概述.md#)是阿里巴巴集团提供的数据同步平台。该平台具备可跨异构数据存储系统、可靠、安全、低成本、可弹性扩展等特点，可为20多种数据源提供不同网络环境下的离线（全量/增量）数据进出通道。数据源类型的详情请参见[支持的数据源](intl.zh-CN/使用指南/数据集成/数据源配置/支持的数据源.md#)。

## 前提条件 {#section_s33_q52_4fb .section}

通过数据集成导入数据前，您需要满足以下条件。

-   [准备阿里云账号](../../../../../intl.zh-CN/准备工作/管理员使用云账号/准备阿里云账号.md#)，并建好账号的访问秘钥，即AccessKeys。
-   开通MaxCompute，自动产生一个默认的MaxCompute数据源。
-   已使用主账号[创建项目](../../../../../intl.zh-CN/准备工作/管理员使用云账号/创建工作空间.md#)。

    您可以在项目中协作完成工作流，共同维护数据和任务等，因此使用DataWorks之前需要先创建一个项目。

    **说明：** 如果您想通过子账号创建数据集成任务，可以赋予其相应的权限。详情请参见[准备RAM子账号](../../../../../intl.zh-CN/准备工作/管理员使用云账号/准备RAM子账号.md#)和[成员管理](intl.zh-CN/使用指南/项目管理/成员管理.md#)。

-   配置好相关的数据源，详情请参见[数据源配置](https://www.alibabacloud.com/help/faq-list/72788.htm)。

## 操作步骤 {#section_q2s_hv2_4fb .section}

1.  以开发者身份登录[DataWorks控制台](https://workbench.data.aliyun.com/console)，单击对应项目下的**进入数据开发**。

    ![](images/14344_zh-CN.jpeg)

2.  右键单击**业务流程**，选择**新建业务流程**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24565/154890276814345_zh-CN.png)

3.  右键单击业务流程下的**数据集成**，选择**新建数据集成节点** \> **同步节点**。
4.  填写新建节点对话框中的配置，单击**提交**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24565/154890276814346_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**节点类型**|默认数据同步类型。|
    |**节点名称**|填写该节点名称。|
    |**目标文件夹**|默认放在相应的业务流程下。|

5.  单击新建同步节点右上角的**转换脚本**，选择**确认**即可进入脚本模式。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24565/154890276814347_zh-CN.png)

6.  单击工具栏中的**导入模板**，填写导入模板对话框中的配置。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24565/154890276814348_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**来源类型**|此处选择**MySQL**类型。|
    |**数据源**|选择配置好的数据源。|
    |**目标类型**|此处选择**Elasticsearch**类型。|

7.  单击**确认**生成初始脚本，可根据自身情况进行配置。

    ```
    {
    "configuration": {
    "setting": {
      "speed": {
        "concurrent": "1", //作业并发数
        "mbps": "1" //作业速率上限 
      }
    },
    "reader": {
      "parameter": {
        "connection": [
          {
            "table": [
              "`es_table`" //源端表名
            ],
            "datasource": "px_mysql_OK" //数据源名，建议跟添加的数据源名保持一致
          }
        ],
        "column": [ //源端表的列名
          "col_ip",
          "col_double",
          "col_long",
          "col_integer",
          "col_keyword",
          "col_text",
          "col_geo_point",
          "col_date"
        ],
        "where": "", //过滤条件
      },
      "plugin": "mysql"
    },
    "writer": {
      "parameter": {
        "cleanup": true, //是否在每次导入数据到Elasticsearch的时候清空原有数据，全量导入/重建索引的时候需要设置为true，同步增量的时候必须为false，这里因为是同步，则需要设置为false。
        "accessKey": "nimda", //如果使用了X-PACK插件，则这里需要填写password，如果没使用，则这里填空字符串即可。阿里云Elasticsearch使用了X-PACK插件，这里需要填写password。
        "index": "datax_test", // Elasticsearch的索引名称，如果之前没有，插件会自动创建。
        "alias": "test-1-alias", //数据导入完成后写入别名
        "settings": {
          "index": {
            "number_of_replicas": 0,
            "number_of_shards": 1
          }
        },
        "batchSize": 1000, //每次批量数据的条数
        "accessId": "default", //如果使用了X-PACK插件，则这里需要填写username，如果没使用，则这里填空字符串即可。阿里云Elasticsearch使用了X-PACK插件，这里需要填写username。
        "endpoint": "http://xxx.xxxx.xxx:xxxx", //Elasticsearch的连接地址，控制台上有
        "splitter": ",", //如果插入数据是array，就使用指定分隔符。
        "indexType": "default", //Elasticsearch中相应索引下的类型名称。
        "aliasMode": "append", //数据导入完成后增加别名的模式，append(增加模式), exclusive（只留这一个）。
        "column": [ //Elasticsearch中的列名，顺序和Reader中的Column顺序一致。
          {
            "name": "col_ip",//对应于TableStore中的属性列：name
            "type": "ip"//文本类型，采用默认分词
          },
          {
            "name": "col_double",
            "type": "string"
          },
          {
            "name": "col_long",
            "type": "long"
          },
          {
            "name": "col_integer",
            "type": "integer"
          },
          {
            "name": "col_keyword",
            "type": "keyword"
          },
          {
            "name": "col_text",
            "type": "text"
          },
          {
            "name": "col_geo_point",
            "type": "geo_point"
          },
          {
            "name": "col_date",
            "type": "date"
          }
        ],
        "discovery": false//是否自动发现，设置为true
      },
      "plugin": "elasticsearch"//Writer插件的名称：ElasticsearchWriter，不需要修改
    }
    },
    "type": "job",
    "version": "1.0"
    }
    ```

8.  单击**保存**并**运行**。

    **说明：** 

    -   Elasticsearch仅支持以脚本模式导入数据。
    -   如果想选择新模板，可单击工具栏中的**导入模板**，一旦导入新模板，原有内容将会被全部覆盖。
    -   同步任务保存后，直接单击**运行**，任务会立刻运行。您也可单击**提交**，将同步任务提交到调度系统中，调度系统会按照配置属性在从第二天开始自动定时执行。

## 参考文档 {#section_cvw_n2b_pfb .section}

其他的配置同步任务详细信息请参见下述文档。

-   [配置Reader插件](https://www.alibabacloud.com/help/faq-list/74300.htm)。
-   [配置Writer插件](https://www.alibabacloud.com/help/faq-list/74301.htm)。

