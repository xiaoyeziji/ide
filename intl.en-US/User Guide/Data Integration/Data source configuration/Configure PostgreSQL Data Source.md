# Configure PostgreSQL Data Source {#concept_a1x_krb_p2b .concept}

The PostgreSQL data source allows you to read data from and write data to PostgreSQL, and supports configuring synchronization tasks in wizard mode and script mode.

**Note:** If PostgreSQL is in a VPC environment, you need to be aware of the following issues.

-   Self-built PostgreSQL Data Source
    -   Test connectivity is not supported, but the configuration synchronization task is still supported, and you can click **confirm** when creating the data source.
    -   You must use a custom scheduled Resource Group to run the corresponding synchronization tasks, make sure that the Custom Resource Group can connect to your self-built database. For more information, see[Data integration when the network of data source \(one side only\) is disconnected](intl.en-US/User Guide/Data Integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#)and[Data sync when the network of data source \(both sides\) is disconnected](intl.en-US/User Guide/Data Integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).
-   PostgreSQL data sources created with RDS

    You do not need to select a network environment, and the system automatically determines based on the information you fill in for the RDS instance.


## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** source to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15367210767572_en-US.png)

4.  In the new data source pop-up box, select the data source type as **PostgreSQL**.
5.  Configure individual information items for the PostgreSQL data source.

    PostgreSQL data source types are divided into **Alibaba cloud database \(RDS\)**, **Public Network IP**, and **non-public network IP**, you can choose according to your situation.

    Consider a data source of the new **PostgreSQL** \> **Ali cloud database \(RDS\)** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15367210767581_en-US.png)

    Configurations:

    -   Type: Alibaba cloud database \(RDS \).
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   RDS instance ID: You can view the instance id of the RDS in the control desk of the RDS.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15367210767582_en-US.png)

        Consider a data source that adds a **PostgreSQL** \> **with a common network IP** type.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15367210767584_en-US.png)

        Configurations:

        -   Type: With a public IP address.
        -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
        -   Description: It is a brief description of the data source with no more than 80 characters.
        -   JDBC URL: Format: jdbc:mysql://ServerIP:Port/database.
        -   Username/Password: The user name and password used to connect to the database.
    Consider the new **PostgreSQL** \> **Data Source with no public network IP** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15367210767585_en-US.png)

    Configurations:

    -   Type: data source without a public IP address.
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Resource Group: It is used to run synchronization tasks, and generally multiple machines can be bound when you add a resource group. For details, see[Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#)
    -   JDBC URL: Format: jdbc:mysql://ServerIP:Port/database.
    -   Username/Password: The user name and password used to connect to the database.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

## Connectivity test description { .section}

-   The connectivity test is available in the classic network arrangement, to identify whether the input JDBC URL, user name, and password are correct.
-   Private Network and no public network IP, data source connectivity test is currently not supported, click **Complete**.

## Next step {#section_jb1_tyb_p2b .section}

Now you have learned how to configure the PostgreSQL data source. The document explains how to configure the PostgreSQL Writer plug‑in later. For more information, see [Configuring PostgreSQL writer](intl.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure PostgreSQL Writer.md#).

