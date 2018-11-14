# 配置OSS数据源 {#concept_mjt_hpb_p2b .concept}

对象存储（Object Storage Service，简称OSS），是阿里云对外提供的海量、安全和高可靠的云存储服务。

**说明：** 

-   如果您想对OSS产品有更深了解，请参见[OSS产品概述](https://www.alibabacloud.com/help/doc-detail/31817.htm)。
-   OSS Java SDK请参见[阿里云OSS Java SDK](http://oss.aliyuncs.com/aliyun_portal_storage/help/oss/OSS_Java_SDK_Dev_Guide_20141113.pdf)。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16209/15421913957559_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**OSS**。
4.  配置OSS数据源的各个信息项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16209/15421913957560_zh-CN.png)

    配置项说明如下：

    -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过60个字符。
    -   数据源描述：对数据源进行简单描述，不得超过80个字符。
    -   Endpoint：OSS Endpoint信息，格式为`http://oss.aliyuncs.com`，OSS服务的Endpoint和Region有关，访问不同的Region时，需要填写不同的域名。

        **说明：** Endpoint的正确的填写格式为`http://oss.aliyuncs.com`，但是`http://oss.aliyuncs.com`在OSS前加上Bucket值以点号的形式连接，例如`http://xxx.oss.aliyuncs.com`，测试连通性可以通过，但同步会报错。

    -   Bucket：相应的OSS Bucket信息，存储空间，是用于存储对象的容器，可以创建一个或者多个存储空间，然后向每个存储空间中添加一个或多个文件。此处填写的存储空间将在数据同步任务里找到相应的文件，其他的Bucket没有添加的则不能搜索其中的文件。
    -   AccessID/AceessKey：[访问密匙](https://www.alibabacloud.com/help/doc-detail/53045.htm)（AccessKeyID和AccessKeySecret）相当于登录密码。
5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

**说明：** 在准备OSS数据时，注意如果数据为CSV文件，则必须为标准格式的CSV文件。例如：列内容如果在半角引号（"）内时，需要替换成两个半角引号（""），否则可能会造成文件被错误分割。

## 测试连通性说明 {#section_eq3_lnb_p2b .section}

-   经典网络下，能够提供测试连通性能力，可以判断输入的Endpoint、AK信息是否正确。
-   专有网络目前不支持数据源连通性测试，直接单击**确认**。

## 后续步骤 {#section_bbg_jnb_p2b .section}

现在，您已经学习了如何配置OSS数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置OSS Writer插件。详情请参见[配置OSS Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置OSS Writer.md#)和[配置OSS Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置OSS Reader.md#)。

