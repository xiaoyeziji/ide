# DataHub data source {#concept_jwx_3qv_42b .concept}

This topic describes how to configure a DataHub data source. DataHub provides a comprehensive data import solution that allows faster massive data computing. DataHub data source allows other data sources to write data to DataHub and supports the writer plug-in.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16198/15483994307526_en-US.png)

4.  In the new data source dialog box, select the data source type DataHub.
5.  Configure DataHub data source individual items.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16198/15483994307527_en-US.png)

    Configurations:

    -   Name: The name must start with a letter or underline \(\_\), and cannot exceed 60 characters in length. It can contain letters, numbers, and underlines \(\_\).
    -   Description: A brief description of the data source and cannot exceed 80 characters in length.
    -   DataHub endpoint: By default, this parameter is read-only and is automatically read from the system configuration.
    -   DataHub project: The DataHub project ID.
    -   AccessID/AceessKey: [The access key](https://www.alibabacloud.com/help/doc-detail/53045.htm)\(AccessKeyID and AccessKeySecret\) is equivalent to the logon password.
6.  Click **Test Connectivity**.
7.  When the connectivity passes the test, click **Complete**.

    Provides connectivity test capability to determine if the information entered is correct.


## Next step {#section_qzn_4qv_42b .section}

Now you have learned how to configure the DataHub data source. The document describes how to configure the Oracle writer plug‑in later. For more information, see [Configure DataHub Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure Writer plug-in/Configure DataHub Writer.md#).

