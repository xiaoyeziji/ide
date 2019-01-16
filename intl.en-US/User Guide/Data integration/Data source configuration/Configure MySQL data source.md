# Configure MySQL data source {#concept_nvl_451_p2b .concept}

The MySQL data source allows you to read data from and write data to MySQL, and supports configuring synchronization tasks in wizard mode and script mode.

**Note:** If you are using MySQL in a VPC environment, you need to be aware of the following issues.

-   Self-built MySQL Data Source
    -   Test connectivity is not supported, but the configuration synchronization task is still supported, and you can click **confirm** when creating the data source.
    -   You must use a custom scheduled Resource Group to run the corresponding synchronization tasks, make sure that the Custom Resource Group can connect to your self-built database. For more information, see[Data integration when the network of data source \(one side only\) is disconnected](intl.en-US/User Guide/Data integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#) and[Data sync when the network of data source \(both sides\) is disconnected](intl.en-US/User Guide/Data integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).
-   MySQL data sources created with RDS

    You do not need to select a network environment, and the system automatically determines based on the information you fill in for the RDS instance.


## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
3.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
4.  Click **Add new data source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15476188987549_en-US.png)

5.  In the new data source dialog box, select the data source type as **MySQL**.
6.  Configure individual information items for the MySQL data source.

    MySQL Data source types are divided into the **Ali cloud database \(RDS\)**, the **public network IP** and the **non-public network IP**.

    Consider a data source of the new **MySQL** \> **Ali cloud database \(RDS\)** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15476188987550_en-US.png)

    Configurations:

    -   Type: the currently selected data source type mysql \> Ali cloud database \(RDS \).
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Instance ID of RDS: You can go to the RDS console to view the instance ID of the RDS.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15476188987551_en-US.png)

    -   RDS instance buyer ID: You can view the information in the RDS console security settings.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15476188987553_en-US.png)

    -   User name/Password: The user name and password used to connect to the database.
    **Note:** Before you can connect successfully, you need to add an RDS white list. For more information, see[Add whitelist](intl.en-US/User Guide/Data integration/Common configuration/Add whitelist.md#).

    Consider a data source of the new **MySQL** \> **public network IP** type as an example.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15476188987554_en-US.png)

    Configurations:

    -   =Type: With a public IP address.
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   JDBC URL: In the format of jdbc://mysql://serverIP:Port/database.
    -   User =name/Password: The user name and password used to connect to the database.
    Consider a data source with a new **MySQL** \> **non-public network IP** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15476188987555_en-US.png)

    Configurations:

    -   Data source type: data source without a public IP address.
    -   Data source name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Data source description: It is a brief description of the data source with no more than 80 characters.
    -   Resource Group: It is used to run synchronization tasks, and generally multiple machines can be bound when you add a resource group. For more information, see [Add scheduling resources](intl.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).
    -   JDBC URL: JDBC URL, in the format of jdbc://mysql://serverIP:Port/Database.
    -   User name/Password: The user name and password used to connect to the database.
7.  Click **Test Connectivity**.
8.  When the connectivity test is passed, click **OK**.

## Connectivity test description {#section_eq3_lnb_p2b .section}

-   The connectivity test is available in the classic network arrangement, to identify whether the input JDBC URL, user name, and password are correct.
-   Private Network and no public network IP, data source connectivity test is currently not supported, click **confirm**.

## Next step {#section_bbg_jnb_p2b .section}

Now you have learned how to configure the MySQL data source. The document explains how to configure the MySQL Writer plug‑in later. For more information, see [Configure MySQL Writer](intl.en-US/User Guide/Data integration/Task Configuration/Configure Writer plug-in/Configure MySQL Writer.md#).

