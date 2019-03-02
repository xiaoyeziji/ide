# Use Data Integration to ship log data collected by LogHub {#concept_uk4_vgb_pfb .concept}

This topic describes how to use Data Integration to ship data collected by LogHub to supported destinations, such as MaxCompute, Object Storage Service \(OSS\), Table Store, relational database management systems \(RDBMSs\), and DataHub. In this topic, we use MaxCompute as an example.

**Note:** This feature is available in the China \(Beijng\), China \(Shanghai\), China \(Shenzhen\), Hong Kong, US \(Silicon Valley\), Singapore, Germany \(Frankfurt\), Australia \(Sydney\), Malaysia \(Kuala Lumpur\), Japan \(Tokyo\), India \(Mumbai\) regions.

## Scenarios {#section_ogh_b2g_4fb .section}

-   Synchronize data across regions between different types of data sources, such as LogHub and MaxCompute data sources.
-   Synchronize data using different Alibaba Cloud accounts between different types of data sources, such as LogHub and MaxCompute data sources.
-   Synchronize data using one Alibaba Cloud account between different types of data sources, such as LogHub and MaxCompute data sources.
-   Synchronize data using a public cloud account and an Alibaba Finance Cloud account between different types of data sources, such as LogHub and MaxCompute data sources.

## Note on cross-account data synchronization {#section_pvp_22g_4fb .section}

If you want to create a Data Integration task using account B to synchronize LogHub data under account A to MaxCompute data source under account B.

1.  Create a LogHub data source with the Access Id and the Access Key of account A.

    Account B has the permissions to access all Log Service projects created by account A.

2.  Create a LogHub data source with the Access Id and the Access Key of RAM user A1.
    -   Use Alibaba Cloud account A to grant pre-defined Log Service permissions \(`AliyunLogFullAccess` and `AliyunLogReadOnlyAccess`\) to RAM user A1. For more information, see [Grant RAM subaccounts permissions to access Log Service](https://www.alibabacloud.com/help/doc-detail/47664.htm).
    -   Use Alibaba Cloud account A to assign custom Log Service permissions to RAM user A1.

        Choose **RAM console** \> **Policies** and choose **Custom Policy** \> **Create Authorization Policy** \> **Blank Template**.

        For more information about authorization, see [Access control RAM](https://www.alibabacloud.com/help/doc-detail/62663.htm) and [RAM subaccount access](https://www.alibabacloud.com/help/doc-detail/29049.htm).

        If the following policy is applied to RAM user A1, account B can only read project\_name1 and project\_name2 data in Log Service through RAM user A1.

        ```
        {
        "Version": "1",
        "Statement": [
        {
        "Action": [
        "log:Get*",
        "log:List*",
        "log:CreateConsumerGroup",
        "log:UpdateConsumerGroup",
        "log:DeleteConsumerGroup",
        "log:ListConsumerGroup",
        "log:ConsumerGroupUpdateCheckPoint",
        "log:ConsumerGroupHeartBeat",
        "log:GetConsumerGroupCheckPoint"
        ],
        "Resource": [
        "acs:log:*:*:project/project_name1",
        "acs:log:*:*:project/project_name1/*",
        "acs:log:*:*:project/project_name2",
        "acs:log:*:*:project/project_name2/*"
        ],
        "Effect": "Allow"
        }
        ]
        }
        ```


## Add a data source {#section_zyq_xgg_4fb .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as a developer with account B or a RAM user of account B, find the project, and then click **Data Integration**.
2.  Choose **Sync Resources** \> **Data Source** and click **Add Data Source** in the upper-right corner.
3.  Select **LogHub** as the data source type, and then configure the data source in the Add Data Source LogHub dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/155151062133924_en-US.png)

    |Configuration|Description|
    |:------------|:----------|
    |**Data Source Name**|Can contain letters, numbers, and underscores \(\_\). It must begin with a letter, and cannot exceed 60 characters in length.|
    |**Description**|The description of the data source, which must not exceed 80 characters in length.|
    |**LogHub Endpoint**|The endpoint of the LogHub data source in the format of http://example.com.|
    |**Project**| For more information, see [Service endpoints](https://www.alibabacloud.com/help/doc-detail/29008.htm).

 |
    |**Access Id and Access Key**|The logon credential, similar to the account name and the password. You may enter the Access Id and the Access Key of an Alibaba Cloud account or a RAM user account.|

4.  Click **Test Connectivity**.
5.  When the connection test is passed, click **OK**.

## Configure a synchronization task in wizard mode {#section_hph_skg_4fb .section}

1.  Choose **Business Flow** \> **Data Integration** and click **Create Integration Node** in the upper-left corner.
2.  Set up configurations in the Create Node dialog box and click **Submit**. Then, the configuration page of the data synchronization task appears.
3.  Select a source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/155151062114351_en-US.png)

    |Configuration|Description|
    |:------------|:----------|
    |**Data source**|Select LogHub and enter the LogHub data source name.|
    |**Logstore**|The name of the table from which incremental data is exported. You must enable the Stream feature on the table when creating the table or using the UpdateTable operation after the creation.|
    |**Start Time**|The start \(included\) of the selected time range for filtering log entries by log time. The format is yyyyMMddHHmmss. For example, 20180111013000. These parameters correspond to the scheduling time of DataWorks tasks.|
    |**End Time**|The end \(excluded\) of the selected time range for filtering log entries by log time. The format is yyyyMMddHHmmss. For example, 20180111013000. These parameters correspond to the scheduling time of DataWorks tasks.|
    |**Number of Records Read Per Batch**|Number of data entries read each time. The default value is 256.|

    You can click the Data preview button to preview the data .

    **Note:** Data Preview allows you to view a small number of LogHub data entries in a preview box, which may be different from the data that you synchronize. The data that you synchronize is determined by the Start Time and End Time.

4.  Select a destination.

    Select a MaxCompute destination and select a table. In this example, select the ok table.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/155151062114352_en-US.png)

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

    Map the fields in source and destination tables. Fields in the source table \(left\) have a one to one correspondence with fields in the destination table. Select **Enable Same Line Mapping**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/155151062114353_en-US.png)

6.  Configure channel control policies.

    Configure the maximum transmission rate and dirty data check rules.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/155151062114354_en-US.png)

    |Configuration|Description|
    |:------------|:----------|
    |**DMU**|The billing unit of Data Integration.**Note:** The DMU value limits the maximum number of concurrent jobs. Ensure that DMU is set to an appropriate value.

 |
    |**Number of Concurrent Jobs**|When you configure Synchronization Concurrency, the data records are split into several tasks based on the specified reader splitting key. These tasks run simultaneously to improve the transmission rate.|
    |**Transmission Rate**|Setting a transmission rate protects the source database from excessive read activity and heavy load. We recommend that you throttle the transmission rate and configure the transmission rate properly based on the source database configurations.|
    |**If there are more than**|The number of dirty data entries. For example, if varchar type data in the source is to be written into a destination column of the int type, a data conversion exception occurs and the data cannot be written into the destination column. You can set an upper limit for the dirty data entries to control the quality of synchronized data. Set an appropriate upper limit based on your business requirements.|
    |**Task's Resource Group**|The resource group used for running the synchronization task. By default, the task runs with the default resource group. When the project has insufficient resources, you can add a custom resource group and run the synchronization task using the custom resource group. For more information about how to add custom resource groups, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).Choose an appropriate resource group based on your data source network conditions, project resources, and business importance.

|

7.  Run the task.

    You can run the task using either of the following methods:

    -   Directly run the task \(one-time running\).

        Click **Run** in the tool bar to run the task. After setting certain parameters, you can run the task directly on the DataStudio page.

    -   Schedule the task.

        Click **Submit** to submit the synchronization task to the scheduling system. The scheduling system periodically runs the task starting from the next day according to the task configurations.


## Configure a synchronization task in script mode {#section_hcl_h4g_4fb .section}

To configure this task in script mode, click **Switch to Script Mode** in the tool bar and click **OK**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24566/155151062133929_en-US.png)

Script mode allows you to set up configurations as needed. An example script is as follows.

```
{
"type": "job",
"version": "1.0",
"Configuration ":{
"reader": {
"plugin": "loghub",
"parameter": {
"datasource": "loghub_lzz",//Data source name. Use the name of the data resource that you have added.
"logstore": "logstore-ut2",//Source Logstore name. A Logstore is a log data collection, storage, and query unit in LogHub.
"beginDateTime": "${startTime}",//Start (included) time for filtering log entries by log time.
"endDateTime": "${endTime}",//End (included) time for filtering log entries by log time.
"batchSize": 256,//The number of data entries that are read each time. The default value is 256.
"splitPk": "",
"column": [
"key1",
"key2",
"key3"
]
}
},
"writer": {
"plugin": "odps",
"parameter": {
"datasource": "odps_first",//Data source name. Use the name of the data resource that you have added.
"table": "ok",//Destination table name
"truncate": true,
"partition": "",//Partition information
"column": [//Destination column name
"key1",
"key2",
"key3"
]
}
},
"Setting ":{
"Speed ":{
"mbps": 8,//Maximum transmission rate
"concurrent": 7//Number of concurrent jobs
}
}
}
}
```

