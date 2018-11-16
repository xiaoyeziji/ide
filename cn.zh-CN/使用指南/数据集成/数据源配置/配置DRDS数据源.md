# 配置DRDS数据源 {#concept_lmn_xc1_p2b .concept}

DRDS（分布式RDS）数据源提供了读取和写入DRDS双向通道的能力，可以通过向导模式和脚本模式配置同步任务。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16200/15423323187532_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**DRDS**。
4.  配置DRDS数据源的各个信息项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16200/15423323197533_zh-CN.png)

    配置项说明如下：

    -   **数据源类型**：选择阿里云数据库（DRDS）。
    -   **数据源名称**：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过60个字符。
    -   **数据源描述**：对数据源进行简单描述，不得超过80个字符。
    -   **实例ID**：您可以进入DRDS管控台查看相关实例ID。
    -   **主账号ID**：您可在DRDS管控台安全设置中查看相应的信息。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16200/154233231932116_zh-CN.png)

5.  数据库名：填写数据库对应的名称。
6.  用户名/密码：数据库对应的用户名和密码。
7.  单击**测试连通性**。
8.  测试连通性通过后，单击**确定**。

    提供测试连通性能力，可以判断输入的信息是否正确 。


如果您选择的**数据源类型**为**有公网IP**，配置项说明如下。

![](images/32117_zh-CN.jpeg)

-   数据源类型：选择有公网IP。
-   数据源名称：数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。
-   数据源描述：对数据源进行简单描述，不得超过80个字符。
-   JDBC URL：JDBC连接信息，格式为`jdbc:mysql://serverIP:Port/database`。
-   用户名/密码：数据库对应的用户名和密码。
-   单击测试连通性。
-   测试连通性通过后，单击确定。

**说明：** 对于有公网IP的数据源，您需要先添加[白名单](cn.zh-CN/使用指南/数据集成/常见配置/添加白名单.md#)才能连接成功。

## 测试连通性说明 {#section_rrz_yc1_p2b .section}

-   经典网络下，能够提供测试连通性能力，可以判断输入的JDBC URL，用户名/密码是否正确。
-   专有网络和无公网IP，目前不支持数据源连通性测试，直接单击**确认**。

## 后续步骤 {#section_dqv_5d1_p2b .section}

现在，您已经学习了如何配置DRDS数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置DRDS Writer插件。详情请参见[配置DRDS Writer](cn.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置DRDS Writer.md#)和[配置DRDS Reader](cn.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置DRDS Reader.md#)。

