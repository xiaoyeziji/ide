# Configure Memcache data source {#concept_ijs_vq1_p2b .concept}

The Memcache \(formerly known as OCS\) data source provides the ability to write data from other data sources to Memcache, and supports configuring synchronization tasks only in script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16205/15476018987544_en-US.png)

4.  In the new data source dialog box, select the data source type as **Memcached**.
5.  Complete the configuration items for the Memcache data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16205/15476018987545_en-US.png)

    Configurations:

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Type: The selected data source type Memcache.
    -   Proxy Host: the appropriate Memcache Proxy.
    -   Port: the appropriate memcache port, with a default of 11211.
    -   Username/Password: The username and password of the database.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

    Provides the ability to test connectivity to determine if the information entered is correct.


## Next step {#section_dqv_5d1_p2b .section}

Now you have learned how to configure the Memcache data source. The document explains how to configure the Memcache Writer plug‑in later. For more information, see [Configure Memcache \(OCS\) Writer](intl.en-US/User Guide/Data integration/Task Configuration/Configure Writer plug-in/Configure Memcache (OCS) Writer.md#).

