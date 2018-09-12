# Configure the AnalyticDB Data Source {#concept_p3d_44v_42b .concept}

AnalyticDB \(ADS\) allows you to write data to AnalyticDB but does not allow you to read data from AnalyticDB. It supports data integration in wizard mode and script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16197/15367291837524_en-US.png)

4.  In the Create Data Source dialog box displayed, set the data source type to AnalyticDB \(ADS\).
5.  Complete the configuration items of the AnalyticDB data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16197/15367291837525_en-US.png)

    -   [Configure the DM Data Source](https://www.alibabacloud.com/help/faq-detail/74294.htm)
    -   [Configure the DM Data Source](https://www.alibabacloud.com/help/faq-detail/74294.htm)
    Configurations:

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Link Url: The ADS URL. Format: serverIP:Port.
    -   Schema: The ADS Schema information.
    -   AccessID/AceessKey: [the access key](https://www.alibabacloud.com/help/doc-detail/53045.htm) \(AccessKeyID and AccessKeySecret\) is equivalent to the login password.
6.  Click **Test Connectivity**.
7.  When the connectivity test is passed, click**Complete**.

    Provides the ability to test connectivity to determine if the information entered is correct.


## Next step { .section}

Now you have learned how to configure the ADS data source. The document explains how to configure the ADS Writer plug‑in later. For more information, see [Configure AnalyticDB\(ADS\) writer](intl.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure AnalyticDB(ADS) writer.md#).

