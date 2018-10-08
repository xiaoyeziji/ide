# Create a project {#concept_j2d_4nj_r2b .concept}

Check whether the primary account is available according to the [Prepare Alibaba Cloud account](intl.en-US/Preparation/Administrator operations/Prepare Alibaba Cloud account.md#) account. If the account is available, perform the following steps to create a project.

**Note:** Currently, the sub-account can also create a project, and the procedure is the same as main accounts.

## Procedure {#section_alx_psp_r2b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) by using a primary account.
2.  You can create the MaxCompute project in the following two ways.
    -   Click **Create Project** in **Common Functions** area.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16175/15389669978937_en-US.jpg)

    -   Go on Project List page, click **Create Project**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16175/15389669978938_en-US.jpg)

3.  Fill in the configuration items in the Create Project dialog box. Select a region and a calculation engine service.

    If you have not purchase the relevant services, it is directly display that there is no service available under the Region. The data development, O&M center, and data management are selected by default.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16175/15389669978939_en-US.png)

    **Choose Calculation Engion Services:**

    -   MaxCompute: MaxCompute is a big data processing platform developed by Alibaba independently. It is mainly used for batch structural data storage and processing, which can provide massive data warehouse solution and big data modeling service.
    -   Machine learning PAI: Machine learning refers to a machine that uses statistical algorithms to learn a large amount of historical data to generate empirical models, and use empirical models to guide businesses.
    **Choose DataWorks service:**

    -   Data integration: A data synchronization platform that provides stable, efficient, and elastically scalable services. The Data Integration is designed to implement fast and stable data movement and synchronization between multiple heterogeneous data sources in complex network environments. For more information, see [Data Integration overview](../../../../intl.en-US/User Guide/Data Integration/Data Integration introduction/Data Integration overview.md#).
    -   Data development: The Data Development helps you to design data computing processes according to your business demands and make mutually dependent tasks be automatically run in the scheduling system. For more information, see [Data development overview](../../../../intl.en-US/User Guide/Data development/Node type/Node type overview.md#).
    -   O&M center: The O&M Center is a place where tasks and instances are displayed and operated. You can view all your tasks in Task List and perform such operations on the displayed tasks. For more information, see [Operation center overview](../../../../intl.en-US/User Guide/Operation center/Operation center overview.md#).
    -   Data management: The Data Management module of the Alibaba Cloud DTplus platform displays the global data view and metadata details of an organization, and enables operations such as divided permission management, data lifecycle management, and approval and management of data table/resource/function permissions. For more information, see [Data management overview](../../../../intl.en-US/User Guide/Data management/Introduction.md#).
4.  Enter project basic information and set advanced settings.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16175/15389669978940_en-US.png)

    **Basic Information**

    -   **Project name**: The length of the project name is between 3 and 27 characters.
    -   **Display name**: The length of the display name is not more than 27 characters.
    -   **Project mode**: The project mode is a new feature introduced by the new version of DataWorks. It is divided into simple mode and standard mode. for the difference between two project development patterns, see [The differences between a simple pattern and a standard pattern](../../../../intl.en-US/Best Practices/Simple mode and standard mode.md#).
        -   Simple mode: One DataWorks project corresponding to a MaxCompute project, can not set development and production environment, you can only do simple data development, can not control the data development process and table permissions.
        -   Standard mode: One DataWorks project corresponding to two MaxCompute projects, can set development and production dual environment, improve code development specifications, and can strictly control the table permissions, prohibit the arbitrary operation of the production environment table, to ensure the data security of the production table.
    **Advanced Settings**

    -   **Enable scheduling frequency**: Control the current project whether to enable or disable the scheduling system, and if it is disabled, it can not periodically schedule tasks.
    -   **Enable select result downloads in this project**: Whether data results from select statement can be downloaded in this project, and if it is disabled, it cannot download the data query results from select statement.
    -   **For MaxCompute**
        -   Development Environment MaxCompute Project name: the default is the project name + "\_ dev" suffix, which can be modified.
        -   Development Environment MaxCompute access identity: the default is a personal account.
        -   Production Environment MaxCompute Project name: the default name is the same as the DataWorks project.
        -   Production environment MaxCompute access identity: The default is the production account, it is not recommended to modify.
        -   Quota group: Quota is used to implement disk quotas.
5.  When the configuration is complete, click **Create project**.

    After the project created successfully, you can view the content on the Project List page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16175/15389669978943_en-US.jpg)

    **Note:** 

    -   Once you become a project owner, it means that everything in the project is yours, and no one has permission to access your project before the authentication.
    -   For general users, it is not necessary to create a project. If you are added to a project, you can use the MaxCompute.

## Next step {#section_yh3_lxp_r2b .section}

Now, you have learned how to create a project, and you can continue to learn the next tutorial. In next tutorial, you can learn how to quickly complete the operations of data development and data O&M. For more information, see [Quick Start](../../../../intl.en-US/Quick Start/Instructions.md#).

