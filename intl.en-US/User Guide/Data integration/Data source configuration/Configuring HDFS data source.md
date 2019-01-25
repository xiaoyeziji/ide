# Configuring HDFS data source {#concept_zyp_2j1_p2b .concept}

As a distributed file system, HDFS, allows you to read and write data to HDFS, and supports configuring synchronization tasks in script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** from the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** on the supported data source pop up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16202/15483996097538_en-US.png)

4.  In the new data source pop up window, select the data source type **HDFS**.
5.  Configure HDFS data sources information individually .

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16202/15483996097539_en-US.png)

    Configurations:

    -   Name: A name must start with a letter or underline \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underlines \(\_\).
    -   Description: A brief description of the data source and cannot exceed 80 characters in length.
    -   defaultFS: The node address of nameNode in the format of hdfs://ServerIP:Port.
6.  Click **Test Connectivity**
7.  When the connectivity has passed the test, click **Complete**.

    Provides test connectivity capability to determine if the information entered is correct.


## Connectivity test description {#section_rrz_yc1_p2b .section}

-   The connectivity test is available in the classic network to verify whether the entered JDBC URL, user name, and password are valid.
-   The VPC network currently does not support data source connectivity tests, , and you can click **OK**.

## Next step {#section_dqv_5d1_p2b .section}

Now you have learned how to configure the HDFS data source. For more information on how to configure the HDFS writer plug-in, see [Configure HDFS Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure Writer plug-in/Configure HDFS Writer.md#).

