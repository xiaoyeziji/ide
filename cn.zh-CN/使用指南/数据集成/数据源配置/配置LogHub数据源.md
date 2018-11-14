# 配置LogHub数据源 {#concept_hrr_q41_p2b .concept}

LogHub数据源作为数据中枢，提供了读取和写入LogHub双向通道的能力，支持Reader和Writer插件。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16203/15421908647540_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**LogHub**。
4.  配置LogHub数据源的各个信息项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16203/15421908647541_zh-CN.png)

    配置项说明如下：

    -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过30个字符。
    -   数据源描述： 对数据源进行简单描述，不得超过80个字符。
    -   LogHub Endpoint：一般格式为http://cn-shanghai.log.aliyun.com。详情请参见[服务入口](https://www.alibabacloud.com/help/doc-detail/29008.htm)。
    -   Project：输入对应的Project。
    -   AccessID/AceessKey：[访问密匙](https://www.alibabacloud.com/help/doc-detail/53045.htm)（AccessKeyID和AccessKeySecret）相当于登录密码。
5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

    提供测试连通性能力，可以判断输入的Project/AK信息是否正确 。


## 后续步骤 {#section_dqv_5d1_p2b .section}

现在，您已经学习了如何配置LogHub数据源，您可以继续学习下一个教程。在该教程中您将学习如何[配置LogHub Reader](intl.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置LogHub Reader.md#)和[配置LogHub Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置LogHub Writer.md#)。

