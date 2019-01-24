# 配置POLARDB数据源 {#concept_imb_1zb_5fb .concept}

POLARDB关系型数据库数据源提供了读取和写入POLARDB双向通道的能力，本文为您介绍配置POLARDB数据源详细的操作步骤及相关说明。

您可以通过向[向导模式配置](cn.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/向导模式配置.md#)和[脚本模式配置](cn.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/脚本模式配置.md#)配置同步任务。

**说明：** 

当前POLARDB数据源不支持自定义资源组，请使用默认资源组。如果您需要使用自定义资源组，请选择添加[配置MySQL数据源](cn.zh-CN/使用指南/数据集成/数据源配置/配置MySQL数据源.md#)，选择**无公网IP**。

如果您的数据源是在VPC环境下的POLARDB，需要注意以下问题：

-   自建的POLARDB数据源
    -   不支持测试连通性，但仍支持配置同步任务，创建数据源时单击确认即可。
    -   必须使用自定义调度资源组运行对应的同步任务，请确保自定义资源组可以连通您的自建数据库，详情请参见[（仅一端不通）数据源网络不通的情况下的数据同步](cn.zh-CN/使用指南/数据集成/最佳实践/（仅一端不通）数据源网络不通的情况下的数据同步.md#)和[（两端都不通）数据源网络不通的情况下的数据同步](cn.zh-CN/使用指南/数据集成/最佳实践/（两端都不通）数据源网络不通的情况下的数据同步.md#)。
-   通过实例ID创建的POLARDB数据源

    您无需选择网络环境，系统自动根据您填写的POLARDB实例信息进行判断。


## 操作步骤 {#section_bzw_mfs_5fb .section}

1.  以项目管理员（主账号）身份登录[DataWorks管理控制台](https://workbench.data.aliyun.com/console)页面。单击对应项目操作栏进入工作区。

2.  鼠标移至左上角DataWorks的标识，选择**数据集成**。
3.  在数据源界面单击新增数据源，弹出支持的数据源，如下图。

    ![](images/32074_zh-CN.jpeg)

4.  在新建数据源弹出框中，选择数据源类型为**阿里云数据库（POLARDB）**。
5.  配置POLARDBL数据源的各个信息项。

    ![](images/32086_zh-CN.jpeg)

    配置项说明如下：

    -   数据源类型：当前选择的数据源类型为阿里云数据源（POLARDB）
    -   数据源名称：数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。
    -   数据源描述：对数据源进行简单描述，不得超过80个字符。
    -   集群ID：可以在POLARDB管控台找到相应的集群ID内容。
    -   POLARDB实例主账号ID：您可在POLARDB管控台安全设置中查看相应的信息。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62183/154830972432076_zh-CN.png)

    -   数据库名：POLARDB里创建的数据库名。
    -   用户名/密码：数据库对应的用户名和密码。
6.  单击**测试连通性**。
7.  测试连通性通过后，单击**确定**。

**说明：** 您需要先添加白名单才能连接成功，详情请参看[添加白名单](https://help.aliyun.com/document_detail/72977.html#concept_jz3_bl5_q2b)文档。

## 测试连通性说明 {#section_i34_pgs_5fb .section}

-   经典网络下，能够提供测试连通性能力。
-   专有网络以添加实例ID形式能够添加成功，提供相关反向代理功能。

## 后续步骤 {#section_zj4_qgs_5fb .section}

现在，您已经学习了如何配置POLARDB数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置POLARDB插件，详情请参见[配置POLARDB Reader](cn.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置POLARDB Reader.md#)和[配置POLARDB Writer](cn.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置POLARDB Writer.md#)。

