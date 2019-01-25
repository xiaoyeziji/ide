# Configure OSS data source {#concept_mjt_hpb_p2b .concept}

This topic describes how to configure an Object Storage Service \(OSS\) data source. OSS is a massive, secure, and highly reliable cloud storage service offered by Alibaba Cloud.

**Note:** 

-   If you want to learn more about [OSS products](https://www.alibabacloud.com/help/doc-detail/31817.htm), see the OSS Product Overview.
-   The OSS Java SDK can be found in the [Alibaba Cloud OSS Java SDK](http://oss.aliyuncs.com/aliyun_portal_storage/help/oss/OSS_Java_SDK_Dev_Guide_20141113.pdf).

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16209/15483998427559_en-US.png)

4.  In the new data source dialog box, and select the data source type **OSS**.
5.  Enter the OSS data source configuration items to be created.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16209/15483998427560_en-US.png)

    Configurations:

    -   Name: The name must start with a letter or underline \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underlines \(\_\).
    -   Description: A brief description of the data source with no more than 80 characters in length.
    -   Endpoint: The OSS endpoint information format is :`http://oss.aliyuncs.com`. It is the endpoint of the OSS service, and the Region. When you visit an endpoint in a different region, you need to enter the different domain names.

        **Note:** The correct endpoint format is`http://oss.aliyuncs.com`, but add the bucket value before the OSS to connect`http://oss.aliyuncs.com` in the form of a point number. For example`http://xxx.oss.aliyuncs.com`, can pass connectivity tests, but will report errors during synchronization.

    -   Bucket: The OSS instance bucket. The bucket is a storage space and serves as the container for storing objects. You can create one or more buckets and add one or more files to each bucket. The bucket entered here searches for corresponding files in the data synchronization task, and file searching is unavailable for buckets that have not been added.
    -   AccessID/AccessKey: The [AccessKey](https://www.alibabacloud.com/help/doc-detail/53045.htm) \(AccessKeyID and AccessKeySecret\) is equivalent to the logon password.
6.  Click **Test Connectivity**
7.  When the connectivity has passed the test, click **Complete**.

## Connectivity test description {#section_eq3_lnb_p2b .section}

-   The connectivity test is available in classic network to identify whether the entered Endpoint and AccessKey information is correct.
-   The data source connectivity test is currently not supported by the VPC network, and you can click **OK**.

## Next step {#section_bbg_jnb_p2b .section}

Now you have learned how to configure the OSS data source. The next topic describes how to configure the OSS writer plug‑in. For more information, see [Configure OSS Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure Writer plug-in/Configure OSS Writer.md#).

