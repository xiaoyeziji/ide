# DataHub data source {#concept_jwx_3qv_42b .concept}

DataHub provides a comprehensive data import solution that allows quicker massive data computing. The DataHub data source, as the data pivot, allows other data sources to write data to DataHub and supports the Writer plug-in.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16198/15476021967526_en-US.png)

4.  In the new data source dialog box, select the data source type as datahub.
5.  Configure individual information items for the datahub data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16198/15476021967527_en-US.png)

    Configurations:

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Datahub endpoint: This parameter is read-only by default and is automatically read from the system configuration.
    -   Datahub project: ID of the DataHub Project.
    -   AccessID/AceessKey: [the access key](https://www.alibabacloud.com/help/doc-detail/53045.htm)\(AccessKeyID and AccessKeySecret\) is equivalent to the logon password.
6.  Click **Test Connectivity**.
7.  When the connectivity test is passed, click **Complete**.

    Provides the ability to test connectivity to determine if the information entered is correct.


## Next step {#section_qzn_4qv_42b .section}

Now you have learned how to configure the DataHub data source. The document explains how to configure the Oralce Writer plug‑in later. For more information, see [Configure DataHub Writer](intl.en-US/User Guide/Data integration/Task Configuration/Configure Writer plug-in/Configure DataHub Writer.md#).

