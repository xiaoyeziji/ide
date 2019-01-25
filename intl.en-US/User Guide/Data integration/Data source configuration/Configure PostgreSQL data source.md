# Configure PostgreSQL data source {#concept_a1x_krb_p2b .concept}

This topic describes how to configure PostgreSQL data source. The PostgreSQL data source allows you to read and write data on PostgreSQL, and supports configuring synchronization tasks in wizard mode and script mode.

**Note:** If PostgreSQL is in a VPC environment, you need to be aware of the following issues:

-   On-premise PostgreSQL data source
    -   On-premise PostgreSQL does not support test connectivity, but supports synchronization task configuration. You can synchronize task configurations by clicking **OK** when creating the data source.
    -   You must use a custom scheduled Resource Group to run the corresponding synchronization tasks, ensure the Custom Resource Group can connect to your on-premise database. For more information, see[Data integration when the network of data source \(one side only\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#)and[Data sync when the network of data source \(both sides\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).
-   PostgreSQL data sources created with RDS

    You do not need to select a network environment, and the system automatically determines the network environment based on information you entered for the RDS instance.


## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15483999187572_en-US.png)

4.  In the new data source dialog box, select the data source type **PostgreSQL**.
5.  Configure PostgreSQL data source individual information items .

    PostgreSQL data source types are categorized into **Alibaba Cloud database \(RDS\)**, **Public Network IP address**, and **non-public network IP address**, you can select the data source type based on your situation.

    Consider a data source of the new **PostgreSQL** \> **Alibaba Cloud database \(RDS\)** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15483999187581_en-US.png)

    Configurations:

    -   Type: Alibaba Cloud database \(RDS \).
    -   Name: The name must start with a letter or underline \(\_\) and cannot exceed 60 characters in length. The name can contain letters, numbers, and underlines \(\_\).
    -   Description: A brief description of the data source that cannot exceed 80 characters in length.
    -   RDS instance ID: You can view the RDS instance ID in the RDS console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15483999187582_en-US.png)

        For example, a data source that adds a **PostgreSQL** \> **with a common network IP address** type.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15483999187584_en-US.png)

        Configurations:

        -   Type: With a public IP address.
        -   Name: A name must start with a letter or underlines \(\_\) and cannot exceed 60 characters in length. It must contain letters, numbers, and underlines \(\_\).
        -   Description: A brief description of the data source that cannot exceed 80 characters in length.
        -   JDBC URL: The JDBC URL format is: jdbc:mysql://ServerIP:Port/database.
        -   Username and password: The user name and password used for connecting to the database.
    Consider the new **PostgreSQL** \> **Data Source without public network IP address** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15483999187585_en-US.png)

    Configurations:

    -   Type: Data source without a public IP address.
    -   Name: The name must start with a letter or underline \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underlines \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   Resource Group: It is used to run synchronization tasks, and typically multiple machines can be bound when you add a resource group. For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common Configuration/Add scheduling resources.md#).
    -   JDBC URL: The JDBC URL format is: jdbc:mysql://ServerIP:Port/database.
    -   Username and password: The user name and password used for database connection.
6.  Click **Test Connectivity**
7.  When the connectivity has passed the test, click **Complete**.

## Connectivity test description { .section}

-   The connectivity test is available in the classic network, to identify whether the entered JDBC URL, user name, and password are valid.
-   Private network and IP address without public network currently does not support data source connectivity test, click **OK**.

## Next step {#section_jb1_tyb_p2b .section}

Now you have learned how to configure the PostgreSQL data source. For more information on how to configure the PostgreSQL writer plug-in, see [Configure PostgreSQL Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure Writer plug-in/Configure PostgreSQL Writer.md#).

