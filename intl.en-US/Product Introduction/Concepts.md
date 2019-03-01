# Concepts {#concept_bnp_ydp_r2b .concept}

## Business flow {#section_udp_k4y_s2b .section}

This topic describes DataWorks business flows, solutions, components, tasks, instances, submissions, script development, resources, functions, and output name concepts.

Advantages:

-   Helps organize data codes from business perspectives supports code organization based on task types and multi-level sub-directories \(Alibaba Cloud recommends no more than four levels\).
-   Provides work flow overview from business perspectives to facilitate optimization.
-   Provides business flow dashboards for efficient development.
-   Organizes release and maintenance based on business flows.

## Solution {#section_srl_n4y_s2b .section}

DataWorks offers customizable and integrable business flow solutions.

Advantages:

-   Multiple business flows
-   Reusable business flows for different solutions
-   Comprehensive solutions for immersive development

## Component {#section_hgj_p4y_s2b .section}

A component is a SQL code procedure template with multiple input and output parameters, SQL code procedures are generally handled by introducing one or more data table sources through filtering, connect, aggregate, and other operations to process target tables for new business needs. The common logic in SQL can be abstract components to enhance code reuse.

## Task {#section_pdk_12p_r2b .section}

A task performs various data operations . The following describes various task applications:

-   A data synchronization node task is used to copy data from RDS to MaxCompute.
-   A MaxCompute SQL node task is used to run MaxCompute SQL for data conversion.
-   A flow task is used to perform a series of data conversions among several inner SQL nodes.

Each task uses zero or more data tables \(data sets\) as an input, and generates one or more data tables \(data sets\) as the output.

Tasks are divided into node tasks, flow tasks, and inner nodes. See the relationships between these tasks in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16168/15514200648911_en-US.png)

-   A node task is a data operation. It can be configured to be dependent on other node tasks and flow tasks to form a Directed Acyclic Graph \(DAG\).
-   A flow task is formed by a group of inner nodes that process a a work flow task. We recommend using less than 10 flow tasks. Inner nodes of a flow task cannot be dependencies of other flow or node tasks. A flow task can be configured to be a dependency of other flow and node tasks to form a DAG.
-   An inner node is a node within a flow task. It basically has the same capabilities as a node task. Its scheduling cycle is inherited from the flow task scheduling frequency and cannot be configured independently. The dependency can only be dragged.

Data execution can be selected from an operation type, see [Node type overview](../../../../../intl.en-US/User Guide/Data development/Node type/Node type overview.md#).

For details about task scheduling parameter configurations, see [Scheduling configuration](../../../../../intl.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

## Instance {#section_dg5_w2p_r2b .section}

An instance is generated when a task is scheduled by the system or triggered manually. An instance is a snapshot that runs by a task at a certain time. The instance contains the task operating time, operating status, operating logs, and other information. For example:

Assume that Task 1 is configured to run at 02:00 each day. In this case, the scheduling system automatically generates a snapshot at the time predefined by the periodic node task at 23:30 each day. That is, the instance of Task 1 will run at 02:00 the next day. If the system detects the upstream task is complete, the system automatically runs the Task 1 instance at 02:00 the next day.

**Note:** 

You can query task instance information on the **O&M Center** \> **Task O&M**page.

## Submit {#section_un1_tfp_r2b .section}

Submit refers to the node task development process, and the work flow tasks from developing environments that are published to the scheduling systems. After a task is submitted, its code and scheduling configuration are synchronized to the scheduling system, which schedules the task according to the configuration.

**Note:** Unsubmitted node tasks and flow tasks do not enter the scheduling system.

## Script {#section_z14_wfp_r2b .section}

A script is a code storage space for data analysis. The script code cannot be released to the scheduling system, and its scheduling parameters cannot be configured. It can only be used for data query and analysis.

## Resources and functions {#section_lj3_ggp_r2b .section}

Resources and functions are both MaxCompute concepts. For details, see [MaxCompute resources](https://www.alibabacloud.com/help/doc-detail/27822.htm) and [MaxCompute functions](https://www.alibabacloud.com/help/doc-detail/27823.htm).

In DataWorks, interfaces are used for resource and function management. Resources and functions managed through other MaxCompute methods cannot be queried in DataWorks.

## Output name {#section_phs_cnh_y2b .section}

The output name is the name of each task's output point. If users set dependencies within a Alibaba Cloud account single tenant, a virtual entity that connects upstream and downstream tasks. .

If a task is set to form upstream and downstream dependencies with other tasks, the setting must be based on the output name. The task output name is also the input name for the downstream node.

**Note:** Similar to a task ID, the output name can be a unique conceptual object for a task that is different from other tasks in the same tenant. You can also add custom output names to a task, however, the output node name must be unique within the tenant.

