# Dependencies {#concept_gbk_lxx_p2b .concept}

Scheduling dependency is the foundation of constructing orderly business process. Only by correctly configuring dependencies between tasks, business data can be produced effectively and timely.

DataWorks V2.0 provides three dependency configuration modes: automatic recommendation, automatic parsing and custom configuration. Refer to [Best practices for setting scheduling dependencies](../../../../../intl.en-US/Best Practices/Best practices for setting scheduling dependencies.md#) for an example of the actual operation of dependencies.

**Note:** **You can watch videos to learn more about dependencies: [DataWorks V2.0 FAQs and Difficulty Analysis](https://www.alibabacloud.com/help/doc-detail/97879.htm).**

The scheduling dependency configuration page is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/15475189997925_en-US.png)

Overall scheduling logic: The downstream scheduling can be started only when the upstream scheduling is successfully implemented. Therefore, all workflow nodes must have at least one parent node. Scheduling dependency is used to set the parent-child relationship. The principle and configuration of scheduling dependency configuration are described in detail below.

**Note:** If there is a need for interdependence between standard mode and simple mode projects, please apply for a bill of lading.

## Introduction to standardized R&D scenarios {#section_fsf_ggv_2gb .section}

-   Concept preparation

    -   DataWorks Task: See [Concepts](../../../../../intl.en-US/Product Introduction/Concepts.md#) for details.
    -   Output Name: See [Concepts](../../../../../intl.en-US/Product Introduction/Concepts.md#) for details. The system will assign a default output name ending with '.out' for each node, and you can also add a custom output name, but note that the node output name is not allowed to repeat within the tenant.
    -   Output table: refers to the table after the INSERT or CREATE in the SQL statement of a node.
    -   Input table: refers to the table after the FROM in the SQL statement of a node.
    -   SQL statement: refers to [MaxCompute SQL](../../../../../intl.en-US/User Guide/SQL/SQL summary.md#).
    In practice, a DataWorks task can contain a single SQL statement or multiple SQL statements.

    Each task that forms an upstream and downstream relationship is associated by an output name, and the root node of the project \(node name: projectname\_root\) can be configured as the upstream node of the most upstream node created.

-   Introduction to the standard development process

    In the normalized development process, multiple SQL tasks are established to form a dependency between upstream and downstream, and it is recommended to follow:

    -   The input table for the downstream task must be the output table for the upstream task.
    -   The same table is output by only one task.
    The purpose is to quickly configure complex dependencies through "Auto Parse" when business processes are inflated.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751899913593_en-US.jpg)

    In the figure above, each task and its code are as follows.

    -   The task code of Task\_1 is as follows. The input data of this task comes from the table "ods\_raw\_log\_d", and the data is output to the table "ods\_log\_info\_d".

        ```
        INSERT OVERWRITE TABLE ods_log_info_d PARTITION (dt=${bdp.system.bizdate})
        SELECT ……  //Refers to your select operation
          FROM (
            SELECT ……  //Refers to your select operation
          FROM ods_raw_log_d
          WHERE dt = ${bdp.system.bizdate}
        ) a;
        ```

    -   The task code of Task\_2 is as follows. The input data of this task comes from the table "ods\_user\_info\_d" and table "ods\_log\_info\_d", and the data is output to the table "dw\_user\_info\_all\_d".

        ```
        INSERT OVERWRITE TABLE dw_user_info_all_d PARTITION (dt='${bdp.system.bizdate}')
        SELECT ……  //Refers to your select operation
        FROM (
          SELECT *
          FROM ods_log_info_d
          WHERE dt = ${bdp.system.bizdate}
        ) a
        LEFT OUTER JOIN (
          SELECT *
          FROM ods_user_info_d
          WHERE dt = ${bdp.system.bizdate}
        ) b
        ON a.uid = b.uid;
        ```

    -   The task code of Task\_3 is as follows. The input data of this task comes from the table "dw\_user\_info\_all\_d", and the data is output to the table "rpt\_user\_info\_d".

        ```
        INSERT OVERWRITE TABLE rpt_user_info_d PARTITION (dt='${bdp.system.bizdate}')
        SELECT ……  //Refers to your select operation
        FROM dw_user_info_all_d
        WHERE dt = ${bdp.system.bizdate}
        GROUP BY uid;
        ```


## Depended upstream node {#section_xs5_z3y_p2b .section}

Upstream node: Specifies the parent node that the current node depends on.

Here, it is required to fill in the output name of upstream node \(one node may have multiple output names at the same time, just fill in one\), rather than the upstream node name. You can manually search for the output name of upstream node to add, or you can parse it through the SQL blood code.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/15475189997926_en-US.png)

**Note:** If added by search, the searcher searches according to the output name of the node that has been submitted to the scheduling system.

-   Search by entering output name of the parent node

    You can construct a dependency by searching for the output name of a node and configuring it as the upstream dependency of the current node.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751899913932_en-US.png)

-   Search by entering the table name of the parent node's output name

    This method must ensure that one of the output names of the parent node is the table name after INSERT or CREATE in the SQL code of the node, such as "projectname.tablename" \(such output name can generally be obtained through automatic parsing\).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751899913933_en-US.png)

    After the submission is executed, the output name can be searched by other nodes by searching the table name.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751899913934_en-US.png)


## Current node output {#section_cfq_cty_p2b .section}

Output: Specifies output of the current node.

Each node is assigned a default output name ending with ".out", and you can also add a custom output name or get an output name through automatic parsing.

**Note:** The name of the output node is globally unique and no duplication is allowed in the entire Aliyun account system.

## Auto-parsing dependencies {#section_f1f_2d5_2gb .section}

DataWorks can parse different dependencies according to the actual SQL content in the task node. The output names of the parent node and the current node that obtained by parsing are as follows.

-   Output name of the parent node: the table name after projectname.INSERT.
-   Output names of the current node:
    -   the table name after projectname.INSERT.
    -   the table name after projectname.CREATE \(Generally used for temporary tables\).

**Note:** If you upgrade from DataWorks V1.0 to DataWorks V2.0, the output name of the current node is "projectname.nodename".

If there are multiple INSERTs and FROMs, multiple output and input names will be parsed.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013595_en-US.jpg)

If you construct multiple tasks with dependencies, and these tasks satisfy the condition that all input tables of downstream tasks come from the output tables of upstream tasks, the fast configuration of full workflow dependencies can be achieved by automatic parsing.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013935_en-US.png)

**Note:** 

-   To increase the flexibility of task, it is recommended that a task contain only one output point, so that you can flexibly assemble SQL business processes for decoupling purpose.
-   If a table name in an SQL statement is both an output table and a referenced table \(a dependent table\), it will only be parsed as an output table.
-   If a table name in an SQL statement is referenced or output many times, only one scheduling dependency is parsed.
-   If there is a temporary table in the SQL code \(for example, a table beginning with "t\_" is specified as a temporary table in the [Project configuration](intl.en-US/User Guide/Data development/Configuration management/Project configuration.md#)\), the table will not be resolved to a scheduling dependency.

Under the premise of automatic parsing, you can avoid/increase the characters in some SQL statements to be automatically parsed into output name/input name by manually setting add/delete and input/output.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013937_en-US.png)

Selecting the table name and right-clicking, you can add or delete the output and input of all the table names that appear in the SQL statement. After the operation, the characters added to be input will be parsed as the output name of parent node , and the characters added to be output will be parsed as the output of the corrent node, otherwise the deletion of the operation will not be resolved.

**Note:** In addition to right-clicking the characters in the SQL statement, you can also modify the dependencies by adding comments. The specific code is as follows.

```
--@extra_input=table name --Add an input
--@extra_output=table name --Add an output
--@exclude_input=table name --Delete an input
 --@exclude_output=table name --Delete an output
```

## Customize Adding Dependencies {#section_kjs_2y5_2gb .section}

When the dependencies between nodes can not be accurately resolved through the SQL blood relationship, you can choose "no" in the figure below to self-configure dependencies.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900021179_en-US.png)

When auto-parsing column is set to "No", you can click **Automatic recommended** to enable the auto-recommended upstream dependency function. The system will recommend all other SQL node tasks that output the current node input table for you based on the SQL blood relationship of the project. You can select one or more tasks in the recommended list on demand and configure as the current node's upstream dependency tasks.

**Note:** The recommended nodes need to be submitted to the scheduling system the day before, and can be recognized by the automatic recommendation function after the data output on the second day.

Common scenarios:

-   The current task's input table is not equal to the upstream task's output table.
-   The current task's output table is not equal to the downstream task's input table.

In custom mode, you can configure dependencies in two ways.

-   Manually add dependent upstream nodes
    1.  Create three new nodes and the system will configure one output name for each of them by default.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013938_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013939_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013940_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013941_en-US.png)

    2.  Configure the upstream node task\_1 to depend on the root node of the project, and click Save.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013942_en-US.png)

    3.  Configure task\_2 to depend on the output name of task\_1, and click Save.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013943_en-US.png)

    4.  Configure task\_3 to depend on the output name of task\_2, click Save.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013944_en-US.png)

    5.  After the configuration is complete, click Submit to determine whether the dependency relationship is correct. If the submission is successful, the dependency configuration is correct.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013945_en-US.png)

-   Construct dependencies by dragging and dropping
    1.  Create three nodes: task\_1, task\_2, task\_3, and configure the upstream task\_1 to depend on the root node, then click Save.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013942_en-US.png)

    2.  Connect the three tasks by dragging and pulling.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900113947_en-US.png)

    3.  Check the dependency configuration of task\_2 and task\_3, you can see the dependent parent node output name that has been automatically generated.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900113948_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900113949_en-US.png)

    4.  After the configuration is complete, click Submit to determine whether the dependency relationship is correct. If the submission is successful, the dependency configuration is correct.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900013945_en-US.png)


## FAQ {#section_sg1_kxy_p2b .section}

Q: After automatic parsing, the submission fails. Error: Dependent parent node output MaxCompute\_DOC.tb\_3 does not exist and cannot submit this node. Please submit parent node task\_2 first.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16303/154751900113952_en-US.png)

A: There are two reasons for this.

-   The upstream node is not submitted, and you can try again after submission.
-   The upstream node has been submitted, but the output name of the upstream node is not MaxCompute\_DOC.tb\_3.

**Note:** Usually, the parent node output name and the current node output name that automatically parsed are obtained according to the table name after INSERT/CREATE/FROM. Make sure that the configuration is consistent with the way described in the section "Auto-parsing dependencies".

Q: In the output of the current node, the downstream node name and downstream node ID are all empty and cannot be entered. So what should I do?

A: If there is no sub-node for downstream of the current node, there is no content. After the sub-node is configured for downstream of the current node, the content is automatically parsed.

Q: What is the node's output name used for?

A: The node's "output name" is used to establish dependencies between nodes. For example, If the output name of node A is "ABC" and node B takes "ABC" as its input, the upstream and downstream relationship is established between nodes A and B.

Q: Can a node have multiple "output names"?

A: Yes. If a downstream node references an output name from the current node \(as the "parent node output name" of the downstream node\), it establishes a dependency with the current node.

Q: Can multiple nodes have the same "output name"?

A: No. The "output name" of each node must be unique in Aliyun account system. If multiple nodes output data to the same MaxCompute table, we recommend that you use "table name\_partition ID" as the output of these nodes.

Q: How do I not parse to an middle table when using auto-parsing dependencies?

A: Select the middle table name in the SQL code and right-click the **Remove Input** or **Remove Output**, and then perform the automatic parsing of the input and output again.

Q: How do I configure dependencise of the most upstream task?

A: In general, you can choose to depend on the root node of this project.

Q: Why did I search for the output name of the node B that does not exist when searching for the upstream node output name on the node A?

A: Because the search function is based on the submitted node information. If the output name of node B is deleted after the successful submission of node B and not submitted to the scheduling system, then the deleted output name of node B can still be found on node A.

Q: There are three tasks A, B, and C. How do I implement the task flow of A-\>B-\>C once an hour \(After A is completed, execute B, after B is completed, execute C\)?

A: The dependency of A, B, and C is set to the output of A as the input of B, the output of B is the input of C, also the scheduling periods of A, B, and C are set to hours.

