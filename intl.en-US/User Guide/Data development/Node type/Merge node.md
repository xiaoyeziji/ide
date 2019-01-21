# Merge node {#concept_kmn_4zn_fgb .concept}

This article introduces the concept of merge node, how to create merge node and define merging logic. It also shows you the scheduling configuration and operation details of the merge node through a practical case.

## Concept {#section_bsr_yks_ggb .section}

-   The merge node is one of the logical control family nodes provided in DataStudio.
-   The Merge node can merge the running states of upstream nodes, aiming to solve the problem of dependency mounting and running triggering of downstream nodes of branch nodes.
-   The current logical definition of merge node does not support selecting the running state of the node, but only supports merging multiple downstream nodes of branch nodes into a successful merge, so that the more downstream nodes can directly mount the merge node as a dependency.

For example, branch node C defines two logically exclusive branches C1 and C2. Different branches use different logic to write to the same MaxCompute table. If downstream node B depends on the output of this MaxCompute table, it must use merge node J to merge branches first, then add merge node J as the upstream dependency of B. If B is mounted directly under C1 and C2, at any time, C1 and C2, one of them will always fail to run because of unsatisfactory branching conditions, and B can not be triggered by the schedule to run.

## Create a merge Node {#section_uzg_bls_ggb .section}

**Merge Node** is located in the **Control** class directory of the new node menu, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/154804708135412_en-US.png)

## Define the merge Logic {#section_z3q_zls_ggb .section}

Add merge branch. You can input the output name or output table name of the parent node, click add, you can see records under merge condition, and the execution results will show you the running status, currently there are only two running states: Successful, Branch not running, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/154804708235415_en-US.png)

The scheduling attribute of the merge node is shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/154804708235418_en-US.png)

## An example of merge node {#section_adn_rns_ggb .section}

In the downstream node, after adding the branch node as the upstream node, you can define the branch direction under different conditions by selecting the corresponding branch node output. For example, in the business process shown in the figure below,**Branch\_1** and **Branch\_2** are both downstream nodes of the branch node.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/154804708235420_en-US.png)

Branch\_1 depends on the output of 'autotest.fenzhi121902\_1', as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/154804708235421_en-US.png)

Branch\_2 depends on the output of 'autotest.fenzhi121902\_2', as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/154804708235422_en-US.png)

The scheduling attribute of the merge node is shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/154804708235424_en-US.png)

## Run the task {#section_edt_mps_ggb .section}

When the branch condition is satisfied and select the downstream node of the branch to run. You can see the details of the run in the **Running Log**.

When the branch condition is not satisfied and do not select the downstream node of the branch to run. You can see that the node is set to 'skip' in the **Running Log**.

The downstream node of the merge node is running normally.

