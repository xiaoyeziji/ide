# Metadata Collection {#concept_mrm_2wg_mfb .concept}

This article will show you how to perform metadata collection.

## Add datasources {#section_f5s_4bh_mfb .section}

**Note:** 

-   Metadata collection only supports source database type **No public network IP**.
-   Due to network restriction, data sources with No public network IP need to run on Customized Resource Groups to pull table and column information, for more information, please refer [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).

1.  Log into [DataWorks Management Console](https://partners-intl.aliyun.com) as project administrator, click **Enter Project** in corresponding project operation bar.
2.  Click **Data Integration** in the top navigation bar, Select **Sync Resource** \> **Data Source**.
3.  Click **Add Data Source**, Select data source type **MySQL**.
4.  Select data source type as **Has No public network IP** in Add Data Source MySQL Dialog Box.

    |Configuration|Instructions|
    |:------------|:-----------|
    |**Data Source Type**|Without public network IP.|
    |**Data Source Name**|Data Source Name can contain letters, numbers, and underscores \(\_\). It must begin with a letter, and cannot exceed 60 characters.|
    |**Description**|The description of the data source, which must not exceed 80 characters.|
    |**Resource Group**|Resource Group: It is used to run synchronization tasks, and generally multiple machines can be selected when you add a resource group. For more information, please refer [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).|
    |**JDBC URL**|JDBC URL: JDBC connection information and its format is`jdbc://mysql://serverIP:Port/Database`.|
    |**User Name**|User name for corresponding database.|
    |**Password**|The password for corresponding database.|

5.  Click **Finish**.

## Create a collection task {#section_abj_pbh_mfb .section}

1.  Click **Add a Collection task** after corresponding source data.
2.  Complete relevant configuration information in Add a Collection taskdialog box.

    **Note:** If the source group is available, the name of source group, which source data belongs to, will display by default.

    |Configuration|Instructions|
    |:------------|:-----------|
    |**Data source to be collected**|The data source has already been added with collection point which cannot be added again, and there would be prompts for related action. The options in the drop-down box are the Has no public network IP data sources that you added.|
    |**Resource Group**|Automatically display the resource groups name you selected when adding source data. For more information please refer [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).|
    |**Table to be collected**|Including **All Tables** and **Specify tables**, the default selection is **All Tables**.Edit box will pop-up after selecting the specific table. Multiple tables entry is supported, separated by comma\(,\).

|
    |**Collection time**|You can choose any exactly hour as collection starting timing, from 00 to 23.|

3.  Click **Confirm**。

Successfully created collection tasks are displayed in collection task list, this list is mainly used for checking database tables and columns information, you can search related collection list by data source name and owner.

|Configuration|Instructions|
|:------------|:-----------|
|**Data Source Name**|Data Source Name need to be consistent with name of the newly added data source.|
|**Node ID**|Each task node ID needs to be unique.|
|**Collected Tables**|Normally, Node name contains 2 parts, which are automatically generated as \`data source name\_table\` and \`data source name\_colunm\` respectively.|
|**Owner**|Owner by default is the user whom created collection task.|
|**Status**|Generally , there are 4 kinds of collection status which are collection failure, collection success, waiting for scheduling, and collecting.|
|**Collection time**|The timing which metadata is triggered regularly per day for information collection.|
|**Start Time**|Generally ,the default format is `yyyy-mm-dd hh:mm:ss`.|
|**End time**|Generally ,the default format is `yyyy-mm-dd hh:mm:ss`.|
|**Action**|The action of task collection can be divided into two parts.-   DataSource actions
    -   **Modify timing**: Modify the trigger timing of the current data source collection task.
    -   **Delete**: Delete current data source collection task.
-   Node operation
    -   **Collect now**: Immediately trigger the collection task, update relevant datasource table names and field names.
    -   **Schedule Operations**: Navigate into **Operation Center** \> **Cycle instance** Page by clicking this button, relevant cycle instance would display filtered by specific conditions.
    -   **Latest logs**: View the corresponding process log.

|

## Configure a synchronization task {#section_gmt_2jh_mfb .section}

When meta collection completed, you can configure corresponding tasks through wizard mode. For more details, please refer [Creating a Synchronization Task](../../../../../reseller.en-US/Quick Start/Step 3: Create a synchronization task.md#).

**Note:** 

-   Wizard mode can assist for Synchronization Task configuration, but the task is still running under Custom Resource Group. There may be network connection issue if you choose to run under Default Resource Group instead.
-   The relevant table and column information had already stored in the metadata service, so there would not be table information and column information collecting problems cause by network connection issue.

When the synchronization task configuration completed, click **Run**, the task runs immediately. Alternatively, click **Submit** to submit the synchronization task to the scheduling system. The scheduling system periodically runs the task starting from the next day according to the task configurations.

