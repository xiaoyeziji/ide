# 配置DataHub数据源 {#concept_jwx_3qv_42b .concept}

DataHub为您提供了完善的数据导入方案，能够更快速地解决海量数据计算问题。DataHub数据源作为数据中枢，提供了其它数据源将数据写入DataHub的能力，支持Writer插件。

**说明：** 标准模式的工作空间支持[数据源隔离](intl.zh-CN/使用指南/数据集成/数据源配置/数据源隔离.md#)功能，您可以分别添加开发环境和生产环境的数据源并进行隔离，以保护您的数据安全。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16198/15532345697526_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**DataHub**。
4.  填写DataHub数据源的各配置项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16198/15532345697527_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源的简单描述，不超过80个字。|
    |**Datahub endpoint**|默认只读，从系统配置中自动读取。|
    |**Datahub project**|对应的DataHub Project标识。|
    |**AccessID/AceessKey**|访问密匙（AccessKeyID和AccessKeySecret）相当于登录密码。|

5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

    提供测试连通性能力，可以判断输入的信息是否正确 。


## 后续步骤 {#section_qzn_4qv_42b .section}

现在，您已经学习了如何配置DataHub数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置DataHub Writer插件。详情请参见[配置DataHub Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置DataHub Writer.md#)。

