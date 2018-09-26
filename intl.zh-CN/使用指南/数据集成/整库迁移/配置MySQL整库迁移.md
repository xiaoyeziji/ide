# 配置MySQL整库迁移 {#concept_j41_l1v_q2b .concept}

本文将通过实践操作，为您介绍如何使用整库迁移功能，将MySQL数据整库迁移到MaxCompute。

整库迁移是为了提升用户效率、降低用户使用成本的一种快捷工具，它可以快速把MySQL数据库内所有表一并上传到MaxCompute，关于整库迁移的详细介绍请参见[整库迁移概述](intl.zh-CN/使用指南/数据集成/整库迁移/整库迁移概述.md#)。

## 操作步骤 {#section_fn2_s1v_q2b .section}

1.  登录到[DataWorks\>数据集成控制台](https://account.alibabacloud.com/login/login.htm)，单击左侧的**离线同步** \> **数据源**，进入数据源管理页面。
2.  单击右上角的**新增数据源**，添加一个面向整库迁移的MySQL数据源clone\_databae。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16268/15367269288560_zh-CN.png)

3.  单击**测试连通性**，验证数据源访问正确无误后，确认并保存该数据源。
4.  新增数据源成功后，即可在数据源列表中看到新增的MySQL数据源clone\_databae。单击对应MySQL数据源后的**整库迁移**，即可进入对应数据源的整库迁移功能页面。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16268/15367269288562_zh-CN.png)

    整库迁移页面主要分3块功能区域。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16268/15367269288563_zh-CN.png)

    -   待迁移表筛选区：此处将MySQL数据源clone\_databae下的所有数据库表以表格的形式展现出来，您可根据实际需要批量选择待迁移的数据库表。
    -   高级设置：此处提供了MySQL数据表和MaxCompute数据表的表名称、列名称、列类型的映射转换规则。
    -   迁移模式、并发控制区：此处可以控制整库迁移的模式（全量、增量）、并发度配置（分批上次、整批上传）、提交迁移任务进度状态信息等。
5.  单击**高级设置**，您可根据具体的需求选择转换规则。例如MaxCompute端建表时统一增加了ods\_这一前缀。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16268/15367269288564_zh-CN.png)

6.  在迁移模式、并发控制区中，选择同步方式为每日增量，并配置增量字段为gmt\_modified，数据集成默认会根据您选择的增量字段生成具体每个任务的增量抽取where条件，并配合DataWorks调度参数比如$\{bdp.system.bizdate\}形成针对每天的数据抽取条件。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16268/15367269298565_zh-CN.png)

    数据集成抽取MySQL库表的数据是通过JDBC连接远程MySQL数据库，并执行相应的SQL语句，将数据从MySQL库中Select出来。由于是标准的SQL抽取语句，可以配置Where子句控制数据范围。此处您可以查看到增量抽取的Where条件如下所示：

    ```
    STR_TO_DATE('${bdp.system.bizdate}', '%Y%m%d') <= gmt_modified AND gmt_modified < DATE_ADD(STR_TO_DATE('${bdp.system.bizdate}', '%Y%m%d'), interval 1 day)
    ```

    为了对源头MySQL数据源进行保护，避免同一时间点启动大量数据同步作业带来数据库压力过大，此处选择分批上传模式，并配置从每日0点开始，每1小时启动3个数据库表同步。

    最后单击**提交任务**，这里可以看到迁移进度信息，以及每一个表的迁移任务状态。

7.  单击a1表对应的迁移任务，跳转至数据集成的任务开发页面。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16268/15367269298566_zh-CN.png)

    由上图可以看到源头a1表对应的MaxCompute表odsa1创建成功，列的名字和类型也符合之前映射转换配置。在左侧目录树clone\_database目录下，会有对应的所有整库迁移任务，任务命名规则是mysql2odps源表名，如上图红框部分所示。


此时便完成了将一个MySQL数据源clone\_databae整库迁移到MaxCompute的工作。这些任务会根据配置的调度周期（默认天调度）被调度执行，您也可以使用DataWorks调度补数据功能完成历史数据的传输。通过**数据集成** \> **整库迁移**功能可以极大减少您初始化上云的配置、迁移成本。

整库迁移a1表任务执行成功的日志如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16268/15367269298567_zh-CN.png)

