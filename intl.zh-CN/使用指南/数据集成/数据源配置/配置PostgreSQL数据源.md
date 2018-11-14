# 配置PostgreSQL数据源 {#concept_a1x_krb_p2b .concept}

PostgreSQL关系型数据库数据源提供了读取和写入PostgreSQL双向通道的能力，可以通过向导模式和脚本模式配置同步任务。

**说明：** 如果是在VPC环境下的PostgreSQL，需要注意以下问题。

-   自建的PostgreSQL数据源
    -   不支持测试连通性，但仍支持配置同步任务，创建数据源时单击**确认**即可。
    -   必须使用自定义调度资源组运行对应的同步任务，请确保自定义资源组可以连通您的自建数据库，详情请参见[（仅一端不通）数据源网络不通的情况下的数据同步](intl.zh-CN/使用指南/数据集成/最佳实践/（仅一端不通）数据源网络不通的情况下的数据同步.md#)和[（两端都不通）数据源网络不通的情况下的数据同步](intl.zh-CN/使用指南/数据集成/最佳实践/（两端都不通）数据源网络不通的情况下的数据同步.md#)。
-   通过RDS创建的PostgreSQL数据源

    您无需选择网络环境，系统自动根据您填写的RDS实例信息进行判断。


## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15421915017572_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**PostgreSQL**。
4.  配置PostgreSQL数据源的各个信息项。

    PostgreSQL数据源类型分为**阿里云数据库（RDS）**、**有公网IP**和**无公网IP**，您可根据自身情况进行选择。

    以新增**PostgreSQL** \> **阿里云数据库（RDS）**类型的数据源为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15421915017581_zh-CN.png)

    配置项说明如下：

    -   数据源类型：阿里云数据库（RDS）。
    -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过60个字符。
    -   数据源描述：对数据源进行简单描述，不得超过80个字符。
    -   RDS实例ID：您可进入RDS的管控台查看RDS的实例ID。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15421915017582_zh-CN.png)

        以新增**PostgreSQL** \> **有公网IP**类型的数据源为例。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15421915017584_zh-CN.png)

        配置项说明如下：

        -   数据源类型：有公网IP。
        -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过60个字符。
        -   数据源描述：对数据源进行简单描述，不得超过80个字符。
        -   JDBC URL：JDBC连接信息，格式为jdbc:postgresql://ServerIP:Port/Database。
        -   用户名/密码：数据库对应的用户名和密码。
    以新增**PostgreSQL** \> **无公网IP**类型的数据源为例。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16211/15421915017585_zh-CN.png)

    配置项说明如下：

    -   数据源类型：无公网IP。
    -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过60个字符。
    -   数据源描述：对数据源进行简单描述，不得超过80个字符。
    -   资源组：可以用于执行同步任务，一般添加资源组时可以绑定多台机器。详情请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。
    -   JDBC URL：JDBC连接信息，格式为jdbc:postgresql://ServerIP:Port/Database。
    -   用户名/密码：数据库对应的用户名和密码。
5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

## 测试连通性说明 { .section}

-   经典网络下，能够提供测试连通性能力，可以判断输入的JDBC URL、用户名/密码是否正确。
-   专有网络和无公网IP，目前不支持数据源连通性测试，直接单击**确定**。

## 后续步骤 {#section_jb1_tyb_p2b .section}

现在，您已经学习了如何配置PostgreSQL数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置PostgreSQL Writer插件。详情请参见[配置PostgreSQL Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置PostgreSQL Writer.md#)和[配置PostgreSQL Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置PostgreSQL  Reader.md#)。

