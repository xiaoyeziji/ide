# Configure HybridDB for PostgreSQL data source {#concept_z3q_wyb_5fb .concept}

This topic describes the HybridDB and PostgreSQL data source. The HybridDB for PostgreSQL data source allows you to read/write data to HybridDB for PostgreSQL. This topic introduces the detailed steps and related instructions for configuring the HybridDB for PostgreSQL data source.

You can configure synchronization tasks in [Wizard mode configuration](reseller.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Wizard mode configuration.md#) and [Script mode configuration](reseller.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Script mode configuration.md#).

**Note:** If the HybridDB for PostgreSQL is in a VPC environment, you need to note the following issues.

-   On-premise PostgreSQL data source
    -   On-premise PostgreSQL data source test connectivity is not supported, but supports synchronization task configuration. You can click confirm when creating the data source.
    -   You must use a custom scheduled resource group to run the corresponding synchronization tasks, make sure the custom Resource Group can connect to the on-premise database. For more information, see [Data integration when the network of data source \(one side only\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#) and [Data sync when the network of data source \(both sides\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).
-   HybridDB for PostgreSQL data sources created with an instance ID.

    You do not need to select a network environment, the system automatically selects the network based on the HybridDB instance for PostgreSQL information.


## Procedure {#section_uvj_mzr_5fb .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) page as an administrator \(primary account\). Click Enter Workspace in the Actions column of the relevant project in the Project List.

2.  Move mouse to the icon of DataWorks in the upper left corner, select **Data Integration**.
3.  Click New Source in the data source page of the supported data source pop-up window, as shown in the following figure.

    ![](images/32074_en-US.jpeg)

4.  Select **HybridDB for PostgreSQL** as the data source type in the new dialog box.
5.  Configure the individual information items for the HybridDB for PostgreSQL data source. Consider a data source for the **New HybridDB for PostgreSQL** \> **Alibaba Cloud Database \(HybridDB\)** type.

    ![](images/32075_en-US.jpeg)

    Configurations:

    -   Type: Alibaba Cloud HybridDB for MySQL
    -   Name: The data source name can contain letters, numbers, and underscores \(\_\). It cannot start with a number or underscore\(\_\).
    -   Description: A brief description of the data source that cannot exceed 80 characters in length.
    -   Instance ID: You can go to the HybridDB for PostgreSQL console to view the relevant instance ID.
    -   Master account ID: You can view the relevant information in the security settings of the HybridDB for PostgreSQL console.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62183/155143113032076_en-US.png)

6.  Click **Test Connectivity**.
7.  When the test connection is completed, click **OK**.

**Note:** You need to add a whitelist before connecting. For more information, see [Add whitelist](reseller.en-US/User Guide/Data integration/Common configuration/Add whitelist.md#) document.

## The description of test connectivity {#section_w31_g1s_5fb .section}

-   The connectivity test is available in the classic network configuration.
-   The private network can be added by adding the instance ID, and provides the related reverse proxy function.

## Next step {#section_xsp_r1s_5fb .section}

The next topic describes how to configure the PostgreSQL Writer plug‑in. For more information, see [Configure HybridDB for PostgreSQL Reader](reseller.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Configure HybridDB for PostgreSQL Reader.md#) and [Configure HybridDB for PostgreSQL Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure HybridDB for PostgreSQL Writer.md#).

