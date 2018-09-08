# Project list {#concept_yrh_vng_r2b .concept}

In the Alibaba Cloud DTplus console, you can view all the projects under the current account of the **Project List** page. Enter project to configure projects, change the calculation services, create, activate, disable, and delete projects.

## Procedure {#section_dhr_f4g_r2b .section}

1.  Log on to the DTplus console and go to [DataWorks](https://www.alibabacloud.com/product/ide) product details page as an organization administrator \(primary account\).
2.  Click **DataWorks console** to enter the console overview page.
3.  Navigate to the **Project List** page to view all the projects under the current account.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721228729_en-US.jpg)


## Create Project {#section_jhg_s4g_r2b .section}

**Note:** Currently, only primary accounts are allowed to create projects.

1.  Click **Create Project**, select a region and a calculation engine service.

    The new project is created under the current region. You may need to purchase related services for the region. The data development, O&M center, and data management are selected by default.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721228730_en-US.png)

    -   Choose Calculation Engine Services
        -   MaxCompute: MaxCompute is a big data processing platform developed by Alibaba independently. It is mainly used for batch structural data storage and processing, which can provide massive data warehouse solution and big data modeling service. For more information, see  [MaxCompute documentation](https://www.alibabacloud.com/product/maxcompute).
        -   Machine learning PAI: Machine learning refers to a machine that uses statistical algorithms to learn a large amount of historical data to generate empirical models, and use empirical models to guide businesses.
    -   Choose DataWorks services
        -   Data integration: A data synchronization platform that provides stable, efficient, and elastically scalable services. The Data Integration is designed to implement fast and stable data movement and synchronization between multiple heterogeneous data sources in complex network environments. For more information, see [Overview of data integration](intl.en-US/User Guide/Data Integration/Data integration product brief/Overview of data integration.md#).
        -   Data development: The data development helps you to design data computing processes according to your business demands and make mutually dependent tasks be automatically run in the scheduling system. For more information, see [Data Development Overview](intl.en-US/User Guide/Data Development/Solution.md#).
        -   O&M center: The O&M center is a place where tasks and instances are displayed and operated. You can view all your tasks in Task List and perform such operations on the displayed tasks.  For more information, see [Operation center overview](intl.en-US/User Guide/Data Development/Operation center/Operation center overview.md#).
        -   Data management: The data management module of the Alibaba Cloud DTplus platform displays the global data view and metadata details of an organization, and enables operations such as divided permission management, data lifecycle management, and approval and management of data table/resource/function permissions.  For more information, see [data management overview](intl.en-US/User Guide/Data Development/Configuration management/Data management/Basics.md#).
2.  Configure the basic information and advanced settings for the new project.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721228731_en-US.png)

    -   Basic configuration
        -   project name: The length of the project name is between 3 and 27 characters.
        -   display name: The length of the display name is not more than 27 characters.
    -   Advanced configuration
        -   Enable scheduling frequency: Control the current project whether to enable or disable the scheduling system, and if it is disabled, it can not periodically schedule tasks.
        -   Enable select result downloads in this project: Whether data results from select statement can be downloaded in this project, and if it is disabled, it cannot download the data query results from select statement.
    -   MaxCompute configuration
        -   Development Environment Maxcompute Project name: the default is the project name + "\_ dev" suffix, which can be modified.
        -   Development Environment Maxcompute access identity: default is a personal account.
        -   Production Environment Maxcompute Project name: the default name is the same as the Dataworks project.
        -   Development Environment Maxcompute access identity: the default is the production account, it is recommended not to change.
        -   Quota group: Quota is used to implement disk quotas.

When the project is created successfully, the project list displays the corresponding content.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721228732_en-US.jpg)

-   Project Status: the project is generally divided into normal, initialization, initialization failure, deleting and deleted five states. Creating a project is initially display initialized state, and then generally show the results of initialization failure or normal.

    After the project is created successfully, you can perform the disable and delete operations. After the project is disabled, you can also activate and delete the project, and the project is normal after it is activated.

-   Subscribe to a service: Your mouse moves on to the service, and all the services you have opened are displayed. Generally, the normal service icon displays blue, the arrears service icon is red and there is corresponding arrears sign, if the arrears service have been deleted, it is displayed in gray, and the arrears service is deleted automatically after 7 days.

**Note:** 

-   Once you become a project owner, it means that everything in the project is yours, and no one has permission to access your project before the authentication.
-   For general users, it is not necessary to create a project. If you are added to a project, you can use the MaxCompute.

## Configure a project {#section_ufp_wsg_r2b .section}

You can configure some basic and advanced attributes of the current project by configuring project operations, mainly manages and configures space, scheduling, and so on.

Click **Configuration** for the project to be configured.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721228733_en-US.jpg)

## Enter Project {#section_odk_gtg_r2b .section}

Click **Enter Project** to configure a project, go to the Data Development page for specific operations.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721238734_en-US.jpg)

## Change the calculation services {#section_kb2_rtg_r2b .section}

Changing services is generally the operation of calculation services and DataWorks services. First, you must purchase a service, and then you can choose a corresponding service to modify it. The mode of payment is automatically displayed based on your purchase. You can recharge, upgrade, downgrade, and renew your MaxCompute.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721238735_en-US.jpg)

-   Recharge: You can recharge your services when the services receive an overdue warning.
-   Upgrade/Downgrade: If your Pay-As-You-Go resource of MaxCompute is unable to meet your business demand, you can upgrade the resource by purchasing more services. to upgrade the resource.
-   Renew: You can renew your package when the package expired, or the system freezes the corresponding instances that contained in this package. For more information, see [Renewal Management](https://www.alibabacloud.com/help/doc-detail/74875.htm).

**Note:** 

-   Subscription: Only display the Recharge button.
-   Pay-As-You-Go: All buttons are displayed.

## Delete or disable a project {#section_vcm_f5g_r2b .section}

Click **More** after the corresponding item name to delete and disable the item.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721238736_en-US.jpg)

-   Delete a project

    After selecting **Delete Project**, fill in the verification code in the dialog box and click **confirm**.

    **Note:** 

    -   The verification code is not changed.
    -   The delete project operation is irreversible, use it carefully.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721238737_en-US.jpg)

-   Disable a project

    Once a project is disabled, the cycle scheduling task in the project stops generating instances. The instances which are generated before the status changes to disabled, run normally. However you cannot log on to the project to view their corresponding status.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15363721238738_en-US.jpg)


