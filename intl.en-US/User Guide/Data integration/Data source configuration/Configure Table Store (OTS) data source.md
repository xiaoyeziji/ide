# Configure Table Store \(OTS\) data source {#concept_ccz_rqb_p2b .concept}

This topic describes how to configure Table Store \(OTS\) data source. Table Store is a NoSQL database service built on Alibaba Cloud’s Apsara distributed file system, enabling you to store and access massive volumes of structured data in real time.

**Note:** For more information about Table Store, see [Table Store Product Overview](https://www.alibabacloud.com/help/doc-detail/27280.htm).

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
3.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
4.  Click **New Source** on the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16210/15514310317569_en-US.png)

5.  Select the data source type **Table Store \(OTS\)** in the new dialog box.
6.  Complete the Table Store data source configuration.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16210/15514310317571_en-US.png)

    Configurations:

    -   Name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
    -   Description: A brief description of the data source that does not exceed 80 characters in length.
    -   Endpoint: The endpoint format of the Table Store server`http://yyy.com`. For more information, see [Endpoint](https://www.alibabacloud.com/help/doc-detail/52671.htm).
    -   Table Store Instance ID: The Instance ID corresponding to the Table Store service.
    -   AccessID/AccessKey: The [AccessKey](https://www.alibabacloud.com/help/doc-detail/53045.htm) \(AccessKeyID and AccessKeySecret\) is equivalent to the logon password.
7.  Click **Test Connectivity**
8.  When the connectivity passed the test, click **Complete**.

## Connectivity test description {#section_eq3_lnb_p2b .section}

-   The connectivity test is available in the classic network to identify whether the entered endpoint or AccessKey information is correct.
-   The VPC network currently does not support data source connectivity test. Click **OK**.

