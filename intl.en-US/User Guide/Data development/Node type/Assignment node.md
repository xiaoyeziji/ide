# Assignment node {#concept_zvk_ypw_fgb .concept}

This topic describes the functions of the Assignment Node. The **Assignment Node** is a special node type that supports the assignment of output parameters by writing code in the node. The Assignment Node transfers the integrated node context to downstream nodes for reference, which in turn is used as values.

## Create an assignment node {#section_ulr_jqw_fgb .section}

Go to **Control** and click the **Assignment Node** that is located in the class directory of the new node menu, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/155253257935107_en-US.png)

## Write the logic value of the assignment node {#section_w4s_kqw_fgb .section}

The assignment node has a fixed output parameter that names outputs in the **Node Context**. It supports the usage of MaxCompute, Shell, and Python to write code to assign parameters, whose values are the operation and calculation results of the node code. Only one language can be selected for a single assignment node.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/155253258035110_en-US.png)

**Note:** 

-   The value of the output parameter takes only the output from the last line of code as follows:
    -   The output of the SELECT statement on the last line of MaxCompute SQL.
    -   The data from the ECHO statement on the last line of shell.
    -   The output of the PRINT statement on the last line of Python.
-   The maximum transfer value of the output parameter is 2M. If the assignment statement output value exceeds this limit, the assignment node will fail to run.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/155253258035111_en-US.png)

## Use the assignment node output on the downstream node {#section_u5q_mqw_fgb .section}

Add an Assignment Node as an upstream dependency in the downstream node, and define the Assignment Node output as an input parameter for the node through node context. Then reference the node in code to obtain the specific values of the upstream assignment node output parameters. For more information, see[Node context](reseller.en-US/User Guide/Data development/Scheduling configuration/Node context.md#).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/155253258035112_en-US.png)

## An example of assignment node {#section_xpq_nqw_fgb .section}

1.  Create the business flow, and then create the following nodes as shown in the figure, respectively.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/155253258035115_en-US.png)

2.  By default, the system will display an **Outputs** parameter when the assignment node is configured. After the task is run, you can find the relevant parameter results in the related **Operation Center** \> **Properties** \> **Context** page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/155253258035119_en-US.png)

3.  The upstream **Outputs** parameter is used as the downstream input parameter, as shown in the figure below.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/155253258035120_en-US.png)


## Run the assignment node task {#section_sgt_4qw_fgb .section}

**Note:** Typically, you can supplement data running in the above configuration parameters in O&M. The above configuration parameters can be validated through patch data operation, but the test operation parameters cannot be validated.

1.  When the task is configured and scheduled, a run instance is generally generated the next day. The following figure is an example of running supplementary data.
2.  You can view the context input and output parameters, and click the next link to view the input or output results during runtime.
3.  In the **Running Log**, you can view the final code output through 'finalResult'.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/155253258035123_en-US.png)


