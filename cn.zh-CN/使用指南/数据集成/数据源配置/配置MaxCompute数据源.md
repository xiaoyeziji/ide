# 配置MaxCompute数据源 {#concept_tms_vp1_p2b .concept}

大数据计算服务（MaxCompute，原名ODPS）为您提供了完善的数据导入方案，能够更快速地解决海量数据计算问题。MaxCompute数据源作为数据中枢，提供了读取和写入MaxCompute双向通道的能力，支持Reader和Writer插件。

**说明：** 

每个项目空间系统都将生成一个默认的数据源（odps\_first），对应的MaxCompute项目名称为当前项目空间对应的计算引擎MaxCompute项目名称。

默认数据源的AK可以点击右上方用户信息，在修改AccessKey信息处进行切换，但需注意：

1.  只能从主账号AK切换到主账号AK。
2.  切换时当前必须没有任务在运行中（数据集成或数据开发等一切和DataWorks相关的任务）。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16204/154763678736951_zh-CN.jpg)

    您自行添加的MaxCompute数据源可以使用子账号AK。


## 操作步骤 {#section_jy4_q4v_42b .section}

1.  以项目管理员身份进入[DataWorks管理控制台](https://workbench.data.aliyun.com/console)，单击对应项目操作栏中的**进入数据集成**。
2.  单击**数据源** \> **新增数据源**，弹出支持的数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16204/15476367877542_zh-CN.png)

3.  在新建数据源弹出框中，选择数据源类型为**MaxCompute（ODPS）**。
4.  配置MaxCompute数据源的各个信息项。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16204/15476367877543_zh-CN.jpg)

    |配置|说明|
    |:-|:-|
    |**数据源名称**|数据源名称必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**ODPS Endpoint**|默认只读，从系统配置中自动读取。|
    |**ODPS Item Name**|MaxCompute（ODPS）项目名称。|
    |**ODPS项目名称**|对应的MaxCompute Project标识。|
    |**AccessID/AceessKey**|访问密匙（AccessKeyID和AccessKeySecret）相当于登录密码。|

5.  单击**测试连通性**。
6.  测试连通性通过后，单击**确定**。

    提供测试连通性能力，可以判断输入的Project/AK信息是否正确 。


## 后续步骤 {#section_dqv_5d1_p2b .section}

现在，您已经学习了如何配置MaxCompute数据源，您可以继续学习下一个教程。在该教程中您将学习如何通过配置MaxCompute Writer插件。详情请参见[配置MaxCompute Writer](cn.zh-CN/使用指南/数据集成/作业配置/配置Writer插件/配置MaxCompute Writer.md#)和[配置MaxCompute Reader](cn.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置MaxCompute  Reader.md#)。

