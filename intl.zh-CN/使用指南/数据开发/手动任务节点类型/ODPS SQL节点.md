# ODPS SQL节点 {#concept_dn4_5fl_q2b .concept}

ODPS SQL采用类似SQL的语法，适用于海量数据（TB级）但实时性要求不高的分布式处理场景。它是OLAP应用，主要面向吞吐量。因为每个作业从前期准备到提交等阶段都需要花费较长时间，因此若要求处理几千至数万笔事务的业务，可以使用ODPS SQL顺利完成。

1.  新建业务流程。

    单击左侧导航栏中的**手动业务流程**，选择**新建业务流程**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15554935117961_zh-CN.png)

2.  新建ODPS SQL节点。

    右键单击**数据开发**，选择**新建数据开发节点** \> **ODPS SQL**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16321/15554935118022_zh-CN.png)

3.  编辑节点代码。

    编写符合语法的ODPS SQL代码，SQL语法请参见[MaxCompute SQL](../../../../intl.zh-CN/用户指南/SQL/SQL概述.md#)模块。

4.  提交节点任务。

    完成调度配置后，单击左上角的**保存**，提交（提交并解锁）到开发环境。

5.  发布节点任务。

    具体操作请参见[发布管理](intl.zh-CN/使用指南/数据开发/发布管理/任务发布.md#)。

6.  在生产环境测试。

    具体操作请参见[手动任务](intl.zh-CN/使用指南/运维中心/任务列表/手动任务.md#)。


