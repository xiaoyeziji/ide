# Version history {#concept_w2x_g2q_r2b .concept}

## DataWorks V2.0 release {#section_fz5_k2q_r2b .section}

Release Version: DataWorks V2.0

-   Release time July 25, 2018
-   Release scope: East China 2 deployment only
-   Release: DataWorks V2.0 adds business processes and components on the basis of DataWorks V2.0, it also improved the data R&D system, supports dual projects, isolated development and production, and ensures data development specifications to reduce error codes.

## Regions that support Dataworks 2.0 {#section_pgk_hjj_ggb .section}

Regions that support Dataworks 2.0:

-   East China 1
-   East China 2
-   North China 2
-   South China 1

## DataWorks V2.0 update list {#section_ap4_gz3_w2b .section}

|DataWorks V2.0 update list|
|:-------------------------|
|DataWorks V2.0 has upgraded the overall visual interaction and **data development** module's usage experience. Furthermore, DataWorks V2.0 provides four new modules, including intelligent monitoring, data protection, data quality and data service. For a smooth transition to the newest version, the following is a list of all DataWorks V2.0 updates. .|
|**Module name**|**Sub-Module**|**Comparison**|**DataWorks V1. 0**|**DataWorks V2.0**|**Improved effects**|
|MaxCompute Project|Project Management Mode|Management methods|A DataWorks project corresponds to a MaxCompute project.|Introducing DataWorks "Standard Mode" concept. Under this mode, a project corresponds to two MaxCompute projects, including development environment and production environment.\( See [Simple mode and standard mode](../../../../../reseller.en-US/Product Introduction/Simple mode and standard mode.md#) \)|Isolate risks to protect code stability in production environments.|
|Data development|Task development|Overall function|Performs a single task, workflow code writing, cycle scheduling configuration. After completion, it can be submitted to the operation center for automatic scheduling.| -   Renamed: Data Development
-   New: solutions, business process concepts
-   Deleted: workflow \(concept\)
-   Optimization: More intelligent SQL editor , task cycle configuration, more open dependency configuration.

 | 1.  SQL Editor: provides a more user-friendly and immersive SQL development experience.
2.  Task Management: **business processes**, **solutions** make it easy to manage complex development tasks.
3.  Task Scheduling: a more open scheduling system that can easily handle complex business scenarios.
4.  Other features: optimized new features to take care of user pain points in detail.

 |
|SQL R&D|Write SQL code on the page in the form of a single task or WorkFlow and test run it.|Provides a more intelligent SQL editor with **code highlighting**, **formatting**, **intelligent supplement**, **error tips**, **table structure display** and other user-friendly functions. At the same time, you can see the SQL internal **structure** visually in the graphical form.|
|Node configuration|Combine Business code through single nodes and workflow modes.|Introduces the **business process** concept of a a **workflow**. You can combine tasks in a business process, and manage different resources in business processes based on their needs \(all tasks, tables, resources, and functions must belong to a business process\). You can also consolidate business processes in one step through a solution, unifying business process management with strong relevance.|
|Cycle configuration|The workflow overall cycle configuration affects the periodic configuration of individual tasks.|All nodes can be configured separately and the scheduling cycle type is not affected by upstream and downstream nodes.|
|Dependency attributes|The dependencies between workflows are limited.|Task nodes in different business processes can be dependencies, and do not need to be dependencies of the business processes.|
|Script development|Overall function|A periodic task supplement usually used for non-periodic temporary data processing.|Same function, renamed as **manual business process**.|
|Resource management|Overall function|Manage all resources in the MaxCompute project as a separate tab, including jar, file, and archive.|As a sub-label in the business process, users can join resources involved in the business process on demand, while creating multi-tiered folders for management.|
|Function management|Overall function|A separate tag that manages the system and custom functions required for the MaxCompute SQL edit.|Can manage all functions as a separate tag or as a subtab in a work process that manages required functions.|
|Table query|Overall function|Shows alltables under the MaxCompute project, with the ability to preview the content, reference, and representation.|Same|
|Table Management \(new\)|Overall function|None|For developers to manage their own tables, life cycle settings, and table management. Supports table management features include modifying the category, description, field, partition, hide or show table , delete table, and more.|
|Temporary query \(new\)|Overall function|None|Used to test if the code matches expectations. Does not contain the following features: submit, publish, set schedule, and parameters function.|
|Component Management \(new\)|Overall function|None|Abstracts a large number of similar and reusable SQL code in SQL code blocks or node tasks, you can configure input and output arguments, and apply it to a variety of practical businesses.|
|Run history \(new\)|Overall function|None|Displays all task records that were run locally in the last three days, you can also view the task run results and provide simple filtering capabilities.|
|Results filter \(new\)|Overall function|None|Provides SQL results integrating the Excel component, allows users to obtain expected results by filtering, and ordering after the page prints the results.|
|Recycle Bin \(new\)|Overall function|None|Prevents business losses caused by mis-deleting user tasks, you can see all deleted nodes under the current item in the recycle bin and provide recovery capabilities.|
|Code global search|Overall function|None|You can input an incomplete string to find the MaxCompute SQL, Shell, Data Synchronization tasks, and quickly locate tasks you need to view or operate.|
|Release function|Overall function|Keeps the publishing function under DataWorksV1 standard mode project.|Rename: Project clone. Simple schema projects have the ability to clone tasks to other projects automatically.|
|O&M center|Task list|Feature|Search for tasks based on the node type, name, and owner.|Adds the ability to search for tasks through **business processes**, **solutions**, and **baseline names**.|From a business perspective, the task O&M matches new features on the task development interface.|
|Task O&amp;M|Feature|Search for tasks based on node type, name, owner, business date, and run date.|Adds the ability to search for tasks through **business processes**, **solutions**, and **baseline names**.|
|Alert|Feature|The monitoring alarm is based on the following: errors, completion, incomplete events and more.|Integrate **Baseline Monitoring**, **incident alarm**, **custom alarm** three functions to build a more intelligent, and complete alarm system.|
|Intelligent Monitoring \(new\)|[Alarm](../../../../../reseller.en-US/User Guide/O&M Center/Alarm/Alarm overview.md#) is a monitoring and analysis system for running DataWorks tasks. Based on monitoring rules and task operation, Intelligent Monitor decides whether to alert, when to alert, how to alert, and who to alert. Intelligent Monitor automatically selects the most appropriate alert time, alert method, and alert object.|Give you a one-stop access to data development and data on the cloud \(secure\) and a closed-loop experience of governance and data sharing.|
|Data quality \(new\)|[Data quality](../../../../../reseller.en-US/User Guide/Data quality/Data quality overview.md#) is a one-stop platform that supports quality verification, notification, and management services for a wide range of heterogeneous data sources.DQC uses a dataset as a monitoring object, supports monitoring MaxCompute data table, and DataHub real-time data stream. When changes are made to offline MaxCompute data changes, DQC verifies the data and blocks the production link to avoid diffusion of problematic data pollution. Furthermore, DQC provides verification of historical results. Thus, you can analyze and quantify data quality.

|
|Data Services \(new\)|[DataService studio](../../../../../reseller.en-US/User Guide/DataService studio/DataService studio overview.md#) provides the ability to quickly generate data APIs from data tables, enabling users to quickly register existing APIs to a data service platform for unified management and publishing. In addition, Data Service is connected to API Gateway. You can deploy APIs to API Gateway with one-click. Data Service is compatible with API Gateway to provide a secure, stable, low-cost, and easy-to-use data sharing service.|
|Data umbrella \(new\)|[Data Security Guard](../../../../../reseller.en-US/User Guide/Data security guard/Enter Data Security Guard.md#) provides data asset identification, sensitive data discovery, data classification, desensitization, monitor access ability, identify, alert and audit.|

