# Manual business flow overview {#concept_fys_trk_q2b .concept}

In a Manual Business Flow, all created nodes must be manually triggered and cannot be executed by scheduling. Therefore, it is unnecessary to configure the parent node dependency and local node output for nodes in a manual business flow.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16316/15525333987957_en-US.png) 

The functions of the manual business flow interface are described below:

|No.|Function|Description|
|:--|:-------|:----------|
|1|Submit|Submits all nodes in the current manual business flow.|
|2|Run|Runs all nodes in the current manual business flow. Because the dependency does not exist among manual tasks, these tasks will run concurrently.|
|3|Stop run|Stops a running node.|
|4|Publish|Goes to the task publish interface, where you can publish some or all submitted nodes , but does not publish to the production environment.|
|5|Go to O&M|Goes to the O&M center.|
|6|Reload|Reloads the current manual business flow interface.|
|7|Auto layout|Automatically sequence the nodes in the current manual business flow.|
|8|Zoom-in|Zoom-in the interface.|
|9|Zoom-out|Zoom-out the interface.|
|10|Query|Query a node in the current manual business flow.|
|11|Full screen|Shows nodes in the current manual business flow in full-screen mode.|
|12|Parameters|Configures the parameters. The priority of a flow parameter is higher than that of a node parameter. If a parameter key matches a parameter, the business flow parameter is configured preferentially.|
|13|Operation records|Views the operation history of all nodes in the current manual business flow.|
|14|Version|Views the submission and published records of all nodes in the current manual business flow.|

