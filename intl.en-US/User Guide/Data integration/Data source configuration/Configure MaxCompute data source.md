# Configure MaxCompute data source {#concept_tms_vp1_p2b .concept}

This topic describes how to configure a MaxCompute data source. The MaxCompute \(formerly known as ODPS\) provides a comprehensive data import solution that accelerates massive data computing. As a data hub, the MaxCompute data source allows you to read /write data on MaxCompute, and supports reader and writer plug-ins.

**Note:** By default, a data source \(odps\_first\) is generated for each project. The MaxCompute project name is the same as that for the current project computing engine.

The AccessKey of the default data source can click on the user information in the upper right corner and change the AccessKey information modification, but it should be noted that:

1.  You can only switch AccessKeys between primary accounts.
2.  When switching there cannot be any tasks in operation whether it is data integration or data development and all other tasks related to DataWorks.

MaxCompute data sources you added manually can use the RAM user AccessKey.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16204/15514227497542_en-US.png)

4.  Select the data source type **MaxCompute \(ODPS\)** in the new window.
5.  Complete the MaxCompute data source configurations.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16204/15514227497543_en-US.jpg)

    Configurations:

    -   Data source name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Data source description: A brief description of the data source that does not exceed 80 characters in length.
    -   MaxCompute endpoint: By default, the MaxCompute endpoint is read-only. The value is automatically read from the system configuration.
    -   MaxCompute project name: The corresponding MaxCompute project indicator.
    -   AccessID/AccessKey: The [AccessKey](https://www.alibabacloud.com/help/zh/doc-detail/53045.htm)\(AccessKeyID and AccessKeySecret\) is equivalent to the logon password.
6.  Click **Test Connectivity**.
7.  When the connectivity has passed the test, click **Complete**.

    The provided connectivity test can identify whether the entered project and AccessKey information is valid.


## Next step {#section_dqv_5d1_p2b .section}

For more information on how to configure the MaxCompute Writer plug-in, see [Configure MaxCompute Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure MaxCompute Writer.md#).

