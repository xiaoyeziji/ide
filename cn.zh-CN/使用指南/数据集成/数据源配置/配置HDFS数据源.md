# 配置HDFS数据源 {#concept_zyp_2j1_p2b .concept}

HDFS是一个分布式文件系统，它提供了读取和写入HDFS双向通道的能力，可以通过脚本模式配置同步任务。

**说明：** 标准模式的工作空间支持[数据源隔离](intl.zh-CN/使用指南/数据集成/数据源配置/数据源隔离.md#)功能，您可以分别添加开发环境和生产环境的数据源并进行隔离，以保护您的数据安全。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16202/15532347037538_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**HDFS**。
4.  填写HDFS数据源的各配置项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16202/15532347037539_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**defaultFS**|nameNode节点地址，格式为`hdfs://ServerIP:Port`。|

5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

    提供测试连通性能力，可以判断输入的信息是否正确 。


## 测试连通性说明 {#section_rrz_yc1_p2b .section}

-   经典网络下，能够提供测试连通性能力，可以判断输入的JDBC URL，用户名/密码是否正确。
-   专有网络目前不支持数据源连通性测试，直接单击**确认**。

## 后续步骤 {#section_dqv_5d1_p2b .section}

现在，您已经学习了如何配置HDFS数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置HDFS Writer插件。详情请参见[配置HDFS Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置HDFS Writer.md#)和[配置HDFS Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置HDFS Reader.md#)。

