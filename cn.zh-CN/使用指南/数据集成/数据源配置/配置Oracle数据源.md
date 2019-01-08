# 配置Oracle数据源 {#concept_jkd_5nb_p2b .concept}

Oracle关系型数据库数据源提供了读取和写入Oracle双向通道的能力，您可以通过向导模式和脚本模式配置同步任务。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15469262417556_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**Oracle**。
4.  配置Oracle数据源的各个信息项。

    Oracle数据源类型分为**有公网IP**和**无公网IP**，您可根据自身情况进行选择。

    以新增**Oracle** \> **有公网IP**类型的数据源为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15469262427557_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源类型**|有公网IP。|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**JDBC URL**|JDBC连接信息，格式为jdbc:oracle:thin:@ServerIP:Port:Database。|
    |**用户名/密码**|数据库对应的用户名和密码。|

    以新增**Oracle** \> **无公网IP**类型的数据源为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16208/15469262427558_zh-CN.png)

    |配置|说明|
    |:-|:-|
    |**数据源类型**|无公网IP，此种类型的数据源需要使用自定义调度资源才能进行同步，可单击**帮助手册**进行查看。|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**JDBC URL**|JDBC连接信息，格式为jdbc:oracle:thin:@ServerIP:Port:Database。|
    |**用户名/密码**|数据库对应的用户名和密码。|

5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

## 测试连通性说明 {#section_eq3_lnb_p2b .section}

-   经典网络下，能够提供测试连通性能力，可以判断输入的JDBC URL，用户名/密码是否正确。
-   专有网络和无公网IP，目前不支持数据源连通性测试，直接单击**确认**。

## 后续步骤 {#section_bbg_jnb_p2b .section}

现在，您已经学习了如何配置Oracle数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置Oracle Writer插件。详情请参见[配置Oracle Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置Oracle Writer.md#)和[配置Oracle Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置Oracle Reader.md#)。

