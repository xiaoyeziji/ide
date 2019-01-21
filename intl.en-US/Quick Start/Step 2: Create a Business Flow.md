# Step 2: Create a Business Flow {#concept_m1t_nzl_s2b .concept}

This topic uses create business flow as an example to describe how to create nodes and configure dependencies in your business flow to facilitate the design and presentation of steps and sequences of data analysis. This article briefly explains how to use the data development function to further analyze and calculate the workspace data.

DataWorks data development features support visual drag-and-drop in the business flow to complete inter-node dependency settings. The data flow and dependencies are implemented in the form of operational business flow. Currently supports multiple task types, such as MaxCompute SQL, data synchronization, open\_mr, shell, machine learning, and virtual nodes. For specific usage methods for each task type, see[Node type overview](../../../../../reseller.en-US/User Guide/Data development/Node type/Node type overview.md#).

## Prerequisites {#section_vcm_g3r_s2b .section}

Make sure you have [built the table and uploaded the data](reseller.en-US/Quick Start/Step 1: Create a table and upload data.md#), prepare the business data table bank\_data and data in the workspace, as well as the result table.

## Procedure { .section}

**Create a business flow**

1.  After [Create Workspace](../../../../../reseller.en-US/Preparation/Administrator Operations/Create Workspace.md#), click **Enter workspace** in the corresponding project.
2.  Go to the DataStudio page and select **create** \> **business flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15480557568983_en-US.png)

3.  Enter the name and description of the business flow.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15480557568984_en-US.png)


**Create a node and dependency on the flow canvas**

This section shows how to create a virtual node “start” and a MaxCompute SQL node “insert\_data”, and to configure “insert\_data” to depend on “start”.

**Note:** 

-   The virtual node is a control-type node that does not affect data during business flow operation and is only used to control O&M of downstream nodes.
-   When a virtual node depends on other nodes and the status is manually set to error by the O&M personnel, downstream nodes that have not run yet cannot be triggered. This prevents further propagation of erroneous upstream data during the O&M flow. For more information, see the section on virtual nodes in [Node type overview](../../../../../reseller.en-US/User Guide/Data development/Node type/Node type overview.md#).
-   The upstream task of a virtual node in a business flow is typically set as the root node of the project, the format of the Project root node is: Project name \_ root.

We recommend you create a virtual node as the root node to control the whole business flow when designing a flow.

1.  Double-click the virtual node and enter the node name start.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15480557568985_en-US.png)

2.  Double-click **MaxCompute SQL** to enter the node name “insert\_data”.
3.  Click the start node, and draw a line between start and insert\_data to make insert\_data a dependency on start, as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15480557568986_en-US.png)


**Editing code in the MaxCompute SQL Node**

This section describes how to use SQL code in the MaxCompute SQL node **insert\_data** to query the number of mortgages available for individuals with different educational backgrounds and save results for analysis or display by the following nodes.

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

**Run and debug MaxCompute SQL**

1.  After editing the SQL statements in the insert\_data node, click **Save** to prevent code loss.
2.  Click **Run** to view the operations logs and results,

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15480557568987_en-US.png)


**Save and submit business flows**

After running and debugging the MaxCompute SQL node “insert\_data”, return to the flow page. Click **Save** and **Submit** the whole flow.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16181/15480557568988_en-US.png)

## Subsequent steps {#section_gm5_llr_s2b .section}

Now you have learned how to create, save, and submit the business flow. You can proceed to the next topic which shows how to create a synchronization task to export data to the different types of data sources. For more information, see [create synchronization task export results](reseller.en-US/Quick Start/Step 3: Create a synchronization task.md#).

