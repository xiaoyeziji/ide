# PatchData {#concept_nrn_pxh_r2b .concept}

PatchData instances are generated during the completion of data for cyclic tasks, which allows O&M management of scheduled instance tasks such as viewing running status and terminating, re-running, and unfreezing tasks.

## Patch Data {#section_hsj_xdq_mgb .section}

Right click your Cycle Task, and you can shoose to **Patch Data**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/154781486337211_en-US.png)

You can choose to patch the data of the **Current node** or the **Current and Child node**. After that, you can choose if you want the Patch Data task can run in parallel.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/154781486337210_en-US.png)

## How to patch data for specific nodes in Combined Nodes {#section_c3z_2gq_mgb .section}

Combined Node comes from your work flow in DataWorks V1.0 . The following pictures show how to patch data for specific nodes in Combined Nodes.

1.  Right click your Combined Node's DAG and click **View Internal Nodes**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/154781486337651_en-US.png)

2.  Right click your upstream Internal Node and copy the Node ID.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/154781486337652_en-US.png)

3.  Search the ID and Patch Data.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/154781486337654_en-US.png)

4.  You can now patch data for specific nodes in Combined Nodes.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/154781486337655_en-US.png)


## Instance list {#section_i2k_xs3_r2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/15478148638837_en-US.jpg)

-   Instance name/DAG graph: You can open the DAG graph for this node to view the results of the Instance run.
-   Stop running: If the instance is running, click STOP to run the kill task.
-   Re-run: re-schedule this instance.
-   More: including node status modification and more functions.

Introduction to more features:

-   Re-run downstream: re-run the downstream task for this node.
-   Success: If the node fails to run, the node is successfully activated downstream.
-   Pause \(freeze\): sets the current node to a pause \(freeze\) State and stops scheduling, when the node state is suspended, an icon![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/15478148638856_en-US.png) appears after the node name.
-   Restore \(thaw\): restore the suspend \(frozen\) node to schedule.
-   Look at blood ties: see the ki-nship map of the node.

## DAG graph Introduction {#section_q3l_ft3_r2b .section}

Click the node name or dag map to open the DAG graph interface for this instance, right-click the node to see the operational features of this node.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/15478148638838_en-US.jpg)

-   Attributes: the attributes of this node are described, including schedule type, status, time, and so on.
-   Run log: this node is running or running log information.
-   Operation Log: The operation log for the node, including the records of node changes, replenishment data, and so on.
-   Code: Code edited by the node.

The right-click node function describes:

-   View running logs: Enter the Operations Log interface, where you can see information such as logview in the Operations Log.
-   View node code: You can view the current code of the node.
-   Edit nodes: You can jump to the data development page to edit the node.
-   View node impact: Enter the node information interface to view information such as baseline impact.
-   Look at blood ties: see the kinship map of the node.
-   Stop operation: Kill task, valid only for this instance
-   Re-run: Failed task or abnormal status task re-run instance.
-   Rerunning downstream: downstream rerunning instances of the current node, if there are multiple downstream instances, all of these instances will run again.
-   Success: the node status is set to success.
-   Emergency Operation: Emergency Operation refers to the operation of the current instance in a very urgent situation, emergency operations are only valid for the current node, including removing dependencies, modifying priorities, and forcing rerunning.
    -   Remove dependencies: undependency this node, this node is often started when upstream fails and there is no data relationship to this instance.
    -   Modify priority: Modify the priority of the current instance when the node is very important, used when running slowly \(not recommended \).
    -   Force run again: ignores the status of the current instance and forces a restart \(not recommended \).
-   Pause \(freeze\): place the current node into a pause \(freeze\) state and stop scheduling.
-   Restore \(thaw\): restore the suspend \(frozen\) node to schedule.

## Description of instance status {#section_jjq_xw3_r2b .section}

|SN|Status|State Mark|
|:-|:-----|:---------|
|1|Running succeeded|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15478148638784_en-US.png)|
|2|Not running|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15478148638785_en-US.png)|
|3|Running failed|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15478148648786_en-US.png)|
|4|Under running|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15478148648787_en-US.png)|
|5|Waiting status|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15478148648788_en-US.png)|
|6|Frozen status|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15478148648789_en-US.png)|

