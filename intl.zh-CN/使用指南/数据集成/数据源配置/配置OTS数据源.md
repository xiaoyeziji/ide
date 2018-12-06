# 配置OTS数据源 {#concept_ccz_rqb_p2b .concept}

表格存储（Table Store）是构建在阿里云飞天分布式系统之上的NoSQL数据存储服务，提供海量结构化数据的存储和实时访问。

**说明：** 如果您想对OTS有更深入的了解，请参见[OTS产品概述](https://www.alibabacloud.com/help/doc-detail/27280.htm)。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16210/15440615947569_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**OTS**。
4.  配置OTS数据源的各个信息项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16210/15440615947571_zh-CN.png)

    配置项说明如下：

    -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过60个字符。
    -   数据源描述：对数据源进行简单描述，不得超过80个字符。
    -   Endpoint：Table Store Server的Endpoint，格式为`http://yyy.com`,详情请参见[服务地址](https://www.alibabacloud.com/help/doc-detail/52671.htm)。
    -   Table Store实例ID：Table Store服务对应的实例ID。
    -   AccessID/AceessKey：[访问密匙](https://www.alibabacloud.com/help/doc-detail/53045.htm)（AccessKeyID和AccessKeySecret）相当于登录密码。
5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

## 测试连通性说明 {#section_eq3_lnb_p2b .section}

-   经典网络下，能够提供测试连通性能力，可以判断输入的Endpoint、AK信息是否正确。
-   专有网络目前不支持数据源连通性测试，直接单击**确认**。

## 后续步骤 {#section_x2g_1kp_y2b .section}

现在，您已经学习了如何配置OTS数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置OTS Reader插件。详情请参见[配置OTS Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置OTS Reader.md#)。

