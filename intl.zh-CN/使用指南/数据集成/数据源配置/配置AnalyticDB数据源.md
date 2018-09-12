# 配置AnalyticDB数据源 {#concept_p3d_44v_42b .concept}

AnalyticDB（简称ADS） 提供了其他数据源向AnalyticDB写入的功能，但不能读取数据，支持数据集成中的向导模式和脚本模式。

## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入工作区**。
2.  单击顶部菜单栏中的**数据集成**，导航至**数据源**页面。
3.  单击**新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16197/15367326207524_zh-CN.png)

4.  在新建数据源弹出框中，选择数据源类型为AnalyticDB（ADS）。
5.  配置AnalyticDB数据源的各个信息项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16197/15367326207525_zh-CN.png)

    配置项说明如下：

    -   数据源名称：由英文字母、数字、下划线组成且需以字符或下划线开头，长度不超过60个字符。
    -   数据源描述：对数据源进行简单描述，不得超过80个字符 。
    -   连接Url：ADS连接信息，格式为：serverIP:Port。
    -   Schema：相应的ADS Schema信息。
    -   AccessID/AceessKey：[访问密匙](https://www.alibabacloud.com/help/doc-detail/53045.htm)（AccessKeyID和AccessKeySecret）相当于登录密码。
6.  单击**测试连通性**。
7.  测试连通性通过后，单击**确定**。

    提供测试连通性能力，可以判断输入的信息是否正确 。


## 后续步骤 { .section}

现在，您已经学习了如何配置ADS数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置ADS Writer插件。详情请参见[配置AnalyticDB（ADS） Writer](intl.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置AnalyticDB（ADS） Writer.md#)。

