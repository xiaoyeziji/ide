# Configure HybridDB for MySQL data source {#concept_rdt_syb_5fb .concept}

This topic describes detailed steps and related instructions for configuring the HybridDB for MySQL data source. The HybridDB for MySQL data source allows you to read/write data to HybridDB for MySQL.

You can configure synchronization tasks in [Wizard mode](reseller.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Wizard mode configuration.md#) and [Script mode](reseller.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Script mode configuration.md#).

**Note:** If the HybridDB for MySQL is in a VPC environment, you need to note the following issues:

-   On-premise MySQL data source
    -   Test connectivity is not supported, but supports synchronization task configuration. You can click **OK** when creating the data source.
    -   You must use a custom scheduled resource group to run the corresponding synchronization tasks, make sure the custom resource group can connect to the on-premise database. For more information, see [Data sync when one-end of the data source network is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#) and [Data synchronization when both-end of the data source network is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).
-   For the HybridDB of MySQL data sources created with an instance ID, you do not need to select a network environment, and the system automatically determines the network environment based on the information you entered for the HybridDB for MySQL instance.

## Procedure {#section_rgw_ftr_5fb .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) page using the administrator \(primary account\). Click **Enter Workspace** in the Actions column of the relevant project in the Project List.

2.  Move mouse to the icon of DataWorks in the upper left corner, select**Data Integration**.
3.  Click **New Source** in the data source page. Go to the supported data source pop-up window, as shown in the following figure.

    ![](images/32067_en-US.jpeg)

4.  Select **Alibaba Cloud Data Source \(HybridDB\)** as the data source type in the new dialog box.
5.  Configure the individual information items for HybridDB of the MySQL data source.

    ![](images/32068_en-US.jpeg)

    Configurations:

    -   Type: The currently selected data source type is the HybridDB for MySQL.
    -   Name: The name can contain letters, numbers, and underscores \(\_\). It cannot start with a number or underscore \(\_\).
    -   Description: A brief description of the data source that cannot exceed 80 characters in length.
    -   Instance ID: You can go to the HybridDB for MySQL console to view the related instance ID.
    -   Master account ID: You can view the related information in the security settings of the HybridDB for MySQL console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62182/155142298132071_en-US.png)

    -   Username and password: The user name and password used for database connection.
6.  Click **Test Connectivity**.
7.  When the test connection is completed, click **Complete**.

**Note:** You need to add a whitelist before connecting. For more information, see [Add whitelist](reseller.en-US/User Guide/Data integration/Common configuration/Add whitelist.md#) document.

## The description of test connectivity {#section_l5s_ctr_5fb .section}

-   The connectivity test is available in the classic network configuration.
-   The private network can be added successfully in the form of adding an instance ID, and providing related reverse proxy function.

## Next step {#section_utj_syr_5fb .section}

The next topic describes how to configure the HybridDB for MySQL Writer plug‑in. For more information, see [Configure HybridDB for MySQL Reader](reseller.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Configure HybridDB for MySQL Reader.md#) and [Configure HybridDB for MySQL Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure HybridDB for MySQL Writer.md#).

