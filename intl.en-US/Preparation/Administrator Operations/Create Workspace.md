# Create Workspace {#concept_j2d_4nj_r2b .concept}

Check whether the primary account is available according to the [Alibaba Cloud Account Preparations](reseller.en-US/Preparation/Administrator Operations/Alibaba Cloud Account Preparations.md#) account. If the account is available, perform the following steps to create a workspace\(project\).

**Note:** Currently, the RAM user can also create a workspace, following the same procedures as the primary account.

## Procedure {#section_alx_psp_r2b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) by using a primary account.
2.  You can create the MaxCompute project with the following two methods.
    -   Click **Create Workspace** in **Common Functions** area.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16175/15514202368937_en-US.jpg)

    -   Go on Workspace List page, click **Create Workspace**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16175/15514202368938_en-US.jpg)

3.  Enter the configuration items in the Create Workspace dialog box. Select a region and a calculation engine service.

    If you have not purchased the relevant services, the Region section will directly display it as no service is available . Data development, O&M center, and data management are selected by default..

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16175/15514202368939_en-US.png)

    **Choose calculation Engine services:**

    -   MaxCompute: MaxCompute is a big data processing platform developed by Alibaba Cloud. It is mainly used for batch structural data storage and processing, and can provide massive data warehouse solutions and big data modeling service.
    -   Machine learning PAI: Machine learning refers to a machine that uses statistical algorithms to learn a large amount of historical data to generate empirical models for business references.
    **Choose DataWorks service:**

    -   Data integration: A data synchronization platform that provides stable, efficient, and elastically scalable services. Data integration is designed to implement fast and stable data movement and synchronization between multiple heterogeneous data sources in complex network environments. For more information, see [Data integration overview](../../../../../reseller.en-US/User Guide/Data integration/Data integration introduction/Data integration overview.md#).
    -   Data development: Data development helps you design data computing processes based on business requirements and make task dependencies automatically run in the scheduling system. For more information, see [Data development overview](../../../../../reseller.en-US/User Guide/Data development/Node type/Node type overview.md#).
    -   O&M center: O&M center is a place where tasks and instances are displayed and operated. You can view all your tasks in Task List and perform such operations on the displayed tasks. For more information, see [Operation center overview](../../../../../reseller.en-US/User Guide/O&M Center/O&M center overview.md#).
    -   Data management: Data management in Alibaba Cloud DTplus platform displays the global data view and metadata details of an organization, and enables operations such as separate permission management, data lifecycle management, and approval and management of data table, resource, or function permissions. For more information, see [Data management overview](../../../../../reseller.en-US/User Guide/Data management/Introduction.md#).
4.  Enter project basic information and set advanced settings.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16175/15514202368940_en-US.png)

    **Basic Information**

    -   **Workspace name**: The workspace name must be between 3 and 27 characters in length.
    -   **Display name**: The display name must be less than 27 characters in length.
    -   **Mode**: The project mode is a new feature introduced by the new DataWorks version. It is separated into simple and standard mode. For differences between the two project development patterns, see [Simple mode and standard mode](../../../../../reseller.en-US/Product Introduction/Simple mode and standard mode.md#).
        -   Simple mode: A DataWorks project corresponding to a MaxCompute project, cannot set the development and production environment. Under this mode, you can only perform simple data development, and cannot control the data development process and table permissions.
        -   Standard mode: A DataWorks project corresponding to two MaxCompute projects can set both the development and production environments, improve code development specifications, and strictly control table permissions, prohibits arbitrary operation of the production environment table, and ensure the data security of the production table.
    **Advanced Settings**

    -   **Enable scheduling frequency**: Controls the current project scheduling system .If it is disabled, it cannot periodically schedule tasks.
    -   **Enable select result downloads in this project**: Whether data results from select statement can be downloaded in this project, and if it is disabled, it cannot download data query results from the select statement.
    -   **For MaxCompute**
        -   Development Environment MaxCompute Project name:The default name is the project name + "\_ dev" suffix, and can be modified.
        -   Development Environment MaxCompute access identity: The default identity is a personal account.
        -   Production Environment MaxCompute Project name: The default name is the same as DataWorks project.
        -   Production environment MaxCompute access identity: The default name is the production account. We do not recommend modifying this name.
        -   Quota group: Quota is used to implement disk quotas.
5.  When the configuration is complete, click **Create Workspace**.

    After the workspace is created successfully, you can view content on the Workspace List page.

    **Note:** 

    -   Once you become a project owner, you have full ownership over all project content, and anyone that requires project access must apply for permission. first .
    -   General users do not have to create a workspace\(project.\) If you are added to a project, you can use MaxCompute.

## Next step {#section_yh3_lxp_r2b .section}

Now that you have learned about project creation, you can move on to the next topic. In the next topic, you will learn how to quickly complete data development operations and data O&M. For more information, see [Quick Start](../../../../../reseller.en-US/Quick Start/Instructions.md#).

