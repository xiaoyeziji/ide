# Configure Oracle data source {#concept_jkd_5nb_p2b .concept}

This topic describes how to configure an Oracle data source. The Oracle data source allows you to read /write data on Oracle, and supports configuring synchronization tasks in wizard and script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** on the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15514309827556_en-US.png)

4.  Select the data source type **Oracle** in the new data source dialog box.
5.  Configure each Oracle data source information item.

    Oracle Data source types are categorized into **Public Network IP Address** and **Non-Public Network IP Address**, and you can select source types based on your requirements.

    For example, a data source that adds a new **Oracle** \> **Network IP Address** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15514309827557_en-US.png)

    Configurations:

    -   Type: An Oracle data source with a public IP address.
    -   Name: The name must start with letters or underscore\(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores\(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   JDBC URL: The JDBC URL format is: jdbc:oracle:thin:@serverIP:Port:Database.
    -   Username and password: The user name and password used for connecting to the database.
    Consider a data source that adds a new **Oracle** \> **Network IP Address** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15514309827558_en-US.png)

    Configurations:

    -   Type: When there are no public network IP addresses, this data source type requires custom scheduling resources for synchronization. You can click the **Help Manual** to view it.
    -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   JDBC URL: The format of the JDBC URL is: jdbc:oracle:thin:@serverIP:Port:Database.
    -   Username and password: The user name and password used for connecting to the database.
6.  Click **Test Connectivity**
7.  When the connectivity has passed the test, proceed by clicking **Complete**.

## Connectivity test description {#section_eq3_lnb_p2b .section}

-   The connectivity test is available in the classic network environment to identify whether the entered JDBC URL, user name, and password are correct.
-   Currently, does not support private network, IP addresses without public network and data source connectivity, proceed by clicking **OK**.

## Next step {#section_bbg_jnb_p2b .section}

For more information on how to configure Oracle Writer plug-in, see [Configuring Oracle Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configuring Oracle Writer.md#).

