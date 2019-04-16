# Metadata collection {#concept_mrm_2wg_mfb .concept}

This topic describes how to perform metadata collection.

## Add datasources {#section_f5s_4bh_mfb .section}

**Note:** 

-   Metadata collection only supports source database type **No Public Network IP Address**.
-   Due to network restrictions, the data sources with no public network IP addresses need to run on Customized Resource Groups to pull table and column information. For more information, see[Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

1.  Log on to [DataWorks Management Console](https://partners-intl.aliyun.com) as the project administrator, and click **Enter Project** in the corresponding project operation menu.
2.  Click **Data Integration** in the top navigation bar, and select **Sync Resource** \> **Data Source**.
3.  Click **Add Data Source**, and select data source type **MySQL**.
4.  Select data source type as **No Public Network IP Address** in Add Data Source MySQL Dialog Box.

    |Configuration|Description|
    |:------------|:----------|
    |**Data source type**|The data source type with no public network IP address.|
    |**Data source name**|The data source name can contain letters, numbers, and underscores \(\_\). It must start with a letter, and cannot exceed 60 characters in length.|
    |**Description**|The description of the data source, which must not exceed 80 characters in length.|
    |**Resource group**|The group used to run synchronization tasks. Generally, you can select multiple machines when adding a resource group. For more information, see[Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).|
    |**JDBC URL**|The JDBC URL. The JDBC connection information and format is`jdbc://mysql://serverIP:Port/Database`.|
    |**User name**|The user name for corresponding database.|
    |**Password**|The password for corresponding database.|

5.  Click **Finish**.

## Create a collection task {#section_abj_pbh_mfb .section}

1.  Click **Add A Collection Task** after selecting the corresponding data source.
2.  Complete relevant configuration information in Add A Collection Taskdialog box.

    **Note:** If the resource group is available, the resource group name of the added data is displayed by default.

    |Configuration|Description|
    |:------------|:----------|
    |**Collected data source**|You cannot add another collection point to a data source with an existing collection point. If you try to add another collection point, the system will display a prompt. t The options in the drop-down menu is the data source you added that have No Public Network IP addresses.|
    |**Resource group**|Automatically displays the selected resource group name when adding the data source. For more information, see [Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).|
    |**Collected table**|The tables collected methods include **All Tables** and **Specify Tables**. By default, **All Tables**are selected.After selecting the table collection method, an Edit pop-up window is displayed . The table selection supports multiple table entries, separate each table entry with commas \(,\).

|
    |**Collection time**|You can choose a collection time for each day starting from 00:00 to 23:00.|

3.  Click **Confirm**.

Successfully create collection tasks displayed in the collection task list, this list is mainly used for checking database tables and column information. You can search the related collection list by data source name and owner.

|Configuration|Descriptions|
|:------------|:-----------|
|**Data source name**|The data source name must be the same as the name of the newly added data source.|
|**Node ID**|Each task node ID must be unique.|
|**Collected tables**|Generally, the Node name comprises two parts, which are automatically generated as the data source name\_table and data source name\_column, respectively.|
|**Owner**|By default, the owner is the user who created the collection task.|
|**Status**|Generally, there are four types of collection status: failure, collection successful, pending scheduling, and collecting.|
|**Collection time**|The timing in which metadata is triggered regularly each day for information collection.|
|**Start Time**|Generally, the default format is `yyyy-mm-dd hh:mm:ss`.|
|**End time**|Generally, the default format is `yyyy-mm-dd hh:mm:ss`.|
|**Action**|The task collection Action can be separated into two parts.-   DataSource actions
    -   **Modify Timing**: Modify the trigger timing of the current data source collection task.
    -   **Delete**: Delete the current data source collection task.
-   Node operation
    -   **Collect Now**: Immediately triggers the collection task, and updates the relevant datasource table names and field names.
    -   **Schedule Operations**: Go to **Operation Center** \> **Cycle Instance**page by clicking this button, the relevant cycle instance will display specific conditions by filter.
    -   **Latest Logs**: View the corresponding run log.

|

## Configure a synchronization task {#section_gmt_2jh_mfb .section}

When the metadata collection is completed, you can configure the corresponding tasks through Guide Mode. For more information, see [Creating a Synchronization Task](../../../../../reseller.en-US/Quick Start/Step 3: Create a synchronization task.md#).

**Note:** 

-   The Guide Mode can assist the Synchronization Task configuration, but the task is still running under the Custom Resource Group. There may be network connection issues, if you choose to run under the Default Resource Group instead.
-   The relevant table and column information is stored in the metadata service, so table and column information collection will not be affected by network connection issues.

After completing the synchronization task configuration, click **Run**to immediately run tasks. You can also click **Submit** to submit the synchronization task to the scheduling system. The scheduling system periodically runs the task starting from the next day based on the task configurations.

