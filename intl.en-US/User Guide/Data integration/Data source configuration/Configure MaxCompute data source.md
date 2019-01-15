# Configure MaxCompute data source {#concept_tms_vp1_p2b .concept}

MaxCompute \(formerly known as ODPS\) provides a comprehensive data import solution that allows quicker massive data computing.  As a data hub, the MaxCompute data source allows you to read data from and write data to MaxCompute, and supports the Reader and Writer plug-ins.

**Note:** A default data source \(odps\_first\) is generated for each project, and the MaxCompute project name is the same as the one for the computing engine of the current project.

The AK of the default data source can click on the user information in the upper right and switch at the modification of AccessKey information, but it should be noted that:

1.  You can only switch from the main account AK to the main account AK.
2.  When switching, there must be no tasks currently in operation \(data integration or data development and all other tasks related to DataWorks\).

The MaxCompute data source you add yourself can use the subaccount AK.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16204/15475197027542_en-US.png)

4.  In the new data source window box, select the data source type as **MaxCompute\(ODPS\)**.
5.  Complete the configuration items of the MaxCompute data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16204/15475197027543_en-US.jpg)

    Configurations:

    -   Data source name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Data source description: It is a brief description of the data source with no more than 80 characters.
    -   ODPS endpoint: defaults to read-only. The value is automatically read from the system configuration.
    -   ODPS project name: the corresponding MaxCompute project indicator.
    -   AccessID/AceessKey: the [access key](https://www.alibabacloud.com/help/zh/doc-detail/53045.htm)\(AccessKeyID and AccessKeySecret\) is equivalent to the logon password.
6.  Click **Test Connectivity**.
7.  When the connectivity test is passed, click **Complete**.

    The connectivity test is provided to identify whether the input project/AK information is correct.


## Next step {#section_dqv_5d1_p2b .section}

Now you have learned how to configure the MaxCompute data source. The document explains how to configure the MaxCompute Writer plug‑in later. For more information, see [Configure MaxCompute Writer](intl.en-US/User Guide/Data integration/Task Configuration/Configure Writer plug-in/Configure MaxCompute Writer.md#).

