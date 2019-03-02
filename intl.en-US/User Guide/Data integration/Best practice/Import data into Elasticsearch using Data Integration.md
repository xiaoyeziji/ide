# Import data into Elasticsearch using Data Integration {#concept_ckd_32b_pfb .concept}

This topic describes how to offline import data into Elasticsearch by using Data Integration.

[Data Integration](reseller.en-US/User Guide/Data integration/Data integration introduction/Data integration overview.md#) is a data synchronization platform provided by Alibaba Group. Data Integration is a reliable, secure, cost-effective, elastic, scalable data synchronization platform. Data Integration can be used across heterogeneous data storage systems and provides offline \(full/incremental\) data synchronization channels in different network environments for more than 20 types of data sources. For more information about data source types, see [Supported data sources](reseller.en-US/User Guide/Data integration/Data source configuration/Supported data sources.md#).

## Prerequisites {#section_s33_q52_4fb .section}

Before importing data using Data Integration, you must:

-   [Prepare Alibaba Cloud account](../../../../../reseller.en-US/Preparation/Administrator Operations/Alibaba Cloud Account Preparations.md#)Sign up for an Alibaba Cloud account and create AccessKeys for this account.
-   Activate MaxCompute, and then a default MaxCompute data source is automatically created.
-   [Create a project](../../../../../reseller.en-US/Preparation/Administrator Operations/Create Workspace.md#) with the Alibaba Cloud account.

    To use DataWorks, first create a project. Then, you can complete the workflow and maintain data and tasks through collaboration within the project.

    **Note:** You can grant RAM users the permissions to create Data Integration tasks. For more information, see [Create a sub-account](../../../../../reseller.en-US/Preparation/Administrator Operations/Create a RAM user.md#) and [Member management](reseller.en-US/User Guide/Project management/User management.md#).

-   Configure data sources. For more information, see [Data source config](https://www.alibabacloud.com/help/faq-list/72788.htm).

## Procedure {#section_q2s_hv2_4fb .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as a developer, find the project, and then click **Data Integration**.
2.  Right click **Business Flow** and select **Create Business Flow**.
3.  Right click **Data Integration** under the created business flow and choose **Create Data IntegrationNode ID** \> **Data Sync**.
4.  Set up configurations in the Create Node dialog box and click **Submit**.

    |Configuration|Description|
    |:------------|:----------|
    |**Node Type**|Defaults to Data Sync.|
    |**Node Name**|The name of the node.|
    |**Destination folder**|The node is located in the corresponding process by default.|

5.  Click **Switch to Script Mode** in the navigation bar and click **Ok**.
6.  Click **Import Template** in the toolbar and set up configurations in the Import Template dialog box.

    |Configuration|Description|
    |:------------|:----------|
    |**Source Type**|In this example, select **MySQL**.|
    |**Data Source**|Select a configured data source.|
    |**Destination Type**|In this example, select **Elasticsearch**.|

7.  Click **Ok** to generate an initial script and set up configurations as needed.

    ```
    {
    "configuration": {
    "setting": {
      "speed": {
        "concurrent": "1", //Number of concurrent jobs
        "mbps": "1" //Maximum transmission rate 
      }
    },
    "reader": {
      "parameter": {
        "connection": [
          {
            "table": [
              "`es_table`" //Source table name
            ],
            "datasource": "px_mysql_OK" //Data source name. We recommend you use the same data source name as the one you added.
          }
        ],
        "column": [ //Column names in the source table
          "col_ip",
          "col_double",
          "col_long",
          "col_integer",
          "col_keyword",
          "col_text",
          "col_geo_point",
          "col_date"
        ],
        "where": "", //Filtering condition
      },
      "plugin": "mysql"
    },
    "writer": {
      "parameter": {
        "cleanup": true, //Whether to clear the original data when importing the data to Elasticsearch each time. Set to true when performing full import or when rebuilding indexes. Set to false when synchronizing incremental data. For the data synchronization in this example, set it to false.
        "accessKey": "nimda", //In this example, the password is required because the X-Pack plugin is used. If the plugin is not used, set it to an empty string.
        "index": "datax_test", //Index name of Elasticsearch. If it is unavailable, the plugin will create one automatically.
        "alias": "test-1-alias", //The alias to which the data is written after the data is imported.
        "settings": {
          "index": {
            "number_of_replicas": 0,
            "number_of_shards": 1
          }
        },
        "batchSize": 1000, //The number of data entries per batch.
        "accessId": "default", //If the X-PACK plug-in is used, enter the username here, and if not, enter an empty string. Because the X-PACK plug-in is used for Alibaba Cloud Elasticsearch, a username is required here.
        "endpoint": "http://example.com:port", //The address to Elasticsearch, which can be found on the console.
        "splitter": ",", //Specify a delimiter if arrays are inserted.
        "indexType": "default", //The type name under the corresponding index in Elasticsearch.
        "aliasMode": "append", //The mode of adding an alias after the data is imported: append and exclusive.
        "column": [ //Column names in Elasticsearch, whose order is the same as that of columns in Reader.
          {
            "name": "col_ip",//Corresponds to the property column “name” in TableStore.
            "type": "ip"//Text type, the default analyzer is used.
          },
          {
            "name": "col_double",
            "type": "string",
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
            "name": "col_text ",
            "type": "text"
          },
          {
            "name": "col_geo_point",
            "type": "geo_point"
          },
          {
            "name": "col_date ",
            "type": "date"
          }
        ],
        "discovery": false//Set to true to enable automatic discovery.
      },
      "plugin": "elasticsearch"//Name of the Writer plugin: ElasticSearchWriter, leave it as the default.
    }
    },
    "type": "job",
    "version": "1.0"
    }
    ```

8.  Click **Save** and **Run**.

    **Note:** 

    -   Elasticsearch only supports importing data in script mode.
    -   If you want to use a new template, click **Import Template** in the toolbar. The existing content is overwritten once the script is reset.
    -   After saving the synchronization task, click **Run** to immediately run the task. Alternatively, click **Submit** to submit the synchronization task to the scheduling system. The scheduling system periodically runs the task starting from the next day according to the task configurations.

## Reference {#section_cvw_n2b_pfb .section}

For more information about how to configure synchronization tasks, see the following documents.

-   [Configure the Reader plug-in](https://www.alibabacloud.com/help/faq-list/74300.htm).
-   [Configure the Writer plug-in](https://www.alibabacloud.com/help/faq-list/74301.htm).

