# Configure the DM data source {#concept_kyt_jsv_42b .concept}

This topic describes how to configure the DM data source. The DM relational database data source provides the capability to read/write data to DM databases, and supports configuring synchronization tasks in wizard and script modes.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator \(primary account\) and click **Enter Workspace** from the Actions column of the relevant project in the Project List.
2.  Select **Data Integration** in the top navigation bar. Click **Data Source** from the left-side navigation pane.
3.  Click **New Source** in the supported data source window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16199/15514226267528_en-US.png)

4.  In the new dialog box, select the **DM** data source type.
5.  Complete the DM data source information items configurations.

    Select either of the following data source types as required when creating a DM data source:

    -   The New DM Data Sources with public IP address

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16199/15514226267529_en-US.png)

        Parameters:

        -   Type: DM Data Sources with a public IP address.
        -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
        -   Description: A brief description of the data source that cannot exceed 80 characters in length.
        -   JDBC URL: In the format of jdbc:mysql://ServerIP:Port/Database.
        -   Username and password: The user name and password used for connecting to the database.
    -   New DM Data Sources without public IP address

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16199/15514226267530_en-US.png)

        Parameters:

        -   Type: DM data sources without public network IP address. Selecting this data source type requires the use of custom scheduling resources for synchronization. You can click **Help manual** for details.
        -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
        -   Description: A brief description of the data source that does not exceed 80 characters in length.
        -   Resource Group: It is used to run synchronization tasks, and generally you can bound multiple machines when adding a resource group. For more information, see[Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).
        -   JDBC URL: In the format of jdbc:mysql://ServerIP:Port/Database.
        -   Username and password: The user name and password to connect to the database.
6.  \(Optional\) Click **Test Connectivity**to test the connectivity after entering all the required field information.
7.  When the connectivity has passed the test, click **Complete**.

    Provides test connectivity capability to determine if the information entered is correct.


## Connectivity test description { .section}

-   The connectivity test is available in the classic network to identify whether the entered JDBC URL, user name, and password are correct.
-   Currently, VPC and data source types without public IP addresses do not support connectivity tests. As a result, click **Confirm**.

