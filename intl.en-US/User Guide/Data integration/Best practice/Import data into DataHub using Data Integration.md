# Import data into DataHub using Data Integration {#concept_mpp_nmw_pfb .concept}

This topic explains how to import data into offline DataHub by using Data Integration.

[Data Integration](reseller.en-US/User Guide/Data integration/Data integration introduction/Data integration overview.md#) is a data synchronization platform provided by Alibaba Group. Data Integration is a reliable, secure, cost-effective, elastic, scalable data synchronization platform. Data Integration can be used across heterogeneous data storage systems and provides offline \(full/incremental\) data synchronization channels in different network environments for more than 20 types of data sources. For more information about data source types, see [Supported data sources](reseller.en-US/User Guide/Data integration/Data source configuration/Supported data sources.md#).

## Prerequisites {#section_ght_gnw_pfb .section}

1.  [Prepare Alibaba Cloud account](../../../../../reseller.en-US/Preparation/Administrator Operations/Alibaba Cloud Account Preparations.md#)An Alibaba Cloud account and logon credentials \(AccessID and AccessKey\) for the account.
2.  Activate MaxCompute, and then a default MaxCompute data source is automatically created. Log on to the DataWorks console using the Alibaba Cloud account.
3.  [Create a project](../../../../../reseller.en-US/Preparation/Administrator Operations/Create Workspace.md#)Create a project. To use DataWorks, first create a project. Then, you can complete the workflow and maintain data and tasks through collaboration within the project.

    **Note:** If you want to create Data Integration tasks using a RAM user, you must grant required permissions to it. For more information, see [Create a sub-account](../../../../../reseller.en-US/Preparation/Administrator Operations/Create a RAM user.md#) and [Member management](reseller.en-US/User Guide/Project management/User management.md#).


## Procedure {#section_myv_r4w_pfb .section}

In the following example, the Stream data is synchronized to DataHub and the synchronization task is configured in script mode:

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as a developer, find the project, and then click **Data Integration**.
2.  Choose **Overview** \> **Tasks** and click **Create Task** in the upper-right corner.
3.  Complete the configurations in the Create Node dialog box and click **Submit**. The configuration page of the data synchronization task appears.
4.  Click **Switch to Script Mode** in the toolbar and click **OK** to switch to script mode.
5.  Click **Import Template** in the toolbar and set up configurations in the Import Template dialog box.

    |Configuration|Description|
    |:------------|:----------|
    |**Source Type**|In this example, select **Stream**.|
    |**Destination Type**|In this example, select **DataHub**.|
    |**Data Source**|Select a configured data source as the destination.**Note:** If no data source is configured, click **Add Data Source** to add one.

|

6.  Click **OK** to generate an initial script. Then, complete the configurations as needed.

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
         "concurrent":"1",//Number of concurrent jobs
         "dmu": 1,//Data migration unit (DMU) is a measurement unit, which measures the resources (including CPU, memory, and network bandwidth) consumed by Data Integration.
         "throttle": false
       }
     },
     "reader": {
       "plugin": "stream",
       "parameter": {
         "column": [//Column name of the source
           {
             "value": "field",//Column properties
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
         "datasource": "datahub",//Data source name
         "topic": "xxxx",//Topic is the minimum unit of DataHub subscription and publishing, which can be used to represent a type of streaming data.
         "mode": "random",//Random write.
         "shardId": "0",//Shard represents a concurrent channel for data transmission of a topic, and each shard has a corresponding ID.
         "maxCommitSize": 524288,//To improve writing performance, configure the system to write data to the destination in batches when the size of the collected data reaches maxCommitSize (in MB). The default value is 1048576 (1 MB).
         "maxRetryCount": 500
       }
     }
    }
    }
    ```

7.  Click **Save** and **Run**.

    **Note:** 

    -   DataHub only supports importing data in script mode.
    -   To use a new template, click **Import Template** in the toolbar. The existing content is overwritten once the script is imported.
    -   After saving the synchronization task, click **Run** to immediately run the task.

        Alternatively, click **Submit** to submit the synchronization task to the scheduling system. The scheduling system periodically runs the task starting from the next day according to the task configurations.


## Reference {#section_cvw_n2b_pfb .section}

For more information about how to configure synchronization tasks, see the following topics.

-   [Configure the Reader plug-in](https://www.alibabacloud.com/help/faq-list/74300.htm).
-   [Configure the Writer plug-in](https://www.alibabacloud.com/help/faq-list/74301.htm).

