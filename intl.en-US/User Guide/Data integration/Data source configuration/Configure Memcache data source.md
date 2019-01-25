# Configure Memcache data source {#concept_ijs_vq1_p2b .concept}

The Memcache \(formerly known as OCS\) data source provides the ability to write data from other data sources to Memcache, and supports configuring synchronization tasks in script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16205/15483997247544_en-US.png)

4.  In the new data source dialog box, select **Memcached** as the data source type.
5.  Complete the Memcache data source configuration.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16205/15483997247545_en-US.png)

    Configurations:

    -   Name: The name must start with a letter or underline \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underlines \(\_\).
    -   Description: A brief description of the data source that cannot exceed 80 characters in length.
    -   Type: Select Memcache as the data source type.
    -   Proxy Host: The corresponding memcache proxy.
    -   Port: The corresponding memcache port. The default port is 11211.
    -   Username and password: The database user name and password.
6.  Click **Test Connectivity**
7.  When the connectivity has passed the test, click **Complete**.

    Provides test connectivity capabilities to determine whether the entered information is valid.


## Next step {#section_dqv_5d1_p2b .section}

Now you have learned how to configure the Memcache data source. For more information on configure the Memcache writer plug‑in. For more information, see [Configure Memcache \(OCS\) Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure Writer plug-in/Configure Memcache (OCS) Writer.md#).

