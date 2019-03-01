# Configure DRDS data sources {#concept_lmn_xc1_p2b .concept}

This topic describes how to configure DRDS data sources. The DRDS data source allows you to read/write data to DRDS, and supports configuring synchronization tasks in wizard and script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16200/15514307607532_en-US.png)

4.  In the new data source dialog box, select the data source type **DRDS**.
5.  Enter the DRDS data source configuration items.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16200/15514307617533_en-US.png)

    Configurations:

    -   Name: The name must start with a letter or underscore \(\_\), and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   JDBC URL: The JDBC URL format is: jdbc:mysql://serverIP:Port/database.
    -   Username and password: The user name and password used for database connection.
6.  Click **Test Connectivity**
7.  When the connectivity has passed the test, click **Complete**.

    The DRDS data source provides test connectivity capability for verifying the entered information validity.


## Connectivity test description {#section_rrz_yc1_p2b .section}

-   The connectivity test is available in the classic network environment to identify whether the entered JDBC URL, user name, and password are valid.
-   Currently, the private network or IP addresses without public network, and data source connectivity tests are not supported. Click **OK**.

## Next step {#section_dqv_5d1_p2b .section}

For more information on how to configure the DRDS Writer plug‑in, see [Configure DRDS Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure DRDS Writer.md#).

