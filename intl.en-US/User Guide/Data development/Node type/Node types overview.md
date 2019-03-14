# Node types overview {#concept_mbj_pc2_p2b .concept}

This topic describes how to apply the seven different node types in DataWorks in different scenarios.

## Virtual node {#section_xp1_td2_p2b .section}

A virtual node is a control node that does not generate any data. The virtual node is generally used as the root node for planning the overall node workflow. For more information about virtual nodes, see [Virtual node](reseller.en-US/User Guide/Data development/Node type/Virtual node.md#).

**Note:** The final workflow output table contains multiple branch input tables. Virtual nodes are usually used if these input tables do not have any dependencies between them.

## ODPS SQL node {#section_e4q_wc2_p2b .section}

An ODPS SQL task allows you to edit and maintain the SQL code on the Web, and easily implement code runs, debug, and collaboration. DataWorks also provides code version management, automatic resolution of upstream and downstream dependencies, and other features. For more information about the examples, see [ODPS SQL node](reseller.en-US/User Guide/Data development/Node type/ODPS SQL node.md#).

By default, DataWorks uses the MaxCompute project as the space for development and production, so that the code content of the MaxCompute SQL node follows the MaxCompute SQL syntax . MaxCompute SQL syntax is similar to Hive, which can be considered a subset of the standard SQL. However, MaxCompute SQL cannot be equated with a database because it does not possess the following database features: transactions, primary key constraints, and indexes.

For more information about MaxCompute SQL syntax, see [SQL overview](https://www.alibabacloud.com/help/doc-detail/27860.htm).

## ODPS MR node {#section_shr_jd2_p2b .section}

MaxCompute supports MapReduce programmed APIs, whose Java APIs can be used to compile the MapReduce program for data processing in MaxCompute. You can create MaxCompute MR nodes and use them for task scheduling. For more information about the examples, see [ODPS MR node](reseller.en-US/User Guide/Data development/Node type/ODPS MR node.md#).

## PyODPS node {#section_ewc_ld2_p2b .section}

The [Python SDK](https://www.alibabacloud.com/help/doc-detail/34615.htm)in MaxCompute can be used to operate MaxCompute.

The PyODPS node in DataWorks can be integrated with MaxCompute Python SDK. You can edit the Python code to operate MaxCompute on a PyODPS node in DataWorks. For more information, see [PyODPS node](reseller.en-US/User Guide/Data development/Node type/PyODPS node.md#).

## SQL component node {#section_aml_qd2_p2b .section}

An SQL component node is an SQL code process template that contains multiple input and output parameters. To handle an SQL code process, you need to import, filter, join, and aggregate one or more data source tables to form a target table required for new business. For more information, see [SQL Component node](reseller.en-US/User Guide/Data development/Node type/SQL Component node.md#).

## Data integration node {#section_gvk_rd2_p2b .section}

A data integration node is a stable, efficient, and automatically scalable external data synchronization cloud service provided by the Alibaba Cloud DTplus platform. With the data synchronization node, you can easily synchronize data in the business system to MaxCompute. For more information, see [Data integration node](reseller.en-US/User Guide/Data development/Node type/Data integration node.md#).

