# Merge node {#concept_kmn_4zn_fgb .concept}

This topic describes the merge node concept, and how to create a merge node and define the merging logic. It also shows you the scheduling configuration and operation details of the merge node through a practical case.

## Concept {#section_bsr_yks_ggb .section}

-   The merge node is a type of logical control family nodes in DataStudio.
-   The merge node can merge the running states of upstream nodes, and aims to solve the issues of dependency mounting and running trigger of downstream nodes of branch nodes.
-   The current logical definition of merge node does not support selecting nodes that are in the running state, but supports merging multiple downstream nodes of the branch nodes, so that more downstream nodes can be mounted to the merge node as a dependency.

For example, the branch node C defines two logically exclusive branches C1 and C2. Different branches use different logic to write to the same MaxCompute table. If the downstream node B depends on the output of this MaxCompute table, and must use the merge node J to merge branches first. Then add merge node J to the upstream dependency of B. If B is mounted directly under C1 and C2, at any given time one of the branch nodes will fail to run because it does not meet branch conditions. B cannot be triggered by the schedule to run.

## Create a merge node {#section_uzg_bls_ggb .section}

**Merge Node** is located in the **Control** class directory of the new node menu, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/155253271235412_en-US.png)

## Define the merge logic {#section_z3q_zls_ggb .section}

To add a merge branch, click Add. You can enter the output name or the parent node output table name, and view records under the merge condition,. The execution results will display the running status. Currently, there are only two running states: Successful, Branch Not Running, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/155253271235415_en-US.png)

The scheduling attribute of the merge node is shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/155253271235418_en-US.png)

## An example of the merge node {#section_adn_rns_ggb .section}

In the downstream node, you can define the branch direction under different conditions by selecting the corresponding branch node output after adding the branch node as the upstream node. For example, in the business process shown in the figure below,**Branch\_1** and **Branch\_2** are both downstream nodes of the branch node.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/155253271235420_en-US.png)

Branch\_1 depends on the output of 'autotest.fenzhi121902\_1', as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/155253271235421_en-US.png)

Branch\_2 depends on the output of 'autotest.fenzhi121902\_2', as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/155253271235422_en-US.png)

The scheduling attribute of the merge node is shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82218/155253271235424_en-US.png)

## Run the task {#section_edt_mps_ggb .section}

When the branch meets the specified condition, select the downstream node of the branch to run. You can view the run details in the **Running Log**.

When the branch does not meet the condition and does not select the downstream node of the branch to run. You can view the node that is set to 'skip' in the **Running Log**.

The downstream node of the merge node is running normally.

