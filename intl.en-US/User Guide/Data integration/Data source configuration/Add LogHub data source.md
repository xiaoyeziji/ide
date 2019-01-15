# Add LogHub data source {#concept_hrr_q41_p2b .concept}

As a data hub, the LogHub data source allows you to read data from and write data to LogHub, and supports the Reader and Writer plug-ins.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16203/15475242007540_en-US.png)

4.  In the new data source dialog box, select the data source type **LogHub**.
5.  Configure individual information items for the loghub data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16203/15475242007541_en-US.png)

    Configurations:

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Data source description: It is a brief description of the data source with no more than 80 characters.
    -   LogHub Endpoint: Generally in the format of http://cn-shanghai.log.aliyun.com. Please refer to the service portal for details.[service entrance](https://www.alibabacloud.com/help/doc-detail/29008.htm).
    -   Project: Name of the project.
    -   AccessID/AceessKey: the [access key](https://www.alibabacloud.com/help/doc-detail/53045.htm)\(AccessKeyID and AccessKeySecret\) is equivalent to the logon password.
6.  Click **Test Connectivity**.
7.  When the connectivity test is passed, click **Complete**.

    The connectivity test is provided to identify whether the input project/AK information is correct.


## Next step {#section_dqv_5d1_p2b .section}

Now you have learned how to configure the LogHub data source. In this tutorial you will learn how to [Configure LogHub Reader](intl.en-US/User Guide/Data integration/Task Configuration/Configure Reader plug-in/Configure LogHub Reader.md#)and[Configure LogHub Writer](intl.en-US/User Guide/Data integration/Task Configuration/Configure Writer plug-in/Configure LogHub Writer.md#).

