# Version history {#concept_w2x_g2q_r2b .concept}

## Dataworks V2.0 release {#section_fz5_k2q_r2b .section}

Release Version: dataworks V2.0

-   Release time July 25, 2018
-   Release scope: East China 2 deployment only
-   Release: dataworks V2.0 adds the concept of business processes and components on the basis of dataworks V2.0, it has also improved the data research and development system, supported the development of dual projects, and isolated development and production, ensure data development specifications to reduce the emergence of error codes.

## Dataworks V2.0 update list {#section_ap4_gz3_w2b .section}

|Dataworks V2.0 update list|
|:-------------------------|
|Dataworks V2.0 has revolutionary improved the overall visual interaction and **data development** module's usage experience. Furthermore, Dataworks V2.0 provides four new modules, including intelligent monitoring, data protection, data quality and data service. To offer you smooth transition between old and new versions, here is a Dataworks V2.0 update list.|
|**Module name**|**Sub-Module**|**Comparison**|**Dataworks V1. 0**|**Dataworks V2.0**|**Improved effects**|
|Maxcompute Project|Project Management Mode|Management methods|A dataworks project corresponds to a maxcompute project.|Introducing the concept of "Standard Mode", a dataworks project corresponds to two maxcompute projects, respectively: development environment, production environment. \( See [Simple mode and standard mode](../../../../reseller.en-US/Best Practices/Simple mode and standard mode.md#) \)|Isolate risks to protect code stability in production environments.|
|Data development|Task development|Overall function|Perform single task, workflow code writing, cycle scheduling configuration. After completion, it can be submitted to the operation center for automatic scheduling.| -   Renamed: Data Development
-   New: solutions, business process concepts
-   Cut: workflow \(concept\)
-   Optimization: SQL editor more intelligent, better task cycle configuration, dependency configuration more open

 | 1.  SQL Editor: provides a more humanized, immersed SQL development experience.
2.  Task Management: **business processes**, **solutions** make it easy to manage complex development tasks.
3.  Task Scheduling: a more open scheduling system that can easily handle more complex business scenarios.
4.  Other features: new features to take care of the user experience in detail.

 |
|SQL research and development|Write SQL code on the page in the form of a single task or workflow and test run it.|Provides a more intelligent SQL editor with **code highlighting**, **formatting**, **intelligent replenishment**, **error tips**, **table structure display** and other humanized functions. At the same time, you can see the SQL internal **structure** visually in the graphical form.|
|Node configuration|Combine Business code through single nodes and workflow modes.|Introducing the concept of a **business process** instead of a **workflow**. You can freely combine tasks in a business process, and manage different resources into business processes according to their needs \(all tasks, tables, resources, functions must belong to a business process \). You can also consolidate business processes in one step through a solution, unifying management of business processes with strong relevance.|
|Cycle configuration|The workflow overall cycle configuration affects the periodic configuration of individual tasks.|All nodes can be configured separately and the scheduling cycle type is not affected by the upstream and downstream nodes.|
|Dependency attributes|The dependencies between workflows are limited.|Task nodes in different business processes can be dependent on each other, and do not need to rely on the overall business process.|
|Script development|Overall function|As a supplement to periodic tasks, it is usually used for non-periodic temporary data processing.|Same function, renamed as **manual business process**.|
|Resource management|Overall function|Manage all resources in the MaxCompute project as a separate tab, including: JAR/file/archive.|As a sub-label in the business process, users can join the resources involved in the business process on demand, while creating multi-tiered folders for management.|
|Function management|Overall function|As a separate tag, manage the system and custom functions that are required for the MaxCompute SQL edit.|Can exist as a separate tag and manage all functions, can also manage only the functions that are required for that business process as a subtab in the business process.|
|Table query|Overall function|Shows all the tables under the MaxCompute project, with the ability to preview the content, reference, and representation.|Same|
|Table Management \(new\)|Overall function|None|For developers to manage their own tables. life cycle settings, table management including modifying the category, description, field, partition , table hiding/unhiding, table deleting, and so on.|
|Temporary query \(new\)|Overall function|None|Used to test if the code matches the expectations with no commit, publish, set schedule parameters function.|
|Component Management \(new\)|Overall function|None|Abstract a large number of similar and reusable SQL code into SQL code blocks or node tasks, the user is free to configure input and output parameters, and apply it to a variety of practical business.|
|Run history \(new\)|Overall function|None|Display all task records that have run locally in the last three days, you can also view the results of the task run and provide simple filtering capabilities.|
|Results filter \(new\)|Overall function|None|Provide SQL results integrating the Excel component, allow users to get the desired results by simply filtering, filtering, and ordering after the page prints the results.|
|Recycle Bin \(new\)|Overall function|None|Used to prevent business losses caused by misdeleting of user tasks, you can see all the deleted nodes under the current item in the recycle bin and provide recovery capabilities.|
|Code global search|Overall function|None|You can input an incomplete string to find the MaxCompute SQL, Shell, Data Synchronization task, quickly locating tasks that you need to view or manipulate.|
|Release function|Overall function|Keep the publishing function under the DataWorksV1 standard mode project.|Rename: Project clone. Simple schema projects have the ability to clone tasks to other projects on their own initiative.|
|O&M center|Task list|Feature|Search for tasks based on node type, name, and owner.|Add the ability to search for tasks through **business processes**, **solutions**, **baseline names**.|From a business perspective, the task is operated to match the new features of the task development interface.|
|Task O&amp;M|Feature|Search for tasks based on node type, name, owner, business date, and run date.|Add the ability to search for tasks through **business processes**, **solutions**, **baseline names**.|
|Alert|Feature|Through error, completion, incomplete events and so on as the basis of monitoring alarm.|Integrate **Baseline Monitoring**, **incident alarm**, **custom alarm** three functions to build a more intelligent, complete alarm system.|
|Intelligent Monitoring \(new\)|[Alarm](../../../../reseller.en-US/User Guide/Operation center/Alarm/Alarm overview.md#) is a monitoring and analysis system for the running of DataWorks tasks. Based on monitoring rules and task operation, Intelligent Monitor decides whether or not to alert, when to alert, how to alert, and who to target. Intelligent Monitor automatically selects the most appropriate alert time, alert method, and alert object.|Give you one-stop access to data development and data on the cloud \(secure\) and a closed-loop experience of governance and data sharing.|
|Data quality \(new\)|[Data quality](../../../../reseller.en-US/User Guide/Data quality/Data quality overview.md#) is a one-stop platform that supports quality verification, notification, and management services for a wide range of heterogeneous data sources.DQC uses a dataset as a monitoring object, monitoring MaxCompute data table and the DataHub real-time data stream. When the offline MaxCompute data changes, DQC verifies the data and blocks the production link to avoid the diffusion of problematic data pollution. Furthermore, DQC provides verification of historical results. Thus, you can analyze and quantify data quality.

|
|Data Services \(new\)|[DataService studio](../../../../reseller.en-US/User Guide/DataService studio/DataService studio overview.md#) provides the ability to quickly generate data APIs from data tables, enables users to quickly register existing APIs to a data services platform for unified management and publishing. In addition, Data Service is connected to API Gateway. You can deploy APIs to API Gateway with one-click. Data Service works together with API Gateway to provide a secure, stable, low-cost, and easy-to-use data sharing service.|
|Data umbrella \(new\)|[Data Security Guard](../../../../reseller.en-US/User Guide/Data Security Guard/Enter Data Security Guard.md#) provides data asset identification, sensitive data discovery, data classification, desensitization, access ability to monitor, identify, alert and audit.|

