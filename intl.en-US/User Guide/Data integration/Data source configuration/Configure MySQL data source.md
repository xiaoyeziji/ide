# Configure MySQL data source {#concept_nvl_451_p2b .concept}

This topic describes how to configure the MySQL data source. The MySQL data source allows you to read /write data on MySQL, and supports configuring synchronization tasks in wizard and script mode.

**Note:** If you are using MySQL in a VPC environment, you need to be aware of the following issues.

-   On-premise MySQL data source
    -   Does not support test connectivity, but supports synchronization task configuration. You can configure synchronization task by clicking **Confirm** when creating the data source.
    -   You must use a custom scheduled Resource Group to run the corresponding synchronization tasks, make sure the Custom Resource Group can connect to the on-premise database. For more information, see[Data integration when the network of data source \(one side only\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#) and[Data sync when the network of data source \(both sides\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).
-   MySQL data sources created with RDS

    You do not need to select a network environment, the system will automatically determine the network environment based on information entered for the RDS instance.


## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **Add Data Source** in the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15514228317549_en-US.png)

4.  Select the data source type **MySQL** in the new dialog box.
5.  Complete the MySQL data source information items configuration.

    MySQL Data source types are divided in the **Alibaba Cloud Database \(RDS\)**, the **Public Network IP Address** and the **Non-Public Network IP Address**.

    Consider a data source for the new **MySQL** \> **Alibaba Cloud Database \(RDS\)** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15514228317550_en-US.png)

    Configurations:

    -   Type: Currently, the selected data source type MySQL \> Alibaba Cloud Database \(RDS\).
    -   Name: A name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   RDS Instance ID: You can go to the RDS console to view the RDS instance ID.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15514228317551_en-US.png)

    -   RDS instance buyer ID: You can view information in the RDS console security settings.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15514228317553_en-US.png)

    -   Username and password: The user name and password used to connect to the database.
    **Note:** You need to add an RDS whitelist before connection. For more information, see[Add whitelist](reseller.en-US/User Guide/Data integration/Common configuration/Add whitelist.md#).

    Consider a data source for the new **MySQL** \> **Public Network IP Address** type as an example.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15514228317554_en-US.png)

    Configurations:

    -   Type: A new MySQL data source with a public IP address.
    -   Name: A name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It must contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   JDBC URL: The format is jdbc://mysql://serverIP:Port/database.
    -   Username and password: The user name and password used for connecting to the database.
    For example, a data source with a new **MySQL** \> **Non-Public Network IP Address** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15514228317555_en-US.png)

    Configurations:

    -   Data source type: The data source without a public IP address.
    -   Data source name: A data source name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It must contain letters, numbers, and underscores \(\_\).
    -   Data source description: A brief description of the data source that does not exceed 80 characters in length.
    -   Resource group: A group used to run synchronization tasks, and generally multiple machines can be bound when you add a resource group. For more information, see [Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).
    -   JDBC URL: The format is jdbc://mysql://serverIP:Port/Database.
    -   Username and password: The user name and password used for connecting the database.
6.  Click **Test Connectivity**.
7.  Click **OK** after the connectivity has passed the test.

## Connectivity test description {#section_eq3_lnb_p2b .section}

-   The connectivity test is available in the classic network environment for verifying whether the entered JDBC URL, user name, and password are valid.
-   Currently, the private network and no public network IP address, data source connectivity test is not supported. Click **OK**.

## Next step {#section_bbg_jnb_p2b .section}

For more information on how to configure the MySQL Writer plug-in, see [Configure MySQL Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure MySQL Writer.md#).

