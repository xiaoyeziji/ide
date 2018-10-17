# Testing instances {#concept_azp_qxh_r2b .concept}

When the periodic task reaches the periodic run time configured to enable the modulation,, an instance snapshot that is automatically scheduled is a periodic instance. An instance workflow is generated at each scheduling. Daily O&M is performed for jobs on the started instance as scheduled, such as operations including viewing run statuses, or stopping, rerunning, or repairing a job,

## Instance list {#section_i2k_xs3_r2b .section}

The instance list provides operations and management for the tasks that have been scheduled in the form of a list. including checking running logs, re-running tasks, and killing running tasks. The specific functions are described as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16362/15397474938868_en-US.png)

-   Filter Function: As the modules in the figure above, there are abundant Screening Conditions, the default filtering business date is a workflow task that is a day before the current time. You can add criteria such as Task Name, run time, owner, and so on for more precise filtering.
-   Kill: It only applies to the instances in "Waiting" and "Running" statuses. If you perform this operation on an instance, the instance becomes "Failed".
-   You can re-run a certain task. When the task is executed successfully, the scheduling of its downstream tasks that are not running can be triggered. This feature is often used for handling error nodes or missed nodes.

    **Note:** Only tasks in the state of "Not Running", "Succeeded" and "Failed" can be re-run.

-   Re-run Downstream Tasks: It allows you to re-run the selected task and its downstream tasks. When the selected job re-runs successfully, scheduling can be triggered for its downstream jobs in the "Not Running" status. It is usually used for data restoration.

    **Note:** You can only check tasks that are not running, completed, or failed. If you check tasks in other states, the page prompts the **selected node to contain nodes that do not meet the running conditions** and prohibits committing to run.

-   Set as Succeeded: It allows you to change the status of the current node to "Succeeded" and run the downstream tasks in the "Not Running" status. This feature is often used for handling error nodes.

    **Note:** Only tasks in a failed state can be successful, and workflow tasks cannot be successful.

-   Freeze: the freeze in the cycle instance is directed only at the current instance and is in the running instance, the freeze operation has no practical effect and does not kill the running instance.
-   Unfreezing: You can unfreeze an instance of the frozen state.
    -   If the instance is not already running, the upstream task runs automatically after it has finished running.
    -   If the upstream task runs, the task is directly set to fail, the instance needs to be rerun manually before it can run properly.
-   Bulk operation: As in the module above, bulk operation includes: stop running, run again, make successful, freeze, unfreeze features.

## Instance DAG Graph {#section_r1m_wx3_r2b .section}

Click the task name to view the instance DAG. In the instance DAG View:

-   Right-click an instance, you can view the dependencies and details of this instance and perform specific actions such as stopping, rerunning, and so on.
-   Double-click an instance to pop up task properties, run logs, operation logs, code, and so on, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16362/15397474938869_en-US.png)

-   **Refresh node instance**: If you have modified the code or schedule parameters after the instance has been generated, you can click this button to use the latest code and parameters \(bulk operations are not supported \). **Use this function with caution because refreshing node instances is not refreshing the node status.**
-   **Properties**: View instance properties, including various time information about the instance Run, Run Status, and so on.
-   **View running log**: It allows you to view the running logs of the task when the node is in the status of "Running", "Succeeded" or "Failed".
-   **Operational Log**: It records the operations performed on the instance, such as killing and re-running.
-   **Code**: It allows you to view the code of the instance task.
-   **Expand parent node/child node**: When a workflow has 3 nodes and above, nodes are automatically hidden when the operations center displays tasks, and you can expand the parent-child level, to see the contents of all nodes. As shown in the following illustration:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16362/15397474938870_en-US.png)

-   **Expand/Close workflow**: When you have a workflow task, you can expand a workflow task, view the Run Status of the internal node task. As shown in the following illustration:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16362/15397474938871_en-US.png)


## Description of instance status {#section_jjq_xw3_r2b .section}

|SN|Status|State Mark|
|:-|:-----|:---------|
|1|Running succeeded|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15397474938784_en-US.png)|
|2|Not running|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15397474938785_en-US.png)|
|3|Running failed|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15397474938786_en-US.png)|
|4|Under running|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15397474938787_en-US.png)|
|5|Waiting status|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15397474938788_en-US.png)|
|6|Frozen status|![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16359/15397474938789_en-US.png)|

