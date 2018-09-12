# Script Mode Configuration {#concept_olb_drc_p2b .concept}

This article will show you how to configure tasks through the data integration Script Mode.

The steps for task configuration are as follows:

1.  Create data source
2.  Create a synchronization task
3.  Import a template.
4.  Configure the synchronization task reader.
5.  Configure the synchronization task writer.
6.  Configure the mapping between the synchronization task reader and the synchronization task writer.
7.  Configure the DMUs, concurrency, transmission rate, dirty data records, resource groups, and other information of synchronization tasks.
8.  Configure the scheduling attribute of the synchronization task.

**Note:** You will be introduced below to the specific implementation of the operation steps, each of the following steps jumps to the corresponding guidance document, after completing the current step, click the link back to this article to continue to the next step.

## Create data source {#section_vkd_w2c_p2b .section}

Synchronization tasks support data transmission between various homogenous data sources and heterogeneous data sources. First, register the target data source at Data Integration. Then you can select the data source directly when configuring a synchronization task on Data Integration. Data integration the data source types that support synchronization are shown in [Supported data sources](intl.en-US/User Guide/Data Integration/Data source configuration/Supported data sources.md#).

After confirming that the target data source is supported by Data Integration, you can register the data source at Data Integration. For detailed data source registration, see [configuring data source information](intl.en-US/User Guide/Data Integration/Data source configuration/Supported data sources.md#).

**Note:** 

-   Some data source data integration does not support test connectivity. For more information on data source test connectivity, see[Data Source testing connectivity](intl.en-US/User Guide/Data Integration/Data source configuration/Data Source testing connectivity.md#).
-   Many times, data sources are created locally and cannot be connected without a public network IP or network. In this case, testing connectivity at the time of configuration of the data source fails directly, data Integration supports adding[Concepts](../../../../intl.en-US/Product Introduction/Concepts.md#) to address this type of network inaccessibility, however, when creating a new synchronization task, you can only select Script Mode \(because the network is not connected, information such as table structure cannot be obtained in wizard mode \).

## Create a synchronization task and the synchronization task reader {#section_tfn_1kc_p2b .section}

**Note:** This article mainly introduces you to the configuration of synchronization tasks in script mode, select **Script Mode** when creating new synchronization tasks in dataset generation.

1.  Enter the [DataWorks management console](https://workbench.data.aliyun.com/console) as a developer, and click **Enter workspace** in the corresponding project action bar.
2.  Click **Data Development** in the left-hand menu bar to open the Business Process navigator.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16217/15367411267629_en-US.png)

3.  Right-click **Business Flow** in the navigation bar, create **Data Integration** \> **Data Sync**, and enter the synchronization Task Name.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16217/15367411267630_en-US.png)

4.  After successfully creating the synchronization node, click the **Switch to Script Mode** in the upper-right corner of the new synchronization node, select **Ok** to enter Script Mode.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16217/15367411267631_en-US.png)

    **Note:** Script Mode supports more features, such as synchronous task editing if the network is not up to date.

5.  Click **Import template** in the upper-right corner of the script pattern, in the bullet box, select the source type of the read and the data source, the target type of the write, and the data source respectively, click **confirm** to generate the initial script.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16217/15367411267632_en-US.png)


## Configure the synchronization task reader {#section_nz2_jlc_p2b .section}

After the synchronization task is created, basic configurations of the reader are generated with the imported template. Now you can manually configure the reader data source and the target table information for the data synchronization task.

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

-   Type: Specifies the synchronization task for this submission, only the job parameter is supported, so you can only fill in as a job.
-   Version: the version number currently supported by all jobs is 1.0 or 2.0.

When you configure the read side, for specific parameter settings and code descriptions, see the Script Mode section in [configuring reader](intl.en-US/User Guide/Data Integration/Task Configuration/Configure Reader plug-in/Configure DB2 Reader.md#).

**Note:** Many tasks require incremental synchronization of data when configuring read-side data sources, you can now get the date in conjunction with what dataworks provides to complete the requirement [Parameter configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#) to get the incremental data.

## Configure the synchronization task writer {#section_xwc_g5c_p2b .section}

After the reader data source is configured, you can manually configure the writer data source and the target table information for the data synchronization task.

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

When you configure write-side information, see the Script Mode section of [configuring writer](intl.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure AnalyticDB(ADS) writer.md#).

**Note:** 

For most tasks, you need to select a write mode based on data sources, such as overwrite mode or append mode. If you have write control requirements, see [configuring writer](intl.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure AnalyticDB(ADS) writer.md#) to choose write mode properly.

## Configure mapping {#section_h35_qlc_p2b .section}

The script mode only supports in-row mapping, that is, the reader "columns" correspond to the writer "columns" one by one from top to bottom.

**Note:** Note whether the field types mapped between columns are data compatible.

## Synchronous task efficiency settings {#section_s3r_zlc_p2b .section}

When the above steps are configured, the efficiency configuration is required. The **setting** domain describes the job configuration parameters in addition to the source and destination, configuration parameters for job global information. Efficiency can be configured in the setting field, including DUM setting, synchronization concurrency setting, synchronization rate setting, dirty data setting, and resource group setting.

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

-   DMU: the unit of charge for data integration.

    **Note:** When setting up a DMU, be aware that the value of the DMU limits the value of the maximum number of concurrency, please configure properly.

-   When you configure \*\*Synchronization Concurrency\*\*, the data records are split into several tasks based on the specified reader splitting key. These tasks run simultaneously to improve the transmission rate.
-   Synchronous rate: Setting the synchronous rate protects the read-side database from excessive extraction speed, put too much pressure on the source library. It is recommended to throttle the synchronization rate and configure the extraction rate properly based on source database configurations.
-   Dirty Data is mainly set to control the quality of synchronized data. It supports setting a threshold of dirty data records. If the number of dirty data records exceeds the threshold during job transmission, the job is aborted with an error. For example, in the configuration above, you specified a maximum error limit of 1024 records, when the job has a dirty data record number greater than 1024 during the transfer process, the job reports an error to exit.
-   You can specify a resource group configuration by clicking **configure task resource groups** in the upper-right corner of the current page.

    When you configure a synchronization task, you specify the resource group in which the task runs, default runs on the default Resource Group. When the project has a tight schedule of resources, you can also expand a scheduled resource by adding a Custom Resource Group, the synchronization task is then specified to run on a Custom Resource Group, to add a Custom Resource Group, see adding a scheduled resource. You can make a reasonable configuration based on data source network conditions, project scheduling resource conditions, and business importance.


**Note:** When synchronizing data is not efficient, see [Optimizing configurations](intl.en-US/User Guide/Data Integration/Task Configuration/Optimizing configuration.md#)optimizing your synchronization tasks.

## Configure scheduling properties {#section_a4t_rnc_p2b .section}

In the scheduling properties, you can set the synchronization task run cycle, run time, task dependency, and so on. Since the synchronization task is the beginning of the ETL job, there are no upstream nodes, at this point, it is recommended to use the project root node as upstream.

After completing the configuration of the synchronization task, save the node and submit.

