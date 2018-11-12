# Node type overview {#concept_mbj_pc2_p2b .concept}

Seven types of nodes are provided in DataWorks, which are applicable to different use cases.

## Virtual node task {#section_xp1_td2_p2b .section}

A virtual node is a control node that does not generate any data. Generally, it is used as the root node for overall planning of nodes in the workflow. For more information about the virtual node task, see [Virtual node](reseller.en-US/User Guide/Data development/Node type/Virtual node.md#).

**Note:** The final output table of a workflow contains multiple branch input tables. Virtual nodes are usually used if these input tables do not have dependency between them.

## ODPS SQL task {#section_e4q_wc2_p2b .section}

An ODPS SQL task enables you to edit and maintain SQL code directly on the Web, and easily implement running, debugging, and collaborative development. DataWorks also provides code version management, automatic resolution of upstream and downstream dependencies, and other capabilities. For more information about the examples, see [ODPS SQL node](reseller.en-US/User Guide/Data development/Node type/ODPS SQL node.md#).

DataWorks uses the project of MaxCompute by default as the space for development and production, so that the code content of the ODPS SQL node follows the syntax of MaxCompute SQL. MaxCompute SQL adopts the syntax like that of Hive, which can be considered as a subset of standard SQL. However, MaxCompute SQL cannot be equated with a database, because it does not possess many features that a database does, such as transactions, primary key constraints, and indexes.

For more information about the specific MaxCompute SQL syntax, see [SQL overview](https://www.alibabacloud.com/help/doc-detail/27860.htm).

## ODPS MR task {#section_shr_jd2_p2b .section}

MaxCompute supports the MapReduce programming APIs, whose Java APIs can be used to compile MapReduce program for data processing in MaxCompute. You can create ODPS MR nodes and use them for task scheduling. For more information about the examples, see [ODPS MR node](reseller.en-US/User Guide/Data development/Node type/ODPS MR node.md#).

## PyODPS task {#section_ewc_ld2_p2b .section}

MaxCompute provides the [Python SDK](https://www.alibabacloud.com/help/doc-detail/34615.htm), which can be used to operate MaxCompute.

DataWorks also provides the PyODPS task type and integrates the Python SDK of MaxCompute. You can directly edit the Python code to operate MaxCompute on a PyODPS node of DataWorks. For more information, see [PyODPS node](reseller.en-US/User Guide/Data development/Node type/PyODPS node.md#).

## SQL component node {#section_aml_qd2_p2b .section}

An SQL component is an SQL code process template containing multiple input and output parameters. To handle an SQL code process, one or more source data tables are imported, filtered, joined, and aggregated to form a target table required for new business. For more information, see [SQL Component node](reseller.en-US/User Guide/Data development/Node type/SQL Component node.md#).

## Data synchronization task {#section_gvk_rd2_p2b .section}

A data synchronization node task is a stable, efficient, and automatically scalable external data synchronization cloud service provided by the Alibaba Cloud DTplus platform. With the data synchronization node, you can easily synchronize data in the business system to MaxCompute. For more information, see [Data integration node](reseller.en-US/User Guide/Data development/Node type/Data integration node.md#).

