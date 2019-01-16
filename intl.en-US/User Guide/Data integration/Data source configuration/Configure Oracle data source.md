# Configure Oracle data source {#concept_jkd_5nb_p2b .concept}

The Oracle data source allows you to read data from and write data to Oracle, and supports configuring synchronization tasks in wizard mode and script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15476033857556_en-US.png)

4.  In the new data source dialog box, select the data source type as **Oracle**.
5.  Configure each item of information for the Oracle data source.

    Oracle Data source types are divided into **public network IP** and **non-public network IP**, and you can choose according to your own situation.

    Consider a data source that adds a new **Oracle** \> **network IP** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15476033857557_en-US.png)

    Configurations:

    -   Type: With a public IP address.
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   JDBC URL: Format: jdbc:oracle:thin:@serverIP:Port:Database.
    -   Username/Password: The user name and password used to connect to the database.
    Consider a data source that adds a new **Oracle** \> **network IP** type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15476033867558_en-US.png)

    Configurations:

    -   Data Source Type: No public network IP, this type of data source requires custom scheduling resources for synchronization, you can click the **help manual** to view it.
    -   Data source name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Data source description: It is a brief description of the data source with no more than 80 characters.
    -   JDBC URL: Format: jdbc:oracle:thin:@serverIP:Port:Database.
    -   Username/Password: The user name and password used to connect to the database.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

## Connectivity test description {#section_eq3_lnb_p2b .section}

-   The connectivity test is available in the classic network arrangement, to identify whether the input JDBC URL, user name, and password are correct.
-   Private Network and no public network IP, data source connectivity test is currently not supported, click **confirm**.

## Next step {#section_bbg_jnb_p2b .section}

Now you have learned how to configure the Oralce data source. The document explains how to configure the Oralce Writer plug‑in later. For more information, see [Configuring Oracle Writer](intl.en-US/User Guide/Data integration/Task Configuration/Configure Writer plug-in/Configuring Oracle Writer.md#).

