# Configure SQL Server data source {#concept_o4y_5yb_p2b .concept}

This topic describes how to configure SQL server data source. The SQL server data source allows you to read and write data to SQL server instances, and supports configuring synchronization tasks in wizard and script mode.

**Note:** Now we only support SQL Server 2005 or higher versions. If the SQL server is in a VPC environment, please take note of the following issues:

-   Create an SQL server data source
    -   Although, test connectivity is not supported, synchronization of task configuration is supported. You can synchronize task configurations by clicking **confirm** when creating the data source.
    -   You must use a custom scheduled Resource Group to run corresponding synchronization tasks, make sure the Custom Resource Group can connect to your on-premise database. For more information, see[Data integration when the network of data source \(one side only\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#) and[Data sync when the network of data source \(both sides\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).
-   SQL server data sources created with RDS

    You do not need to select a network environment, the system will automatically determine the data source based on the information entered for the RDS instance.


## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator, and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** in the supported data source pop up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15499637257595_en-US.png)

4.  In the new data source dialog box, select the data source type **SQL Server**.
5.  Configure SQL Server data source information individually.

    SQL server data source types are categorized into **Alibaba Cloud database \(RDS\)**, **Public Network IP**, and **non-public network IP**, you can select the data type based on your requirements.

    Consider the new data source of **SQL Server** \> **Alibaba Cloud database \(RDS\)** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15499637257596_en-US.png)

    Configurations:

    -   Type: Alibaba Cloud database \(RDS\).
    -   Name: The name must start with a letter or underlines \(\_\) and cannot exceed 60 characters in length. It can cotnain letters, numbers, and underlines \(\_\).
    -   Description: A brief description of the data source that cannot exceed 80 characters in length.
    -   RDS instance ID: You can view the RDS instance ID in the RDS console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15499637267597_en-US.png)

    -   RDS instance buyer ID: You can view the buyer's information under RDS console security settings.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15499637267598_en-US.png)

    -   Username and password: The user name and password for database connection.
    **Note:** You need to add an RDS whitelist before connecting to the database.

    Consider a data source with a new **SQL Server** \> **public network IP address** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15499637267599_en-US.png)

    Configurations:

    -   Type: With a public IP address.
    -   Name: The name must start with a letter or underline \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underlines \(\_\).
    -   Description: A brief data source description that cannot exceed 80 characters in length.
    -   JDBC URL: JDBC connection information in the form of jdbc:sqlserver://ServerIP:Port;DatabaseName=Database.
    -   Username and password: The user name and password used to connect to the database.
    Consider a data source with a new**SQL Server** \> **public network IP** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16213/15499637267600_en-US.png)

    Configurations:

    -   Type: A data source without a public IP address.
    -   Name: The name must start with a letter or underline \(\_\) and can be 1 to 60 characters in length. it can contain letters, numbers, or underlines \(\_\).
    -   Description: A brief description of the data source and can be 1 to 80 characters.
    -   Resource Group: It is used to run synchronization tasks, and generally multiple machines can be bound when you add a resource group. For more information, see [Add task resources](reseller.en-US/User Guide/Data integration/Common Configuration/Add task resources.md#).
    -   JDBC URL: JDBC connection information in the form of jdbc:sqlserver://ServerIP:Port;DatabaseName=Database.
    -   Username and Password: The user name and password used to connect to the database.
6.  Click **Test Connectivity**.
7.  When the test connectivity is passed, click **Complete**.

## Connectivity test description {#section_pnb_31c_p2b .section}

-   The connectivity test is available in the classic network configuration ,identify whether the input JDBC URL, user name, and password are correct.
-   Private network and no public network IP address, currently does not support data source connectivity test, click **OK**.

## Next step {#section_jb1_tyb_p2b .section}

Now you have learned how to configure a SQL Server data source. For more information on how to configure the SQL server writer plug‑in, see [Configure SQL Server Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure SQL Server Writer.md#).

