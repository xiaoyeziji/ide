# 配置FTP数据源 {#concept_n1l_d21_p2b .concept}

FTP数据源提供了读取和写入FTP双向通道的能力，可以通过向导模式和脚本模式配置同步任务。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15421908077534_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**FTP**。
4.  配置FTP数据源的各个信息项。

    新建FTP数据源时，有以下两种数据源类型，您可根据自身情况进行选择。

    -   有公网IP

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15421908087535_zh-CN.png)

        配置项说明如下：

        -   数据源类型：有公网IP。
        -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过60个字符。
        -   数据源描述：对数据源进行简单描述，不得超过80个字符。
        -   Portocol：目前仅支持FTP和SFTP协议。
        -   Host：对应FTP主机的IP地址。
        -   Port：若选择的是FTP协议，则端口默认为21。若选择的是SFTP协议，则端口默认为22。
        -   用户名/密码：访问该FTP服务的账号密码。
    -   无公网IP

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15421908087536_zh-CN.png)

        配置项说明如下：

        -   数据源类型：无公网IP。选择此类型的数据源需要使用自定义调度资源才能进行同步，您可单击**帮助手册**查看详情。
        -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过60个字符。
        -   数据源描述：对数据源进行简单描述，不得超过80个字符。
        -   资源组：可以用于执行同步任务，一般添加资源组时可以绑定多台机器。详情请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)。
        -   Portocol：目前仅支持FTP和SFTP协议。
        -   Host：对应FTP主机的IP地址。
        -   Port：若选择的是FTP协议，则端口默认为21。若选择的是SFTP协议，则端口默认为22。
        -   用户名/密码：访问该FTP服务的账号密码。
5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

    提供测试连通性能力，可以判断输入的信息是否正确 。


## 测试连通性说明 {#section_rrz_yc1_p2b .section}

-   经典网络下，能够提供测试连通性能力，可以判断输入的Host，Port，用户名/密码是否正确。
-   专有网络目前不支持数据源连通性测试，直接单击**确认**。

## 后续步骤 {#section_dqv_5d1_p2b .section}

现在，您已经学习了如何配置FTP数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置FTP Writer插件。详情请参见[配置FTP Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置FTP Writer.md#)和[配置FTP Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置FTP Reader.md#)。

。

