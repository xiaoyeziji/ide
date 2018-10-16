# Configuring SQL server data source {#concept_o4y_5yb_p2b .concept}

The SQL Server data source allows you to read data from and write data to the SQL Server instances, and supports configuring synchronization tasks in wizard mode and script mode.

**Note:** If SQL Server is in a VPC environment, you need to be aware of the following issues.

-   Add an SQL Server data source
    -   Test connectivity is not supported, but the configuration synchronization task is still supported, and you can click **confirm** when creating the data source.
    -   You must use a custom scheduled Resource Group to run the corresponding synchronization tasks, make sure that the Custom Resource Group can connect to your self-built database. For more information, see[Data integration when the network of data source \(one side only\) is disconnected](reseller.en-US/User Guide/Data Integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#) and[Data sync when the network of data source \(both sides\) is disconnected](reseller.en-US/User Guide/Data Integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).
-   SQL Server data sources created with RDS

    You do not need to select a network environment, and the system automatically determines based on the information you fill in for the RDS instance.


## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** source to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15396610597595_en-US.png)

4.  In the new data source pop-up box, select the data source type as **SQL Server**.
5.  Configure individual information items for the PostgreSQL data source.

    PostgreSQL data source types are divided into **Ali cloud database \(RDS\)**, **Public Network IP**, and **non-public network IP**, you can choose according to your situation.

    Consider a data source of the new **SQL Server** \> **Alibaba cloud database \(RDS\)** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15396610597596_en-US.png)

    Configurations:

    -   Type: Alibaba cloud database \(RDS \).
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Instance ID of RDS: You can view the instance ID of the RDS in the control desk of the RDS.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15396610597597_en-US.png)

    -   RDS instance buyer ID: You can view the information in the RDS console security settings.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15396610597598_en-US.png)

    -   User name/Password: The user name and password used to connect to the database.
    **Note:** Before you can connect successfully, you need to add an RDS white list.

    Consider a data source with a new **SQL Server** \> **public network IP** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15396610607599_en-US.png)

    Configurations:

    -   Type: With a public IP address.
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   JDBC URL: JDBC connection information in the form of jdbc:sqlserver://ServerIP:Port;DatabaseName=Database.
    -   Username/Password: The user name and password used to connect to the database.
    Consider a data source with a new**SQL Server** \> **public network IP** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15396610607600_en-US.png)

    Configurations:

    -   Type: data source without a public IP address.
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Resource Group: It is used to run synchronization tasks, and generally multiple machines can be bound when you add a resource group. For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).
    -   JDBC URL: JDBC connection information in the form of jdbc:sqlserver://ServerIP:Port;DatabaseName=Database.
    -   User name/Password: The user name and password used to connect to the database.
6.  Click **Test Connectivity**.
7.  When the connectivity test is passed, click **Complete**.

## Connectivity test description {#section_pnb_31c_p2b .section}

-   The connectivity test is available in the classic network arrangement, to identify whether the input JDBC URL, user name, and password are correct.
-   Private Network and no public network IP, data source connectivity test is currently not supported, click **OK**.

## Next step {#section_jb1_tyb_p2b .section}

Now you have learned how to configure the SqlServer data source. The document explains how to configure the SQL Server Writer plug‑in later. For more information, see [Configure SQL Server Writer](reseller.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure SQL Server Writer.md#).

