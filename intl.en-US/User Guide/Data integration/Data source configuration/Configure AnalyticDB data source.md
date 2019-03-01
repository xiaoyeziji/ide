# Configure AnalyticDB data source {#concept_p3d_44v_42b .concept}

This topic describes how to configure an AnalyticDB \(ADS\) data source. ADS allows you to write data to AnalyticDB, but does not allow you to read data from it. ADS supports data integration in wizard and script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16197/15514304987524_en-US.png)

4.  In the Create Data Source dialog box, set the data source type to AnalyticDB \(ADS\).
5.  Complete the AnalyticDB data source configuration items.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16197/15514304987525_en-US.png)

    -   [Configure the DM Data Source](https://www.alibabacloud.com/help/faq-detail/74294.htm)
    -   [Configure the DM Data Source](https://www.alibabacloud.com/help/faq-detail/74294.htm)
    Configurations:

    -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source cannot exceed 80 characters in length.
    -   Link URL: The ADS URL. Format: serverIP:Port.
    -   Schema: The ADS schema information.
    -   AccessID/AccessKey: [The access key](https://www.alibabacloud.com/help/doc-detail/53045.htm) \(AccessKeyID and AccessKeySecret\) is equivalent to the login password.
6.  Click **Test Connectivity**.
7.  After completing the test connectivity, click**Complete**.

    The test connectivity determines if the information entered is valid.


## Next step { .section}

For more information on how to configure the ADS Writer plug-in, see [Configure AnalyticDB\(ADS\) Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure AnalyticDB(ADS) Writer.md#).

