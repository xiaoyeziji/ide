# Configuring HDFS data source {#concept_zyp_2j1_p2b .concept}

This topic describes configuring a HDFS data source. HDFS is a distributed file system that allows you to read/write data to HDFS, and supports configuring synchronization tasks in Script Mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** from the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** on the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16202/15514308097538_en-US.png)

4.  In the new data source pop-up window, and select the data source type **HDFS**.
5.  Configure HDFS data sources information separately.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16202/15514308097539_en-US.png)

    Configurations:

    -   Name: A name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source and cannot exceed 80 characters in length.
    -   defaultFS: The node address of nameNode in the format of hdfs://ServerIP:Port.
6.  Click **Test Connectivity**
7.  When the connectivity has passed the test, click **Complete**.

    Configure the HDFS data source that provides test connectivity capability to determine if the entered information is correct.


## Connectivity test description {#section_rrz_yc1_p2b .section}

-   The connectivity test is available in the classic network to verify whether the entered JDBC URL, user name, and password are valid.
-   Currently, the VPC network does not support data source connectivity tests. Click **OK**.

## Next step {#section_dqv_5d1_p2b .section}

For more information on how to configure the HDFS Writer plug-in, see [Configure HDFS Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure HDFS Writer.md#).

