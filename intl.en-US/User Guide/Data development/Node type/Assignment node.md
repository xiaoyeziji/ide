# Assignment node {#concept_zvk_ypw_fgb .concept}

**Assignment node** is a special type of node. It supports assignment of output parameters by writing code in the node, and transfers them in combination with the node context, for downstream nodes to reference and use their values.

## Create an assignment node {#section_ulr_jqw_fgb .section}

**Assignment Node** is located in the **Control** class directory of the new node menu, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/154804686035107_en-US.png)

## Write the value logic of assignment node {#section_w4s_kqw_fgb .section}

The assignment node has a fixed output parameter named outputs in the **Node Context**. It supports the use of MaxCompute, Shell and Python to write code to assign parameters, whose values are the operation and calculation results of node code. Only one language can be selected for a single assignment node.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/154804686035110_en-US.png)

**Note:** 

-   The value of the outputs parameter takes only the output from the last line of code, that is:
    -   The output of the SELECT statement on the last line of MaxCompute SQL .
    -   Data from the ECHO statement on the last line of shell.
    -   The output of the PRINT statement on the last line of Python.
-   There is a certain limit to the value of the outputs parameter, with a maximum transfer value of 2M. If the output of the assignment statement exceeds this limit, the assignment node will fail to run.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/154804686035111_en-US.png)

## Use the output of the assignment node on the downstream Node {#section_u5q_mqw_fgb .section}

In the downstream node, after adding an assignment node as an upstream dependency, define the output of the assignment node as an input parameter for the node by the way of node context, and reference it in the code, the specific values for the output parameters of the upstream assignment node can be obtained. For more information, see[Node context](reseller.en-US/User Guide/Data development/Scheduling Configuration/Node Context.md#).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/154804686035112_en-US.png)

## An example of assignment node {#section_xpq_nqw_fgb .section}

1.  Create the business process, and then create the following nodes respectively, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/154804686035115_en-US.png)

2.  When configuring the assignment node, the system will display a **outputs** parameter by default. After running, you can find the relevant parameter results in the related **Operation Center** \> **Properties** \> **Context** page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/154804686035119_en-US.png)

3.  The upstream **outputs** parameter is used as the downstream input parameter, as shown in the figure below.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/154804686035120_en-US.png)


## Run the assignment node task {#section_sgt_4qw_fgb .section}

**Note:** In general operation and maintenance, the above configuration parameters can be validated by patch data operation, but the test operation parameters can not be validated.

1.  When the task is configured and scheduled, a run instance is generally generated the next day.
2.  At runtime, you can view the input and output parameters of the context, and click the following link to see your input or output results.
3.  In the **Running Log**, you can view the final output of the code through 'finalResult'.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/82907/154804686035123_en-US.png)


