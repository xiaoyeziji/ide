# Basic Attributes {#concept_wwj_tsn_q2b .concept}

The figure below shows the basic attribute configuration interface:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16329/15427067808197_en-US.png)

-   Node Name: It is the node name that you enter when creating a workflow node. To modify a node name, right-click the node on the directory tree and choose **Rename** from the short-cut menu.
-   Node ID: It is the unique node ID generated when a task is submitted, and cannot be modified.
-   Node ID: It is the unique node ID generated when a task is submitted, and cannot be modified.
-   Owner: It is the node owner. The owner of a newly created node is the current logon user by default. To modify the owner, click the input box, and enter the owner name or directly select another user.

    **Note:** When you select another user, the user must be a member of the current project.

-   Description: It is generally used to describe the business and purpose of the node.
-   Parameter: It is used to assign value to a variable in the code during task scheduling.

    For example, when a variable "pt=$\{datetime\}" is used to indicate the time in the code, you can assign a value to the variable here. The assigned value can use the scheduling built-in time parameter "datetime=$bizdate".

-   Resource Group: It specifies the resource group for running the node.

## Parameter value assignment formats for various node types {#section_wxv_slq_p2b .section}

-   ODPS SQL, ODPSPL, ODPS MR, and XLIB types: `Variable name 1=Parameter 1 Variable name 2=Parameter 2...`, Multiple parameters are separated by spaces.
-   SHELL type: `Parameter 1 Parameter 2...`, Multiple parameters are separated by spaces.

Some frequently-used time parameters are provided as built-in scheduling parameters. For more information about these parameters, see [Parameter configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#).

