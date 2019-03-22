# 配置HybridDB for MySQL数据源 {#concept_rdt_syb_5fb .concept}

HybridDB for MySQL数据源为您提供读取和写入HybridDB for MySQL的双向能力，本文将为您介绍如何配置HybridDB for MySQL数据源。

**说明：** 标准模式的工作空间支持[数据源隔离](intl.zh-CN/使用指南/数据集成/数据源配置/数据源隔离.md#)功能，您可以分别添加开发环境和生产环境的数据源并进行隔离，以保护您的数据安全。

您可以通过向[向导模式配置](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/向导模式配置.md#)和[脚本模式配置](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/脚本模式配置.md#)配置同步任务。

**说明：** 如果是在VPC环境下的HybridDB for MySQL，需要注意以下问题。

-   自建的MySQL数据源
    -   不支持测试连通性，但仍支持配置同步任务，创建数据源时单击确认即可。
    -   必须使用自定义调度资源组运行对应的同步任务，请确保自定义资源组可以连通您的自建数据库，详情请参见[（仅一端不通）数据源网络不通的情况下的数据同步](intl.zh-CN/使用指南/数据集成/最佳实践/（仅一端不通）数据源网络不通的情况下的数据同步.md#)和[（两端都不通）数据源网络不通的情况下的数据同步](intl.zh-CN/使用指南/数据集成/最佳实践/（两端都不通）数据源网络不通的情况下的数据同步.md#)。
-   对于通过实例ID创建的HybridDB for MySQL数据源，您无需选择网络环境，系统自动根据您填写的HybridDB for MySQL实例信息进行判断。

## 操作步骤 {#section_rgw_ftr_5fb .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](images/32067_zh-CN.jpeg)

3.  在新建数据源弹出框中，选择数据源类型为**阿里云数据源（HybridDB）**。
4.  填写HybridDB for MySQL数据源的各配置项。

    ![](images/32068_zh-CN.jpeg)

    |配置|说明|
    |:-|:-|
    |**数据源类型**|当前选择的数据源类型为阿里云数据源（HybridDB）。|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**实例ID**|您可进入HybridDB for MySQL管控台查看相关的实例ID。|
    |**主账号ID**|您可在HybridDB for MySQL管控台安全设置中查看相应的信息。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/62182/155323505132071_zh-CN.png)

|
    |**用户名/密码**|数据库对应的用户名和密码。|

5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

**说明：** 您需要先添加白名单才能连接成功，详情请参看[添加白名单](intl.zh-CN/使用指南/数据集成/常见配置/添加白名单.md#)文档。

## 测试连通性说明 {#section_l5s_ctr_5fb .section}

-   经典网络下，能够提供测试连通性能力。
-   专有网络以添加实例ID形式能够添加成功，提供相关反向代理功能。

## 后续步骤 {#section_utj_syr_5fb .section}

现在，您已经学习了如何配置HybridDB for MySQL数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置HybridDB for MySQL插件，详情请参见[配置HybridDB for MySQL Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置HybridDB for MySQL Reader.md#)和[配置HybridDB for MySQL Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置HybridDB for MySQL Writer.md#)。

