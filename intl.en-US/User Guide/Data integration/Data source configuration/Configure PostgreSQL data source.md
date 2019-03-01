# Configure PostgreSQL data source {#concept_a1x_krb_p2b .concept}

This topic describes how to configure a PostgreSQL data source. The PostgreSQL data source allows you to read/write data on PostgreSQL, and supports configuring synchronization tasks in wizard and script mode.

**Note:** If the PostgreSQL is in a VPC environment, you need to note the following issues:

-   On-premise PostgreSQL data source
    -   The on-premise PostgreSQL does not support test connectivity, but supports synchronization task configuration. You can synchronize task configurations by clicking **OK**, when creating the data source.
    -   You must use a custom scheduled Resource Group to run the corresponding synchronization tasks, ensure the Custom Resource Group can connect to the on-premise database. For more information, see[Data integration when the network of data source \(one side only\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#)and[Data sync when the network of data source \(both sides\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).
-   PostgreSQL data sources created with RDS

    You do not need to select a network environment, the system automatically selects the network environment based on the RDS instance information.


## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15514310567572_en-US.png)

4.  Select the data source type **PostgreSQL** in the new dialog box.
5.  Complete the PostgreSQL data source individual information items configuration.

    PostgreSQL data source types are categorized into **Apsara DB for RDS**, **Public Network IP Address**, and **Non-Public Network IP Address**. You can select the data source type based on the situation.

    The following is an example of how to add a new **PostgreSQL** \> **Apsara DB for RDS** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15514310567581_en-US.png)

    Configurations:

    -   Type: Apsara DB for RDS.
    -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. The name can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that cannot exceed 80 characters in length.
    -   RDS instance ID: You can view the RDS instance ID in the RDS console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15514310567582_en-US.png)

        The following figure is an example of a data source that adds a **PostgreSQL** \> **With a Public Network IP Address** type.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15514310567584_en-US.png)

        Configurations:

        -   Type: A PostgreSQL data source with a public IP address.
        -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It must contain letters, numbers, and underscores \(\_\).
        -   Description: A brief description of the data source that cannot exceed 80 characters in length.
        -   JDBC URL: The JDBC URL format is: jdbc:mysql://ServerIP:Port/database.
        -   Username and password: The user name and password used for connecting to the database.
    The following is an example of new **PostgreSQL** \> **Data Source Without Public Network IP Address** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15514310567585_en-US.png)

    Configurations:

    -   Type: A PostgreSQL data source without a public IP address.
    -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that cannot exceed 80 characters in length.
    -   Resource Group: The resource used to run synchronization tasks. Typically, you can bound multiple machines when you add a resource group. For more information, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).
    -   JDBC URL: The JDBC URL format is: jdbc:mysql://ServerIP:Port/database.
    -   Username and password: The user name and password used for database connection.
6.  Click **Test Connectivity**
7.  When the connectivity has passed the test, click **Complete**.

## Connectivity test description { .section}

-   The connectivity test is available in the classic network to verify whether the entered JDBC URL, user name, and password are valid.
-   Currently, private network and IP address without public network does not support data source connectivity test. Click **OK**.

## Next step {#section_jb1_tyb_p2b .section}

For more information on how to configure the PostgreSQL Writer plug-in, see [Configure PostgreSQL Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure PostgreSQL Writer.md#).

