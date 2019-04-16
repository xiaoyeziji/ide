# ODPS SQL node {#concept_dn4_5fl_q2b .concept}

The ODPS SQL adopts a syntax similar to that of SQL, and is applicable to a distributed scenario, where the amount of data is massive \(TB-level\) with low real-time requirement. It is an OLAP application oriented to throughput. ODPS SQL is recommended if a business needs to handle thousands or tens of thousands of transactions because it takes a long time to complete the process from preparation to submission of a job.

1.  Create a business flow.

    Click **Manual Business Flow** in the left-side navigation pane, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15525336637961_en-US.png)

2.  Create ODPS SQL node.

    Right-click **Data Development**, and select **Create Data Development Node** \> **ODPS SQL**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16321/15525336638022_en-US.png)

3.  Edit the node code.

    For more information about the syntax of the SQL statements, see [MaxCompute SQL](https://www.alibabacloud.com/help/doc-detail/27860.htm) statements.

4.  Node scheduling configuration.

    Click the **Schedule** on the right of the node task editing area to go to the Node Scheduling Configuration page. For more information, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Basic attributes.md#).

5.  Submit the node.

    After the configuration is completed, click **Save** in the upper-left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

6.  Publish a node task.

    For more information about the operation, see Release management.

7.  Test in the production environment.

    For more information about the operation, see [Manual task](reseller.en-US/User Guide/O&M Center/Task list/Manual task.md#).


