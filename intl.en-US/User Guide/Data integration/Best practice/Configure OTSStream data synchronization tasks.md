# Configure OTSStream data synchronization tasks {#concept_llg_dpx_pfb .concept}

The OTSStream plugin is used for exporting Table Store incremental data. The incremental data can be considered as operation logs that contain data and operation information.

Different from full export plugins, the incremental export plugin only has multi-version mode that does not allow you to specify columns. This limit is related to how incremental export works. For more information, see [Configure OTSStream Reader](intl.en-US/User Guide/Data integration/Task Configuration/Configure Reader plug-in/Configure OTSStream Reader.md#).

**Note:** When configuring OTSStream data synchronization tasks, note the following:

-   The system can only read the data that is generated five minutes ago but over the past 24 hours.
-   The end time cannot be later than the current system time. Therefore, the end time must be at least five minutes earlier than the task start time.
-   Scheduling a task to run daily may cause data loss.
-   Scheduling periodic and monthly tasks is not supported.

Example:

The start time and the end time must cover the time period for operating Table Store tables. For example, if you insert two data entries to Table Store at 20171019162000, the start time and the end time can be set to 20171019161000 and 20171019162600 respectively.

## Add a data source {#section_zvh_wrx_pfb .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as a project administrator, find the project, and then click **Data Integration**.
2.  Choose **Sync Resources** \> **Data Source** and click **Add Data Source** in the upper-right corner.
3.  Select **Table Store \(OTS\)** as the data source type and set up the configurations in the dialog box that appears.

    |Configuration|Description|
    |:------------|:----------|
    |**Data Source Name**|Can contain letters, numbers, and underscores \(\_\). It must begin with a letter, and cannot exceed 60 characters in length.|
    |**Description**|The description of the data source.|
    |**Endpoint**|The endpoint of the LogHub data source in the format of `http://example.com`.|
    |**Table Store instance ID**|The instance ID corresponding to the Table Store service.|
    |**AccessId/AccessKey**|The logon credential, similar to the account name and the password.|

4.  Click **Test Connectivity**.
5.  When the connection test is passed, click **Complete**.

## Configure a synchronization task in wizard mode {#section_xxh_45x_pfb .section}

1.  Choose **Overview** \> **Tasks** and click **Create Task** in the upper-right corner.
2.  Set up configurations in the Create Node dialog box and click **Submit**. Then, the configuration page of the data synchronization task appears.
3.  Select a data source.

    |Configuration|Description|
    |:------------|:----------|
    |**Data Source**|Select OTS stream and enter the OTS stream data source name.|
    |**Table**|The name of the table from which incremental data is exported. You must enable the Stream feature on the table when creating the table or using the UpdateTable operation after the creation.|
    |**Start Time**|The start time \(included\) in milliseconds of the incremental data. The format is yyyyMMddHHmmss.|
    |**End time**|The end time \(excluded\) in milliseconds of the incremental data. The format is yyyyMMddHHmmss.|
    |**State Table**|The name of the table for recording states.|
    |**Maximum Retries**|The maximum number of retries of each request for reading incremental data from Table Store. The default value is 30.|
    |**Export Sequence Information**|Whether to export time-series information. Time-series information includes the time when data is written.|

4.  Select a destination.

    Select a MaxCompute destination and select a table.

    |Configuration|Description|
    |:------------|:----------|
    |**Data Source**|Select ODPS and enter a destination name.|
    |**Table**|Select the table to be synchronized.|
    |**Partition information**|The table to be synchronized is a non-partitioned table. Therefore, no partition information is displayed.|
    |**Clearance Rule**|     -   **Clear Existing Data Before Writing \(Insert Overwrite\)**: All data in the table or partition is cleaned up before import.
    -   **Retain Existing Data \(Insert Into\)**: No data is cleared before data importing. New data is always appended with each run.
 |
    |**Compression**|The default value is Disable.|
    |**Consider Empty String as Null**|The default value is No.|

5.  Set field mappings.

    Map the fields in source and destination tables. Fields in the source table \(left\) have a one to one correspondence with fields in the destination table.

6.  Configure channel control policies.

    Configure the maximum transmission rate and dirty data check rules.

    |Configuration|Description|
    |:------------|:----------|
    |**DMU**|The billing unit of Data Integration.**Note:** The DMU value limits the maximum number of concurrent jobs. Ensure that DMU is set to an appropriate value.

 |
    |**Number of concurrent jobs**|When you configure Synchronization Concurrency, the data records are split into several tasks based on the specified reader splitting key. These tasks run simultaneously to improve the transmission rate.|
    |**Transmission Rate**|Setting a transmission rate protects the source database from excessive read activity and heavy load. We recommend that you throttle the transmission rate and configure the transmission rate properly based on the source database configurations.|
    |**If there are more than**|The number of dirty data entries. For example, if varchar type data in the source is to be written into a destination column of the int type, a data conversion exception occurs and the data cannot be written into the destination column. You can set an upper limit for the dirty data entries to control the quality of synchronized data. Set an appropriate upper limit based on your business requirements.|
    |**Task's Resource Group**|The resource group used for running the synchronization task. By default, the task runs with the default resource group. When the project has insufficient resources, you can add a custom resource group and run the synchronization task using the custom resource group. For more information about how to add custom resource groups, see [Add scheduling resource](intl.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).Choose an appropriate resource group based on your data source network conditions, project scheduling resources, and business importance.

|

7.  Click **Save** and **Run**.

    Click the **Run** button above the task panel to run the task on the Data Integration page. You need to set the custom parameters before running the task.


## Configure a synchronization task in script code {#section_cbl_n3y_pfb .section}

To configure this task in script mode, click Switch to Script Mode in the toolbar and click **OK**.

Script mode allows you to set up configurations as needed. An example script is as follows.

```
{
  "type": "job",
  "version": "1.0",
  "Configuration ":{
    "reader": {
      "plugin": "otsstream",
      "parameter": {
        "datasource": "otsstream",//Data source name. Use the name of the data resource that you have added.
        "dataTable": "person",//Name of the table from which the incremental data is exported. You must enable the Stream feature on the table when creating the table or using the UpdateTable operation after the creation.
        "startTimeString": "${startTime}",//The start time (included) in milliseconds of the incremental data. The format is yyyyMMddHHmmss.
        "endTimeString": "${endTime}",//The start time (excluded) in milliseconds of the incremental data. The format is yyyyMMddHHmmss.
        "statusTable": "TableStoreStreamReaderStatusTable",//The name of the table for recording the states.
        "maxRetries": 30,//The maximum number of retries of each request.
        "isExportSequenceInfo": false,
      }
    },
    "writer": {
      "plugin": "odps",
      "parameter": {
        "datasource": "odps_first",//Data source name
        "table": "person",//Destination table name
        "truncate": true,
        "partition": "pt=${bdp.system.bizdate}",//Partition information
        "column": [//Destination column name
          "id",
          "colname",
          "version",
          "colvalue",
          "optype",
          "sequenceinfo"
        ]
      }
    },
    "Setting ":{
      "Speed ":{
        "mbps": 7,//Maximum transmission rate
        "concurrent": 7//Number of concurrent jobs
      }
    }
  }
}
```

**Note:** 

-   You can configure the time range of the incremental data using either of the following methods.
    -   `"startTimeString": "${startTime}"`

        The start time \(included\) in milliseconds of the incremental data. The format is yyyyMMddHHmmss.

        `"endTimeString": "${endTime}"`

        The end time \(excluded\) in milliseconds of the incremental data. The format is yyyyMMddHHmmss.

    -   `"startTimestampMillis":""`

        The start time \(included\) in milliseconds of the incremental data.

        The Reader plugin finds a point corresponding to startTimestampMillis from the statusTable, and starts to read and export data from this point.

        If the Reader plugin cannot find the corresponding point, it starts to read incremental data retained by the system from the first entry, and skip the data which is written later than startTimestampMillis.

        `"endTimestampMillis":" "`

        The end time \(included\) in milliseconds of the incremental data.

        The Reader plugin exports data from the startTimestampMillis and ends at the data with the timestamp later than or equal to the endTimestampMillis.

        When the Reader plugin finishes reading all the incremental data, the reading process is ended even if it does not reach the endTimestampMillis.

        This value is a timestamp value, measured in milliseconds.

-   If isExportSequenceInfo is set to true \(“isExportSequenceInfo”: true\), the system exports an extra column for time-series information. The time-series information contains data writing time. The default value of isExportSequenceInfo is false, which means no time-series information is exported.

