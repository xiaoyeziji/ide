# PatchData {#concept_nrn_pxh_r2b .concept}

PatchData instances are generated during the completion of data for cyclic tasks, which allows O&M management of scheduled instance tasks such as viewing running status and terminating, re-running, and unfreezing tasks.

## Instance list {#section_i2k_xs3_r2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/15396566618837_en-US.jpg)

1.  Instance name/DAG graph: You can open the Dag graph for this node to view the results of the Instance run.
2.  Stop running: If the instance is running, click STOP to run the kill task.
3.  Re-run: re-schedule this instance.
4.  More: including node status modification and more functions.

Introduction to more features:

-   Re-run downstream: re-run the downstream task for this node.
-   Success: If the node fails to run, the node is successfully activated downstream.
-   Pause \(freeze\): sets the current node to a pause \(freeze\) State and stops scheduling, when the node state is suspended, an icon![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/15396566618856_en-US.png) appears after the node name.
-   Restore \(thaw\): restore the suspend \(frozen\) node to schedule.
-   Look at blood ties: see the ki-nship map of the node.

## Dag graph Introduction {#section_q3l_ft3_r2b .section}

Click the node name or dag map to open the Dag graph interface for this instance, right-click the node to see the operational features of this node.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16361/15396566618837_en-US.jpg)

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
    1.  Remove dependencies: undependency this node, this node is often started when upstream fails and there is no data relationship to this instance.
    2.  Modify priority: Modify the priority of the current instance when the node is very important, used when running slowly \(not recommended \).
    3.  Force run again: ignores the status of the current instance and forces a reboot \(not recommended \).
-   Pause \(freeze\): place the current node into a pause \(freeze\) state and stop scheduling.
-   Restore \(thaw\): restore the suspend \(frozen\) node to schedule.

## Description of instance status {#section_jjq_xw3_r2b .section}

|SN|Status|State Mark|
|:-|:-----|:---------|
|1|Running succeeded|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15396566618784_en-US.png)|
|2|Not running|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15396566618785_en-US.png)|
|3|Running failed|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15396566628786_en-US.png)|
|4|Under running|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15396566628787_en-US.png)|
|5|Waiting status|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15396566628788_en-US.png)|
|6|Frozen status|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15396566628789_en-US.png)|

