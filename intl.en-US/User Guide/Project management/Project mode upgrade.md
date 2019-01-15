# Project mode upgrade {#concept_xpy_3bn_cfb .concept}

In DataWorks V2.0, a standard project model, a standard project model, was introduced, A DataWorks project corresponds to two MaxCompute projects that isolate the development and production environments, increase the release process of the task to ensure the correctness of the task code.

## Benefits of a standard pattern {#section_avm_4bn_cfb .section}

In previous versions, the projects that you created were a DataWorks project that corresponds to a MaxCompute project, this is a simple pattern in DataWorksV2.0. Simple mode directly causes table permissions to be uncontrollable, such: just want to query some of the table for some of the students in the project, this scenario cannot be implemented directly in simple mode, because a DataWorks project corresponds to a MaxCompute, the development role permissions of DataWorks have the operation privileges of all tables under the MaxCompute project, so it is not possible to control table permissions precisely, and it is necessary to create a separate DataWorks project, to complete the isolation of data using the method of project isolation.

DataWorks V1. for the scenario of table permission control, a scenario is derived: manually bind two DataWorks projects, set the project to be a published Project for the B project, project A receives tasks published in Project B without having to develop code directly. So that project a became a project similar to the production environment, B is a project similar to the development environment.

There are also vulnerabilities in the mode of two DataWorks project bindings, project A is also a normal DataWorks project, can be directly in the data development module for task development, resulting in \(production\) the Code Update portal for the environment is not unique, and there is a logical vulnerability throughout the development process.

In response to the above-mentioned problems, we launched a standard project model.

In a standard project model, there are several benefits for data developers:

1.  A DataWorks project corresponds to two MaxCompute projects that perfectly separate the development and production computing engines, project members have only the permissions of the development environment, and default no permission actions on the Production Project's tables, improves the security of production data.
2.  In standard mode, the data development interface defaults to the task of operating the development environment, the tasks of the production environment are published to production through the publishing function, ensure the uniqueness of production environment code editing entry, improve the safety of production environment code.
3.  In standard mode, the development environment does not do periodic scheduling by default, it can reduce the consumption of computing resources under account, and guarantee the resources running in production environment task.

## Project mode upgrade {#section_mbg_qbn_cfb .section}

In DataWorks V1.0, we create simple schema projects, and that is, under simple schema projects, how can we upgrade to a standard model?

1.  In project management, you can see the buttons that are upgraded to the standard mode.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21243/154752189111707_en-US.png)

    As you can see from the figure above, the original project will become a production project in the dual project, the user needs to create a new development environment for MaxCompute, and the project name can be selected by itself. When you click confirm, DataWorks joins the members of the original project in the newly created MaxCompute development project, the members and roles of the original project are retained, however, the project member's permissions on the Production Project are abolished, and only the project owner has permissions on the production item.

    For example: A company has an a project on DataWorks, and after you click on the project upgrade, create a Development Environment Project, the members, roles, tables, and resources in the original a project are all created under the'dev Project \(only tables are created, do not clone the table data as well \). Member A1 \(development role\), B1 \(operation and maintenance role\), former project \), it also joins under the'dev project and retains the role permissions. Project A becomes a production project, the A1 and B1 users 'data permissions in Project A are abolished, by default, there is no select and drop permission for the table, and the data for the production item is directly protected. In the data studio interface, the default operation of the MaxCompute project is a'dev, to query the data of the production environment in the data development interface, you need to use the project name. The way the table is called. The data development interface can only edit code for the AHA Dev environment, to update the code in Project A, you can submit a task to the scheduling system only by the'dev, how to publish to the production environment for updates. A process of task release \(Audit\) was added to ensure the correctness of the production environment code.

2.  When you click on the project mode, the following prompts appear, and you need to enter the project name for the development environment.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21243/154752189111708_en-US.png)

    **Note:** After the project has been upgraded, you cannot directly access the data of the original project, and you need to apply for role permissions. The tables that you query in the data development interface, by default, are the tables of your development environment, to access the production table, you need to apply for the role permission after using the project name. The way the table name is accessed.


