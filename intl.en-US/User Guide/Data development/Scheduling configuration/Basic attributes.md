# Basic attributes {#concept_dlk_2lq_p2b .concept}

The figure below shows the basic attribute configuration interface:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16300/15525327457831_en-US.png)

-   Node Name: The node name of the created workflow node. To modify the node name, right-click the node on the directory tree and choose **Rename** from the short-cut menu.
-   Node ID: The unique node ID generated when a task is submitted and cannot be modified.
-   Node Type: The node type that you select when creating a workflow node and cannot be modified.
-   Owner: The node owner. By default, the owner of a newly created node is the current logon user. To modify the owner, click the input box, and enter the owner name or select another user.

    **Note:** When you select another user, the user must be a member of the current project.

-   Description: Generally used to describe the business and node purpose.
-   Parameter: A parameter used to assign value to a variable in the code during task scheduling.

    For example, when a variable "pt=$\{datetime\}" is used to indicate the code time, you can assign a value to the variable here. The assigned value can use the scheduling built-in time parameter "datetime=$bizdate".


## Parameter value assignment formats for various node types {#section_wxv_slq_p2b .section}

-   ODPS SQL, ODPS PL, ODPS MR types: `Variable name 1=Parameter 1 Variable name 2=Parameter 2...`, separate multiple parameters with spaces.
-   SHELL type: `Parameter 1 Parameter 2...`, separate multiple parameters with spaces.

Some frequently-used time parameters are provided as built-in scheduling parameters. For more information about these parameters, see [Parameter configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Parameter configuration.md#).

