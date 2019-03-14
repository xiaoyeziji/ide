# Virtual node {#concept_iqn_dfq_p2b .concept}

A virtual node is a control node that does not generate any data. Generally, it is used as the root node for the overall workflow node planning.

**Note:** The final workflow output table contains multiple branch input tables. The virtual nodes are usually used if these input tables do not have any dependencies.

## Create a virtual node task {#section_xnx_dhq_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15525324597651_en-US.png)

2.  Right-click **Data Development**, and select **Create Data Development Node** \> **Virtual Node**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16298/15525324597816_en-US.png)

3.  Set the node type to **Virtual Node**, and enter the node name. Select the target folder, and click **Submit**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16298/15525324597817_en-US.png)

4.  Edit the node code: You do not need to edit the virtual node code.
5.  Node scheduling configuration.

    Click the **Schedule** on the right-side of the node task editing area to go to the Node Scheduling Configuration page. For more information about scheduling configuration, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Basic attributes.md#).

6.  Submit the node.

    After completing the configuration, click **Save** in the upper-left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

7.  Publish a node task.

    For more information about the operation, see Publish management.

8.  Test in the production environment.

    For more information about the operation, see [Cyclic task](reseller.en-US/User Guide/O&M Center/Task list/Cyclic task.md#).


