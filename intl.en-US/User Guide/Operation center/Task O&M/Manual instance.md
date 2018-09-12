# Manual instance {#concept_nsf_4xh_r2b .concept}

Manual instances are generated after a manual task is triggered, which allows O&amp;M management of scheduled instance tasks such as viewing running status and killing and re-running tasks.

A manual instance, as the name implies, is an instance of a manual task, and a manual task is characterized by No scheduling dependency, you only need to trigger manually.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16360/15367352208835_en-US.jpg)

1.  Instance name/DAG graph: You can open the DAG graph for this node to view the results of the Instance run.
2.  Stop running: If the instance is running, click STOP to run the kill task.
3.  Re-run: re-schedule this instance.

Manual tasks have no dependencies, so the DAG graph only displays this instance, click the instance to see the properties, run log, operation log, code four columns. Right-click instance to see run log, code, edit node, view blood, terminate run, run again.

-   Attributes: the attributes of this node are described, including schedule type, status, time, and so on.
-   Run log: this node is running or running log information.
-   Operation Log: The operation log for the node, including the records of node changes, replenishment data, and so on.
-   Code: Code edited by the node.

Introduction to the right-click node instance function:

-   View running logs: Enter the Operations Log interface, where you can see information such as logview in the Operations Log.
-   View node code: You can view the current code of the node.
-   Edit nodes: You can jump to the data development page to edit the node.
-   Look at blood ties: see the kinship map of the node.
-   Stop operation: Kill task, valid only for this instance
-   Re-run: Failed task or abnormal status task re-run instance.

