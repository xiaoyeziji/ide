# 配置MySQL数据源 {#concept_nvl_451_p2b .concept}

MySQL关系型数据库数据源提供了读取和写入MySQL双向通道的能力，可以通过向导模式和脚本模式配置同步任务。

**说明：** 

标准模式的工作空间支持[数据源隔离](intl.zh-CN/使用指南/数据集成/数据源配置/数据源隔离.md#)功能，您可以分别添加开发环境和生产环境的数据源并进行隔离，以保护您的数据安全。

如果是在VPC环境下的MySQL，需要注意以下问题：

-   自建的MySQL数据源
    -   不支持测试连通性，但仍支持配置同步任务，创建数据源时单击**确认**即可。
    -   必须使用自定义调度资源组运行对应的同步任务，请确保自定义资源组可以连通您的自建数据库，详情请参见[（仅一端不通）数据源网络不通的情况下的数据同步](intl.zh-CN/使用指南/数据集成/最佳实践/（仅一端不通）数据源网络不通的情况下的数据同步.md#)和[（两端都不通）数据源网络不通的情况下的数据同步](intl.zh-CN/使用指南/数据集成/最佳实践/（两端都不通）数据源网络不通的情况下的数据同步.md#)。
-   通过RDS创建的MySQL数据源

    您无需选择网络环境，系统自动根据您填写的RDS实例信息进行判断。


## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15532348277549_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**MySQL**。
4.  填写MySQL数据源的各配置项。

    MySQL数据源类型分为**阿里云数据库（RDS）**、**有公网IP**和**无公网IP**。

    以新增**MySQL** \> **阿里云数据库（RDS）**类型的数据源为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15532348277550_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源类型**|当前选择的数据源类型MySQL\>阿里云数据库（RDS）。|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**RDS实例ID**|您可以进入RDS管控台查看RDS的实例ID。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15532348277551_zh-CN.png)

|
    |**RDS实例主账号ID**|您可在RDS管控台安全设置中查看相应的信息。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15532348277553_zh-CN.png)

|
    |**用户名/密码**|数据库对应的用户名和密码。|

    **说明：** 您需要先添加RDS白名单才能连接成功，详情请参[添加白名单](intl.zh-CN/使用指南/数据集成/常见配置/添加白名单.md#)见。

    以新增**MySQL** \> **有公网IP**类型的数据源为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15532348277554_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源类型**|有公网IP。|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**JDBC URL**|JDBC连接信息，格式为jdbc:mysql://ServerIP:Port/Database。|
    |**用户名/密码**|数据库对应的用户名和密码。|

    以新增**MySQL** \> **无公网IP**类型的数据源为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16207/15532348277555_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源类型**|无公网IP。|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**资源组**|可以用于执行同步任务，一般添加资源组时可以绑定多台机器。详情请参见[新增任务资源](intl.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。|
    |**JDBC URL**|JDBC连接信息，格式为jdbc:mysql://ServerIP:Port/Database。|
    |**用户名/密码**|数据库对应的用户名和密码。|

5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

## 测试连通性说明 {#section_eq3_lnb_p2b .section}

-   经典网络下，能够提供测试连通性能力，可以判断输入的JDBC URL，用户名/密码是否正确。
-   专有网络和无公网IP，目前不支持数据源连通性测试，直接单击**确认**。

## 后续步骤 {#section_bbg_jnb_p2b .section}

现在，您已经学习了如何配置MySQL数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置MySQL Writer插件。详情请参见[配置MySQL Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置MySQL Writer.md#)和[配置MySQL Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置MySQL Reader.md#)。

