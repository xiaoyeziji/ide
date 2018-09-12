# Configure DRDS data sources {#concept_lmn_xc1_p2b .concept}

The DRDS data source allows you to read data from and write data to DRDS, and supports configuring synchronization tasks in wizard mode and script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16200/15367206157532_en-US.png)

4.  In the new data source pop-up box, select the data source type as **DRDS**.
5.  Fill in configuration items for the DRDS data source to be created.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16200/15367206157533_en-US.png)

    Configurations:

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   JDBC URL: JDBC URL, in the format of jdbc:mysql://serverIP:Port/database.
    -   Username/Password: The user name and password used to connect to the database.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

    Provides the ability to test connectivity to determine if the information entered is correct.


## Connectivity test description {#section_rrz_yc1_p2b .section}

-   The connectivity test is available in the classic network arrangement, to identify whether the input JDBC URL, user name, and password are correct.
-   Private Network and no public network IP, data source connectivity test is currently not supported, click **confirm**.

## Next step {#section_dqv_5d1_p2b .section}

Now you have learned how to configure the DRDS data source. The document explains how to configure the DRDS Writer plug‑in later. For more information, see [Configure DRDS writer](intl.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure DRDS Writer.md#).

