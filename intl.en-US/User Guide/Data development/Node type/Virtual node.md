# Virtual node {#concept_iqn_dfq_p2b .concept}

A virtual node is a control node that does not generate any data. Generally, it is used as the root node for overall planning of nodes in the workflow.

**Note:** The final output table of a workflow contains multiple branch input tables. Virtual nodes are usually used if these input tables do not have dependency between them.

## Create a virtual node task {#section_xnx_dhq_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15389902837651_en-US.png)

2.  Right-click **Data Development**, and select **Create Data Development Node** \> **Virtual Node**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16298/15389902837816_en-US.png)

3.  Set the node type to **Virtual Node**, enter the node name, select the target folder, and click **Submit**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16298/15389902837817_en-US.png)

4.  Edit the node code: You do not need to edit the code of a virtual node.
5.  Node scheduling configuration.

    Click the **Schedule** on the right of the node task editing area to go to the node scheduling configuration page. For more information, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

6.  Submit the node.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

7.  Publish a node task.

    For more information about the operation, see Release management.

8.  Test in the production environment.

    For more information about the operation, see [Cyclic task](reseller.en-US/User Guide/Operation center/Task list/Cyclic task.md#).


