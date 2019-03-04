# Script mode configuration {#concept_olb_drc_p2b .concept}

This topic describes how to configure tasks through the data integration Script mode.

The task configuration steps are as follows:

1.  Create a data source.
2.  Create a synchronization task.
3.  Import a template.
4.  Configure the synchronization task reader.
5.  Configure the synchronization task writer.
6.  Configure the mapping between the synchronization task reader and the synchronization task writer.
7.  Configure the DMUs, concurrency, transmission rates, dirty data records, resource groups, and other synchronization task information.
8.  Configure the scheduling attribute of the synchronization task.

**Note:** The following introduces the specific implementation of operation steps, each of the following steps jumps to the corresponding topic. After completing the current step, click the link to return to this article to go on to the next step.

## Create data source {#section_vkd_w2c_p2b .section}

Synchronization tasks supports data transmission between various homogenous and heterogeneous data sources. You need to register the target data source in Data Integration, and then you can select the data source when configuring a synchronization task on Data Integration. Integrate data source types that support synchronization as shown in [Supported data sources](intl.en-US/User Guide/Data integration/Data source configuration/Supported data sources.md#).

After confirming the target data source is supported by Data Integration, you can register the data source in Data Integration. For detailed data source registration, see [Configuring data source information](intl.en-US/User Guide/Data integration/Data source configuration/Supported data sources.md#).

**Note:** 

-   For some data sources, Data Integration does not support test connectivity. For more information on data source test connectivity, see [Test data source connectivity](intl.en-US/User Guide/Data integration/Data source configuration/Test data source connectivity.md#).
-   Data sources created locally frequently cannot without a network connection or public network IP address. In this case, testing connectivity during the configuration time of the data source fails directly. Data Integration supports [Add task resources](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#) to solve this type of network inaccessibility.

## Create a synchronization task and the synchronization task reader {#section_tfn_1kc_p2b .section}

**Note:** This topic describes the configuration of synchronization tasks in script mode, select **Script Mode** when creating new synchronization tasks in dataset generation.

1.  Enter the [DataWorks management console](https://workbench.data.aliyun.com/console) as a developer, and click **Data Development** in the corresponding project Action bar.
2.  Click **Data Development** in the left-side navigation pane to open the Business Process .

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16217/15516842837629_en-US.png)

3.  Right-click **Business Flow** in the left-side navigation pane to create **Data Integration** \> **Data Sync**, and enter the synchronization Task Name.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16217/15516842837630_en-US.png)

4.  After creating the synchronization node, click the **Switch to Script Mode** in the upper-right corner of the new synchronization node. Select **OK** to enter the Script Mode.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16217/15516842837631_en-US.png)

    **Note:** Script Mode supports more features, such as synchronous task editing if the network is not up-to-date.

5.  Click **Import Template** in the upper-right corner of the script pattern. Select the data source type for read/write respectively in the pop-up window, and then click **OK** to generate the initial script.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16217/15516842837632_en-US.png)


## Configure the synchronization task reader {#section_nz2_jlc_p2b .section}

After creating the synchronization task, the reader basic configurations are generated with the imported template. Now you can manually configure the reader data source and the target table information of the data synchronization task.

```
{"type": "job",
    "version": "2.0",
    "Steps": [// above is configured for the entire synchronization task header code, do not make modifications. The reader configurations are as follows:
        {
            "stepType": "mysql",
            "parameter": {
                "datasource": "MySQL",
                "column": [
                    "id",
                    "value",
                    "table"
                ],
                "socketTimeout": 3600000,
                "connection": [
                    {
                        "datasource": "MySQL",
                        "table": [
                            "`case`"
                        ]
                    }
                ],
                "where": "",
                "splitPk": "",
                "encoding": "UTF-8"
            },
            "name": "Reader",
            "category": "reader" // description classified as reader read end
        }, //The above are reader configurations.
```

Configurations:

-   Type: Specifies the synchronization task for this submission. Only the job parameter is supported, so you can only enter a job.
-   Version: The version number currently supported by all jobs is 1.0 or 2.0.

For more information on configuring the read side for specific parameter settings and code descriptions, see the Script Mode section in [Configuring reader](intl.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Configure DB2 reader.md#).

**Note:** Many tasks require incremental synchronization of data when configuring read data sources, you can now obtain the date in conjunction with what DataWorks provided to complete the requirement [Parameter configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#) to obtain the incremental data.

## Configure the synchronization task writer {#section_xwc_g5c_p2b .section}

You can manually configure the writer data source and the target table information for the data synchronization task after configuring the reader data source.

```
{ //The writer configurations are as follows:
  "stepType": "odps",
  "parameter": {
      "partition": ""
      "truncate": true,
      "compress": false,
      "datasource": "odps_first",
      "column": [
          "*"
       ],
       "emptyAsNull": false,
       "table": ""
     },
     "name": "Writer",
     "category": "writer" // instructions are classified as writer write end
   }
 }, //The above are reader configurations.
```

For more information on configuring the write-side information, see the Script Mode section of [Configuring writer](intl.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure AnalyticDB(ADS) Writer.md#).

**Note:** 

For most tasks, you need to select a Write mode based on data sources, such as overwrite or append mode. If you have Write control requirements, see [Configuring writer](intl.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure AnalyticDB(ADS) Writer.md#) to choose the write mode.

## Configure mapping {#section_h35_qlc_p2b .section}

The script mode only supports in-row mapping, that is, the Reader "columns" correspond to the Writer "columns" sequentially from top-to-bottom.

**Note:** Check if the field types mapped between the columns are data compatible.

## Synchronous task efficiency settings {#section_s3r_zlc_p2b .section}

The efficiency configuration is required when the preceding steps are configured. The **Setting** domain describes the job configuration parameters in addition to the source, destination, and configuration parameters for task global information. Efficiency can be configured in the setting field, including DMU setting, synchronization concurrency setting, synchronization rate setting, dirty data setting, and resource group setting.

```
"setting": {
        "errorLimit": {
            "record": "1024" // dirty data entry settings
        },
        "speed": {
            "throttle": false,  // do you want to limit the speed?
            "concurrent": 1, // synchronous concurrency number settings
            "dmu": 1 // DMU quantity settings
        }
    },
```

Configurations:

-   DMU: The billing unit for data integration.

    **Note:** The configured DMU value limits the maximum concurrency value.

-   When you configure \*\*Synchronization Concurrency\*\*, the data records are separated into several tasks based on the specified reader shard key. These tasks run simultaneously to improve the transmission rate.
-   Synchronous rate: The synchronous rate setting protects the read-side database from fast extraction speed, and reduces pressure on the source library. It is recommended to throttle the synchronization rate and configure the extraction rate properly based on the database source configurations.
-   Dirty data is set to control the synchronized data quality. It supports setting a threshold for dirty data records. If the number of dirty data records exceeds the threshold during job transmission, the job is aborted with an error. For example, the specified maximum error limit is 1024 records in the preceding configuration. When the job dirty data record number is greater than 1024 during the transfer process, an error is reported during exit.
-   You can specify a resource group configuration by clicking **configure task resource groups** in the upper-right corner of the current page.

    When a synchronization task is configured, the resource group in which the task runs is specified. By default, the task runs on the default Resource Group. When the project resource scheduling is tight, you can also expand a resource scheduling by adding a Custom Resource Group. The synchronization task is then specified to run on a Custom Resource Group. For more information on how to add a Custom Resource Group, see [Add task resources](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#) . You can set configurations based on the data source network conditions, project scheduling resource conditions, and business importance.


**Note:** When synchronizing data is inefficient, see [Optimizing configuration](intl.en-US/User Guide/Data integration/Task configuration/Optimizing configuration.md#) to optimize your synchronization tasks.

## Configure scheduling properties {#section_a4t_rnc_p2b .section}

You can set the synchronization task run cycle, run time, task dependency, and more in the scheduling properties. Because the synchronization task starts the ETL job, there are no upstream nodes. We recommend you use the project root node for the upstream configuration at this point.

After completing the synchronization task configuration, save the node and submit.

