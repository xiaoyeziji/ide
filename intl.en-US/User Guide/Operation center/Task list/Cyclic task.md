# Cyclic task {#concept_hk4_gvh_r2b .concept}

Cyclic Task: Tasks automatically triggered by the scheduling system.

Click the **Cycle Task**, default display the current landing responsibility person node.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16357/15390797668766_en-US.jpg)

As shown in the figure above, task nodes can be filtered, providing name search, responsible person, baseline and other conditional search.

Default displays the name of the current task, modification date, task type, responsible person, scheduling type, resource group, alarm settings, operations. The operation button contains the following functions:

-   DAG diagram: the DAG diagram of this node is displayed.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16357/15390797678767_en-US.jpg)

-   Test: to test the current node.
-   Data complement: data complement for the current node, see [Data completion instances](reseller.en-US/User Guide/Operation center/Task O&M/PatchData.md#).
-   More: including node status modification and more functions.

More functions:

-   Pause \(freeze\): Set the current node to a pause \(freeze\) state and stop scheduling. When the node state is paused, the ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16357/153907976711847_en-US.png) icon appears after the node name.
-   Restore \(thaw\): restore the suspend \(frozen\) node to schedule.
-   View instances: view the cycle instance of this node.
-   Add alarm: configure alarm for node
-   Modify the responsible person: modify the person responsible for the node
-   Modify resource group: modify the resource group of nodes \(if there are multiple resource groups in the project\).
-   Configuring quality monitoring: configuring DQC data quality and checking data.
-   Look at blood ties: see the kinship map of the node.
-   Upstream and downstream: this node in the DAG diagram, the right-click node will pop up the operable window. The detailed operation is as follows:


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16357/15390797678768_en-US.jpg)

-   Expanding parent / child nodes: When a workflow has three or more nodes, the operation and maintenance center will automatically hide the nodes when displaying tasks. Users can see more node dependencies by expanding the parent-child hierarchy. The larger the hierarchy, the more comprehensive the display.
-   View node code: You can view the current code of the node.
-   Edit nodes: You can jump to the page to edit the node.
-   Testing: A prompt window pops up to edit the instance name and you can select the business date, which automatically jumps to the test instance page.
-   Complement data: you can choose "include this node" and "include this node and downstream node".
-   Pause \(freeze\): place the current node into a pause \(freeze\) state and stop scheduling.
-   Restore \(thaw\): restore the suspend \(frozen\) node to schedule.
-   View instances: view the cycle instance of this node.
-   View kinship : see the kinship map of the node.

