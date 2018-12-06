# Configure OTS data source {#concept_ccz_rqb_p2b .concept}

Table Store is a NoSQL database service that built on Alibaba Cloud’s Apsara distributed file system, enabling you to store and access massive volumes of structured data in real time.

**Note:** If you want to learn more about OTS, see the [OTS](https://www.alibabacloud.com/help/doc-detail/27280.htm) Product Overview.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
3.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
4.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16210/15440615997569_en-US.png)

5.  In the new data source pop-up box, select the data source type as **OTS**.
6.  Complete the configuration items for the OTS data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16210/15440615997571_en-US.png)

    Configurations:

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Endpoint: the endpoint of the table store server in the format`http://yyy.com`. For more information, see [Endpoint](https://www.alibabacloud.com/help/doc-detail/52671.htm).
    -   Table Store Instance ID: Instance ID corresponding to the Table Store service.
    -   AccessID/AceessKey: the [access key](https://www.alibabacloud.com/help/doc-detail/53045.htm) \(AccessKeyID and AccessKeySecret\) is equivalent to the login password.
7.  Click **Test Connectivity**
8.  When the connectivity test is passed, click **Complete**.

## Connectivity test description {#section_eq3_lnb_p2b .section}

-   The connectivity test is available in the classic network to identify whether the input endpoint/AK information is correct.
-   The data source connectivity test is currently not supported by the proprietary network, and you can click **confirm**.

