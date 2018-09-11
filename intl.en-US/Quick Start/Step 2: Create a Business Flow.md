# Step 2: Create a Business Flow {#concept_m1t_nzl_s2b .concept}

This article will take the creation of business flows as an example, describes how to create nodes and configure dependencies in your business flow, to facilitate the design and presentation of the steps and sequences of data analysis. This article briefly explains how to use the data development function to further analyze and calculate the workspace data.

DataWorks data development features support visual drag-and-drop in the business flow to complete inter-node dependency settings. The flowing and interdependencies of data are implemented in the form of operational business flows. Multiple Task types such as ODPS SQL, data synchronization, open\_mr, shell, machine learning, and virtual nodes are currently supported, for specific usage methods for each task type, see[Introduction of Node Type](../../../../intl.en-US/User Guide/Data development/Node type/Node type overview.md#).

## Prerequisites {#section_vcm_g3r_s2b .section}

Make sure that you have [built the table and uploaded the data](intl.en-US/Quick Start/Step 1: Create a table and upload data.md#), prepare the business data table bank\_data and the data in it in the workspace, as well as the result table.

## Procedure { .section}

**Create a Business Flow**

1.  After [Create a project](../../../../intl.en-US/Preparation/Administrator operations/Create a project.md#), click **Enter workspace** in the corresponding project.
2.  Go to the DataStudio page and select **create** \> **business flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15366513358983_en-US.png)

3.  Enter the name and description of the business flow.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15366513358984_en-US.png)


**Create a node and dependency on the flow canvas**

This section shows how to create a virtual node “start” and an ODPS SQL node “insert\_data”, and to configure “insert\_data” to depend on “start”.

**Note:** 

-   As a control-type node, the virtual node does not affect the data during flow operation and is only used for O&amp;M control of downstream nodes..
-   When a virtual node depends on the other nodes and its status is manually set to failure by the O&M personnel, its downstream nodes that have not run yet, cannot be triggered. This prevents further propagation of erroneous upstream data during the O&M flow. For more information, see the section on virtual nodes in [Introduction of Node Type](../../../../intl.en-US/User Guide/Data development/Node type/Node type overview.md#).
-   The upstream task of a virtual node in a business flow is typically set as the root node of the project, the format of the Project root node is: Project name \_ root.

We recommend that you create a virtual node as the root node to control the whole flow when designing a flow.

1.  Double-click the virtual node and enter the node name start.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15366513358985_en-US.png)

2.  Double-click **ODPS SQL** to enter the node name “insert\_data”.
3.  Click the start note, and draw a line between start and insert\_data to make insert\_data dependent on start, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15366513358986_en-US.png)


**Editing code in the ODPS SQL Node**

This section describes how to use SQL code in the ODPS SQL node **insert\_data** to query the quantity of mortgages available for individuals having different educational background and save results for analysis or display by the following nodes.

The SQL statements are as follows. For more information about the syntax, see [MaxCompute SQL](https://www.alibabacloud.com/help/doc-detail/27860.htm).

```
INSERT OVERWRITE TABLE result_table  --Insert data to result_table
SELECT education
    , COUNT(marital) AS num
FROM bank_data
WHERE housing = 'yes'
    AND marital = 'single'
GROUP BY education
```

**Run and debug ODPS SQL**

1.  After editing the SQL statements in the insert\_data node, click **Save** to prevent code loss.
2.  Click **Run** to view the operations logs and results,

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15366513358987_en-US.png)


**Save and submit business flows**

After running and debugging the ODPS SQL node “insert\_data”, return to the flow page. Click **Save** and **Submit** the whole flow.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15366513358988_en-US.png)

## Subsequent steps {#section_gm5_llr_s2b .section}

Now you have learned how to create, save, and submit the flow. You can proceed with the next tutorial which shows how to create a synchronization task to export data to the diffrent types of the data sources. For more information, see [creating synchronization task export results](intl.en-US/Quick Start/Step 3: Create a synchronization task.md#).

