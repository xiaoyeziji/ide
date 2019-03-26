# Performing a task at a specific time with branch node {#concept_djk_4gs_lgb .concept}

## Background of branch nodes {#section_c3m_pgs_lgb .section}

During the daily use of DataWorks, you may often encounter the following problem: I have a node that needs to be executed on the last day of each month. How should I set it up?

Answer: Before the [branch node](../../../../../reseller.en-US/User Guide/Data development/Node type/Branch node.md#) appears, the Cron expression can not express this scene, so it is temporarily unavailable to support.

Now, DataWorks [officially supports branch nodes](../../../../../reseller.en-US/Product Introduction/Version history.md#). With branch nodes, we can apply the switch-case programming model to perfectly meet the above requirements.

## Branch nodes and other control nodes {#section_qcb_drs_lgb .section}

On the Data Development page, you can see the various **control nodes** currently supported by DataWorks, including assignment nodes, branch nodes, merge nodes and so on.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244037235_en-US.png)

The functions of various types of control nodes:

-   Pass its own results to the downstream **assignment node**:

    The [assignment node](../../../../../reseller.en-US/User Guide/Data development/Node type/Assignment node.md#) reuses the characteristics that the [node context](../../../../../reseller.en-US/User Guide/Data development/Scheduling configuration/Node context.md#) depends. Based on the two existing constant/variable node context, the assignment node comes with a custom context output. DataWorks captures the select result or the print result of the assignment node. This result is used as the value of the context output parameter in the form of outputs for reference by downstream nodes.

-   Determine which downstream **branch nodes** are normally executed:

    The [branch node](../../../../../reseller.en-US/User Guide/Data development/Node type/Branch node.md#) reuse the characteristics of the [input and output](../../../../../reseller.en-US/User Guide/Data development/Scheduling configuration/Dependencies.md#) set in the dependencies on DataWorks.

    For common nodes, the output of the node is only a globally unique string. When the downstream needs to set dependencies, searching for this globally unique string as input to the node can be hung into the list of downstream nodes.

    However, for a branch node, we can associate a condition for each output: when the downstream set dependency, we can selectively use the output of a certain condition as the output of the branch node. In this way, when the node becomes the downstream of the branch node, it is also associated with the condition of the branch node: that is, the condition is satisfied, and the downstream corresponding to the output is executed normally; the other downstream nodes corresponding to the output that does not meet the criteria are set to run empty.

-   The **merge node** that is normally scheduled regardless of whether the upstream performs normally:

    For branches that are not selected by the branch node, DataWorks puts all node instances on this branch link as empty run instances. That is to say, once an upstream of a certain instance is running empty , this instance itself becomes empty.

    Dataworks can currently prevent this empty run attribute from being passed without restriction by [merge node](../../../../../reseller.en-US/User Guide/Data development/Node type/Merge node.md#): for a merge node instance, no matter how many empty run instances its upstream has, it will succeed directly and will no longer leave the downstream empty running.


From the figure below, you can see the logical relationship of the dependency tree in the presence of a branch node.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244037237_en-US.png)

-   **ASN**: an [assignment node](../../../../../reseller.en-US/User Guide/Data development/Node type/Assignment node.md#), which is used to calculate more complex situations to prepare for branch node conditional selection.
-   **X/Y**: branch nodes, they are downstream of the assignment node ASN, and make branch selection according to the output of the assignment node. As shown in the green line in the figure, the node X selects the left branch, the node Y selects the two branches on the left:
    -   The node **A/C** are executed normally because they are downstream of the output selected by the node X/Y.
    -   Although the node **B** is downstream of the branch selected by the node Y, since the node X does not select this output, the node B is set to run empty.
    -   The node **E** is not selected by node Y, so even if there is an ordinary upstream named node Z, it is also set to run empty.
    -   The upstream node E of the node**G** runs empty, so even if the node C/F are both executed normally, the node E also runs empty.
    -   How can the empty running properties no longer be passed down?

        **JOIN node** is a **merge node**. Its special function is to stop the transfer of empty run properties. You can see that because the node D is downstream of the JOIN node, the empty run attribute of the node B is blocked, and the node D can start running normally.


By using branch nodes to cooperate with other control nodes, we can satisfy the requirement scenario where a node only runs on the last day of each month.

## Use a branch node {#section_e5l_4ts_lgb .section}

**Define task dependencies**

First you need to define a set of task dependencies:

1.  The root assignment node calculates whether the current day is the last day of the month by timing SKYNET\_CYCTIME. If it is, the output is 1, and if it is not, the output is 0. This output is captured by DataWorks and passed to the downstream.
2.  The branch node defines the branch according to the output of assignment node.
3.  The two shell nodes are hung under the branch node and perform different branch logic.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244037238_en-US.png)

**Define assignment nodes**

The assignment node comes with an outputs when it is new, the code for the assignment node supports three kinds of SQL/SHELL/Python.

-   For SQL types, DataWorks captures the SQL of the last SELECT statement as the value of outputs.
-   For SHELL/Python types, DataWorks captures the last line of standard output as the value of outputs.

In this article, the Python type is used as the code for the assignment node, and the scheduling properties and code settings are as follows.

-   The code is as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244037241_en-US.png)

-   Schedule configuration

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244037242_en-US.png)


**Define branches**

Branch nodes can define conditions with simple Python syntax expressions, each of which is bound to an output. This means that the downstream node under this output is executed when the condition is met, and the other nodes are set to run empty.

-   Schedule configuration

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244137243_en-US.png)

-   Branch configuration

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244137244_en-US.png)

-   Schedule configuration generates output of conditional bindings

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244137245_en-US.png)


**Hang the execution task nodes under different branches**

Finally, it is important to note when setting dependencies on the nodes that actually perform tasks: you can see that the branch node already has three outputs, according to the logic of setting dependencies in the past, any one of these three outputs can be regarded as input; however, since the output of the branch node is now associated with the condition, it should be carefully selected.

-   Node dependencies performed on the last day of each month

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244137246_en-US.png)

-   Node dependencies performed at other times of each month

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244237247_en-US.png)


**Result verification**

Once completing all of the above configuration, you can submit and publish the task. After publishing, you can perform [patch data](../../../../../reseller.en-US/User Guide/O&M Center/Task O&M/PatchData.md#) to test the effect: the business date 2018-12-30 and 2018-12-31 are selected , that is, the timing is 2018-12-31 and 2019-01-01 respectively, so that the first batch of patch data should trigger the logic of "last day", the second batch triggers the logic of "non-last day". We look at the difference between the two.

**Business date 2018-12-30 \(timing 2018-12-31\)**

-   Branch selection results of branch node

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244237248_en-US.png)

-   The node "RunOnLast" is executed normally.
-   The node "RunExceptLast" is set to run empty.

**Business date 2018-12-31 \(timing 2019-01-01\)**

-   Branch selection results of branch node

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103578/155359244237252_en-US.png)

-   The node "RunOnLast" is set to run empty.
-   The node "RunExceptLast" is executed normally.

## Summary {#section_xsb_4ws_lgb .section}

Based on the branch node, you have achieved the goal which **execute on last day of each month**. Of course, this is the easiest way to use a branch node. By using an assignment node with a branch node, you can combine a variety of conditions to meet your business needs.

Finally, the main points of using branch nodes are reviewed.

-   DataWorks captures the last SELECT statement or the last line of the standard output stream of the assignment node as the output of an assignment node for downstream references.
-   Each output of the branch node is associated with the condition, and the downstream branch node is used as the upstream. It is necessary to understand the meaning of the conditions associated with each output before selecting.
-   Unselected branches are set to run empty, and the empty run properties are passed down until the merge node is encountered.
-   In addition to blocking the empty run properties, the merge node has more powerful features to wait for your mining.

