# Add LogHub data source {#concept_hrr_q41_p2b .concept}

This topic describes how to add a LogHub data source. The LogHub is a data hub, and LogHub data source allows you to read/write data to LogHub. LogHub supports Reader and Writer plug-ins.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16203/15514308917540_en-US.png)

4.  Select the data source type **LogHub** in the new dialog box.
5.  Configure individual information items for the LogHub data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16203/15514308917541_en-US.png)

    Configurations:

    -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It contains letters, numbers, and underscores \(\_\).
    -   Data source description: A brief description of the data source that does not exceed 80 characters in length.
    -   LogHub Endpoint: Generally, the LogHub Endpoint format is in http://cn-shanghai.log.aliyun.com. For more information, see [service entrance](https://www.alibabacloud.com/help/doc-detail/29008.htm).
    -   Project: The project name.
    -   AccessID/AccessKey: The [AccessKey](https://www.alibabacloud.com/help/doc-detail/53045.htm)\(AccessKeyID and AccessKeySecret\) is equivalent to the logon password.
6.  Click **Test Connectivity**.
7.  When the connectivity has passed the test, click **Complete**.

    The connectivity test is provided to identify whether the entered AccessKey project information is correct.


## Next step {#section_dqv_5d1_p2b .section}

For more information on how to configure LogHub reader/writer, see [Configure LogHub Reader](intl.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Configure LogHub Reader.md#)and[Configure LogHub Writer](intl.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure LogHub Writer.md#).

