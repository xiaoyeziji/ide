# Configuring HDFS data source {#concept_zyp_2j1_p2b .concept}

HDFS, as a distributed file system, allows you to read data from and write data to HDFS, and supports configuring synchronization tasks in script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** from the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16202/15389851247538_en-US.png)

4.  In the new data source pop-up box, select the data source type **HDFS**.
5.  Configure individual information items for HDFS data sources.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16202/15389851247539_en-US.png)

    Configurations:

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   defaultFS: The node address of nameNode in the format of hdfs://ServerIP:Port.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

    Provides the ability to test connectivity to determine if the information entered is correct.


## Connectivity test description {#section_rrz_yc1_p2b .section}

-   The connectivity test is available in the classic network arrangement, to identify whether the input JDBC URL, user name, and password are correct.
-   The data source connectivity test is currently not supported by the proprietary network, and you can click **confirm**.

## Next step {#section_dqv_5d1_p2b .section}

Now you have learned how to configure the HDFS data source. The document explains how to configure the HDFS Writer plug-in later. For more information, see [Configure HDFS Writer](reseller.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure HDFS Writer.md#).

