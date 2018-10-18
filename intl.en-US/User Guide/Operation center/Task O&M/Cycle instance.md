# Cycle instance {#concept_nlb_nxh_r2b .concept}

Cycle instances are instance snapshots that are automatically scheduled when any cyclic task reaches the cyclic running time for scheduling. 

One instance workflow is generated after each scheduling,  which allows O&M management of scheduled instance tasks such as to view the running status and killing, re-running, and unfreezing tasks.

## Instance list {#section_cqp_cyh_r2b .section}

The instance list provides operations and management for the tasks that have been scheduled in the form of a list. including checking running logs, re-running tasks, and killing running tasks.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15398526208775_en-US.png)

|Operation|Description|
|:--------|:----------|
|Filter|As the modules in the figure above, there are abundant Screening Conditions, the default filtering business date is a workflow task that is a day before the current time. You can add criteria such as Task Name, run time, owner, and so on for more precise filtering.|
|Terminate|It only applies to the instances in "Waiting" and "Running" statuses. If you perform this operation on an instance, the instance becomes "Failed".|
|Rerun| You can re-run a certain task. When the task is executed successfully, the scheduling of its downstream tasks that are not running can be triggered. This feature is often used for handling error nodes or missed nodes.

**Note:** Only tasks in the state of "Not Running", "Succeeded" and "Failed" can be re-run.

 |
|Rerun Downstream| It allows you to re-run the selected task and its downstream tasks. When the selected job re-runs successfully, scheduling can be triggered for its downstream jobs in the "Not Running" status. It is usually used for data restoration.

 **Note:** Prerequisite: Only a task in the Not Running, Succeeded, or Failed state can be selected. Otherwise, a prompt**An ineligible node is selected**is displayed and re-running is prohibited.

 |
|Set as Succeeded| It allows you to change the status of the current node to "Succeeded" and run the downstream tasks in the "Not Running" status. This feature is often used for handling error nodes.

**Note:** Only tasks in a failed state can be successful, and workflow tasks cannot be successful.

 |
|Freeze|the freeze in the cycle instance is directed only at the current instance and is in the running instance, the freeze operation has no practical effect and does not kill the running instance.|
|Unfreeze|You can unfreeze an instance of the frozen state.-   If the instance is not already running, the upstream task runs automatically after it has finished running.
-   If the upstream task runs, the task is directly set to fail, the instance needs to be rerun manually before it can run properly.

|
|Bulk operation|As in the module above, bulk operation includes: stop running, run again, make successful, freeze, unfreeze 5 features.|

## Instance DAG Graph {#section_ivs_xyh_r2b .section}

Click the task name to view the instance DAG.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15398526218779_en-US.png)

-   Right-click an instance, you can view the dependencies and details of this instance and perform specific actions such as stopping, rerunning, and so on.

    |Operation|Description|
    |:--------|:----------|
    |**Show Parent Node/Child Node**|When a workflow has 3 nodes and above, nodes are automatically hidden when the operations center displays tasks, and you can expand the parent-child level, to see the contents of all nodes. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15398526218780_en-US.png)

|
    |**View running log**|It allows you to view the running logs of the task when the node is in the status of "Running", "Succeeded" or "Failed".|
    |**View Code**|It allows you to view the code of the instance task.|
    |**Edit Node**|You can jump to the data development page to edit the node.|
    |**View Lineage**|see the kinship map of the node.|
    |**Terminate**|Kill task, valid only for this instance|
    |**Rerun**|Failed task or abnormal status task re-run instance.|
    |**Rerun Downstream**|It allows you to re-run the selected task and its downstream tasks. When the selected job re-runs successfully, scheduling can be triggered for its downstream jobs in the "Not Running" status. It is usually used for data restoration.|
    |**Configured**|It allows you to change the status of the current node to "Succeeded" and run the downstream tasks in the "Not Running" status. This feature is often used for handling error nodes.|
    |**Freeze**|the freeze in the cycle instance is directed only at the current instance and is in the running instance, the freeze operation has no practical effect and does not kill the running instance.|
    |**Unfreeze**|You can unfreeze an instance of the frozen state.|

-   Double-click an instance to pop up task properties, run logs, operation logs, code, and so on.

    |View content|Description|
    |:-----------|:----------|
    |**Properties **|the attributes of this node are described, including schedule type, status, time, and so on.|
    |**Running Log**|this node is running or running log information.|
    |**Operations Log**|The operation log for the node, including the records of node changes, replenishment data, and so on.|
    |**Code**|Code edited by the node.|


## Description of instance status {#section_afv_lzh_r2b .section}

|SN|Status|State Mark|
|:-|:-----|:---------|
|1|Running succeeded|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15398526218784_en-US.png)|
|2|Not running|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15398526218785_en-US.png)|
|3|Running failed|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15398526218786_en-US.png)|
|4|Under running|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15398526218787_en-US.png)|
|5|Waiting status|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15398526228788_en-US.png)|
|6|Frozen status|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15398526228789_en-US.png)|

