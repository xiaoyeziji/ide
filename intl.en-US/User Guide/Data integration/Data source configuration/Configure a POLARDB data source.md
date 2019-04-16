# Configure a POLARDB data source {#concept_imb_1zb_5fb .concept}

A POLARDB data source is a relational database, which allows you to read data from and write data into it. This section describes how to configure a POLARDB data source.

You can configure synchronization tasks in wizard mode or script mode. For more information, see [Wizard mode configuration](intl.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Wizard mode configuration.md#) and [Script mode configuration](intl.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Script mode configuration.md#).

**Note:** 

Currently, POLARDB data sources do not support custom resource groups. Use the default resource group. If you need to use custom resource groups, add a MySQL data source and set the data source type to **Public IP Address Unavailable**. For more information, see [Configure MySQL data source](intl.en-US/User Guide/Data integration/Data source configuration/Configure MySQL data source.md#).

If your POLARDB data source is located in a VPC, pay attention to the following:

-   For a user-created POLARDB data source
    -   Connectivity testing is not supported, but you can still configure synchronization tasks. You can ignore the Test Connectivity button, and click Complete when you create the data source.
    -   You must use a custom resource group to run the corresponding synchronization tasks. Make sure that the custom resource group can connect to the database that you have created. For more information, see [Data Integration when one side of the data source is disconnected](intl.en-US/User Guide/Data integration/Best practice/Data Integration when one side of the data source is disconnected.md#) and [Data sync when both ends of the data source network is disconnected](intl.en-US/User Guide/Data integration/Best practice/Data sync when both ends of the data source network is disconnected.md#).
-   For a POLARDB data source created with the ID of a POLARDB instance

    You do not need to select the network environment, because the system automatically determines the network environment based on the information you enter for the POLARDB instance.


## Procedure {#section_bzw_mfs_5fb .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator \(the primary account\). In the Workspaces area of the Overview page, click Data Analytics in the Actions column of a workspace.
2.  Move your pointer over the DataWorks icon in the upper-left corner, and select **Data Integration**.
3.  Click Add Data Source on the Data Source page. A dialog box appears, listing the supported data source types, as shown in the following figure.

    ![](images/32085_en-US.jpeg)

4.  In the Add Data Source dialog box, select **POLARDB** as the data source type.
5.  Set the parameters required for creating a POLARDB data source.

    ![](images/32086_en-US.jpeg)

    The parameters are described as follows:

    -   Data Source Type: The data source type. In this case, POLARDB is selected.
    -   Data Source Name: The data source name. The value must contain letters, digits, and underscores \(\_\), but it must not start with a digit or underscore \(\_\).
    -   Description: A brief description of the data source. The value can contain a maximum of 80 characters in length.
    -   Cluster ID: You can view the cluster ID in the POLARDB console.
    -   Polardb instance main account ID: You can view the primary account ID of the POLARDB instance on the Security Settings page of the POLARDB console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62183/155176859732076_en-US.png)

    -   Database Name: The name of the database created in the POLARDB instance.
    -   Username and Password: The username and password used to connect to the database.
6.  Click **Test Connectivity**.
7.  After the connectivity test is passed, click **Complete**.

**Note:** The connectivity test can be passed only after the data source is added to the whitelist. For more information, see [Add a whitelist](intl.en-US/User Guide/Data integration/Common configuration/Add whitelist.md#).

## Connectivity test description {#section_i34_pgs_5fb .section}

-   The connectivity test is available in a classic network environment.
-   In a VPC, you can add the data source successfully by adding the instance ID. In this case, the reverse proxy feature is provided to ensure that the data source can be connected.

## Next step {#section_zj4_qgs_5fb .section}

Now you have learned how to configure the POLARDB data source. For more information about how to configure the POLARDB reader and writer plug-ins, see [Configure POLARDB Reader](intl.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Configure POLARDB Reader.md#) and [Configure POLARDB Writer](intl.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure POLARDB Writer.md#).

