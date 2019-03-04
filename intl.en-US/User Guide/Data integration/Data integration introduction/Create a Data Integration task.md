# Create a Data Integration task {#concept_lnl_13d_2gb .concept}

This topic describes how to create a Data Integration task.

-   Data Integration is a reliable, secure, cost-effective, and elastically scalable data synchronization platform provided by Alibaba Group. It can be used across heterogeneous data storage systems and provides full or incremental data access channels in different network environments for a variety of data sources.
-   A **reader plug-in** reads data from a database at the underlying layer by connecting to a remote database and running SQL statements to select data from the database.
-   A **writer plug-in** writes data into a database at the underlying layer by connecting to a remote database and running SQL statements to write data into the database.

## Preparations {#section_bdb_qjd_2gb .section}

**Create an Alibaba Cloud account**

1.  Activate an [Alibaba Cloud account](../../../../../intl.en-US/Preparation/Administrator Operations/Alibaba Cloud Account Preparations.md#), and create the AccessKeys for this account.
2.  Activate MaxCompute to automatically generate a default MaxCompute data source, and log on to DataWorks using the Alibaba Cloud account.
3.  [Create a workspace](../../../../../intl.en-US/Preparation/Administrator Operations/Create Workspace.md#). You can collaboratively complete workflows and maintain data or tasks in the workspace. Before using DataWorks, you need to create a workspace.

**Note:** You can grant RAM accounts the permissions to create Data Integration tasks. For more information, see [Create a RAM account](../../../../../intl.en-US/Preparation/Administrator Operations/Create a RAM user.md#).

**Create source and destination databases and tables**

1.  You can create tables by **running statements** or create tables directly **in the data source client**. For more information about how to create databases and tables of different data source types, see their official documents.
2.  Grant read and write permissions on the databases and tables.

**Note:** Generally, a reader plug-in requires at least the **read** permission, while a writer plug-in requires the **add, delete, and modify** permissions. We recommend that you grant sufficient permissions on tables of databases in advance.

## Procedure {#section_p3h_xkd_2gb .section}

**Create a data source**

1.  Obtain [data source](intl.en-US/User Guide/Data integration/Data source configuration/Supported data sources.md#) information about a database.
2.  Configure the data source on the GUI.

**Note:** 

-   Not all data sources can be configured on the GUI. If you cannot find the configuration page for a data source, you can configure it in script mode by writing data source information in a JSON script.
-   For more information about the data sources that are supported, see [Supported data sources](intl.en-US/User Guide/Data integration/Data source configuration/Supported data sources.md#).

**\(Optional\) Create a custom resource group**

1.  Create a [resource group](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).
2.  Add a server.
3.  Install the agent.
4.  Test the connectivity.

**Note:** 

-   If the data source is located in a private network environment or the resources provided by DataWorks do not meet your requirements, you can create a custom resource group.
-   We recommend that you set the network type of the custom resource group to VPC regardless whether the server is in a classic network or VPC.
-   For more information about how to configure a custom resource group, see [Add scheduling resources](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).
-   Best practices:
    -   [Data synchronization when either the source or destination is located in a private network environment](intl.en-US/User Guide/Data integration/Best practice/Data Integration when one side of the data source is disconnected.md#)
    -   [Data synchronization when both the source and destination are located in a private network environment](intl.en-US/User Guide/Data integration/Best practice/Data sync when both ends of the data source network is disconnected.md#)

**Configure a Data Integration task**

1.  Configure the reader of the Data Integration task. For more information about how to configure a reader, see [Configure a reader plug-in](https://www.alibabacloud.com/help/faq-list/72788.htm).
2.  Configure the writer of the Data Integration task. For more information about how to configure a writer, see [Configure a writer plug-in](https://www.alibabacloud.com/help/faq-list/74301.htm).
3.  Configure the mapping between the reader and writer.
4.  Configure channel control. You can switch to a custom [resource group](intl.en-US/User Guide/Data integration/Common configuration/Add task resources.md#) in this step.

**Note:** 

-   A task can be configured in [wizard](intl.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Wizard mode configuration.md#) or [script](intl.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Script mode configuration.md#) mode.
-   When configuring a task, you can optimize the task speed. For more information, see [Optimizing configuration](intl.en-US/User Guide/Data integration/Task configuration/Optimizing configuration.md#).
-   You can switch from the wizard mode to script mode, but not from the script mode to wizard mode. We have provided templates for all plug-ins.

**Run the Data Integration task**

1.  You can run the Data Integration task directly on the GUI. Logs will not be saved.
2.  Before submitting the task, you need to [configure scheduling](intl.en-US/User Guide/Data development/Scheduling Configuration/Dependencies.md#). Generally, an instance is generated on the next day after submission.

**Note:** When configuring the task, you can set [scheduling parameters](intl.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#).

**View run logs**

You can view run logs of your task in [O&M](intl.en-US/User Guide/O&M Center/O&M center overview.md#).

**Note:** 

You can find the DAG in [O&M](intl.en-US/User Guide/O&M Center/O&M center overview.md#), right-click the DAG, and select**Run Log** to view the run logs.

