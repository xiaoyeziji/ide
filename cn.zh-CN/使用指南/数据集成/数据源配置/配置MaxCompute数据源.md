# 配置MaxCompute数据源 {#concept_tms_vp1_p2b .concept}

大数据计算服务（MaxCompute，原名 ODPS）为您提供了完善的数据导入方案，能够更快速地解决海量数据计算问题。MaxCompute数据源作为数据中枢，提供了读取和写入MaxCompute双向通道的能力，支持Reader和Writer插件。

**说明：** 每个项目空间系统都将生成一个默认的数据源（odps\_first），对应的MaxCompute项目名称为当前项目空间对应的计算引擎MaxCompute项目名称。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入工作区**。
2.  单击顶部菜单栏中的**数据集成**，导航至**数据源**页面。
3.  单击**新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16204/15367208187542_zh-CN.png)

4.  在新建数据源弹出框中，选择数据源类型为**MaxCompute（ODPS）**。
5.  配置MaxCompute数据源的各个信息项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16204/15367208187543_zh-CN.jpg)

    配置项说明如下：

    -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过30个字符。
    -   数据源描述： 对数据源进行简单描述，不得超过80个字符。
    -   ODPS Endpoint：默认只读，从系统配置中自动读取。
    -   ODPS项目名称：对应的MaxCompute Project标识。
    -   AccessID/AceessKey：[访问密匙](https://www.alibabacloud.com/help/zh/doc-detail/53045.htm)（AccessKeyID和AccessKeySecret）相当于登录密码。
6.  单击**测试连通性**。
7.  测试连通性通过后，单击**确定**。

    提供测试连通性能力，可以判断输入的Project/AK信息是否正确 。


## 后续步骤 {#section_dqv_5d1_p2b .section}

现在，您已经学习了如何配置MaxCompute数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置MaxCompute Writer插件。详情请参见[配置MaxCompute Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置MaxCompute Writer.md#)和[配置MaxCompute Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置MaxCompute  Reader.md#)。

