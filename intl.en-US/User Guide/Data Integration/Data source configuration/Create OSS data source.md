# Create OSS data source {#concept_mjt_hpb_p2b .concept}

Object Storage Service \(OSS\) is a massive, secure, and highly reliable cloud storage service offered by Alibaba Cloud.

**Note:** 

-   If you want to learn more about [OSS products](https://www.alibabacloud.com/help/doc-detail/31817.htm), see the OSS Product Overview.
-   The OSS Java SDK can be found in the [Alibaba Cloud OSS Java SDK](http://oss.aliyuncs.com/aliyun_portal_storage/help/oss/OSS_Java_SDK_Dev_Guide_20141113.pdf).

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16209/15367210037559_en-US.png)

4.  In the new data source pop-up box, select the data source type as **OSS**.
5.  Fill in configuration items for the OSS data source to be created.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16209/15367210037560_en-US.png)

    Configurations:

    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Endpoint: OSS endpoint information, in the form of `http://oss.aliyuncs.com`, the Endpoint of the OSS service, and the Region, when you visit different Regions, you need to fill in different domain names.

        **Note:** The correct filling format for Endpoint is`http://oss.aliyuncs.com`, but add the bucket value before the OSS to connect`http://oss.aliyuncs.com` in the form of a point number, for example`http://xxx.oss.aliyuncs.com`, test connectivity can pass, but synchronization will report errors.

    -   Bucket: The bucket of the OSS instance. The bucket is a storage space and serves as the container for storing objects. You can create one or more buckets and add one or more files to each bucket. The bucket entered here searches for corresponding files in the data synchronization task, and file searching is unavailable for non-added buckets.
    -   AccessID/AceessKey: the [access key](https://www.alibabacloud.com/help/doc-detail/53045.htm) \(AccessKeyID and AccessKeySecret\) is equivalent to the login password.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

## Connectivity test description {#section_eq3_lnb_p2b .section}

-   The connectivity test is available in the classic network to identify whether the input Endpoint/AK information is correct.
-   The data source connectivity test is currently not supported by the proprietary network, and you can click **confirm**.

## Next step {#section_bbg_jnb_p2b .section}

Now you have learned how to configure the OSS data source. The document explains how to configure the OSS Writer plug‑in later. For more information, see [Configure the OSS writer](intl.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure OSS Writer.md#).

