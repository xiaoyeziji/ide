# Workspace List {#concept_yrh_vng_r2b .concept}

In the Alibaba Cloud DTplus console, you can view all workspaces under the current account of the **Workspace List** page. Enter workspace to configure workspaces, change calculation services, create, activate, disable, and delete workspaces.

## Procedure {#section_dhr_f4g_r2b .section}

1.  Log on to the DTplus console and go to [DataWorks](https://www.alibabacloud.com/product/ide) product details page as an organization administrator \(primary account\).
2.  Click **DataWorks console** to enter the console overview page.
3.  Go to the **Workspace List** page to view all workspaces under the current account.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440538729_en-US.jpg)


## Create Workspace {#section_jhg_s4g_r2b .section}

1.  Click **Create Workspace**, select a region and a calculation engine service.

    The new workspace is created under the current region. You may need to purchase related services for the region. Data development, O&M center, and data management are selected by default.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440538730_en-US.png)

    -   Choose Calculation Engine Services
        -   MaxCompute: MaxCompute is a big data processing platform developed by Alibaba. It is mainly used for batch structural data storage and processing, which can provide massive data warehouse solution and big data modeling service.
        -   Machine learning PAI: Machine learning refers to a machine that uses statistical algorithms to learn a large amount of historical data to generate empirical models for business references.
    -   Choose DataWorks services
        -   Data integration: A data synchronization platform that provides stable, efficient, and elastically scalable services. Data integration is designed to implement fast and stable data movement and synchronization between multiple heterogeneous data sources in complex network environments. For more information, see [Data Integration Overview](reseller.en-US/User Guide/Data Integration/Data Integration Introduction/Data Integration Overview.md#).
        -   Data development: Data development helps you design data computing processes according to business requirements and automatically run dependent tasks in the scheduling system. For more information, see [Data Development Overview](reseller.en-US/User Guide/Data development/Solution.md#).
        -   O&M center: The O&M center is a place where tasks and instances are displayed and operated. You can view all your tasks in Task List and perform such operations on the displayed tasks. For more information, see [Operation center overview](reseller.en-US/User Guide/Operation center/Operation center overview.md#).
        -   Data management: Data management of Alibaba Cloud DTplus platform displays the global data view and metadata details of an organization, and enables operations, such as divided permission management, data lifecycle management, and approval and management of data table, resource, and function permissions. For more information, see [data management overview](reseller.en-US/User Guide/Data management/Introduction.md#).
2.  Configure the basic information and advanced settings for the new workspace.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440538731_en-US.png)

    -   Basic configuration
        -   Workspace Name: The workspace name must be 3 to 27 characters in length.
        -   Display Name: The display name must be 27 characters in length.
    -   Advanced configuration
        -   Enable scheduling frequency: Controls whether to enable the scheduling system in the current workspace. If the scheduling frequency is disabled, it cannot periodically schedule tasks.
        -   Enable select result downloads in this workspace: When this configuration is enabled, data results from select statement can be downloaded in this workspace. When this configuration is disabled, it cannot download the data query results from select statement.
    -   MaxCompute configuration
        -   Development Environment MaxCompute Workspace name: The default name is the workspace name + "\_ dev" suffix, which can be modified.
        -   Development Environment MaxCompute access identity: The default is a personal account.
        -   Production Environment MaxCompute Workspace name:The default name is the same as the DataWorks workspace.
        -   Development Environment MaxCompute access identity: The default access identity is the production account. We recommend you do not change the default setting.
        -   Quota group: Quota is used to implement disk quotas.

When the workspace is created successfully, the Workspace List displays the corresponding content.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440538732_en-US.jpg)

-   Workspace Status: the workspace is generally divided into normal, initialization, initialization failure, deleting and deleted five states. Creating a workspace initially displays an initialized state, and then generally shows the results of initialization failure or normal.

    After the workspace is created successfully, you can perform disable and delete operations. After the workspace is disabled, you can also activate and delete the workspace. The workspace is normal after it is activated.

-   Subscribe to a service: Your mouse moves on to the service, and all the services you opened are displayed. Generally, the normal service icon display is blue, the outstanding payment service icon is red. If the outstanding payment service has been deleted, it is displayed in gray, and the outstanding payment service is automatically deleted after 7 days.

**Note:** 

-   Once you become a workspace owner, it means you have full ownership over the workspace, and anyone that wants to access your workspace must apply for permission.
-   For general users, you do not have to create a workspace. If you are added to a workspace, you can use MaxCompute.

## Configure a workspace {#section_ufp_wsg_r2b .section}

You can configure some basic and advanced attributes of the current workspace by configuring workspace operations, mainly manage and configure space, scheduling, and more.

Click **Configuration** for the workspace to be configured.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440548733_en-US.jpg)

## Enter workspace {#section_odk_gtg_r2b .section}

Click **Enter Workspace** to configure a workspace, go to the Data Development page for specific operations.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440548734_en-US.jpg)

## Modify service {#section_kb2_rtg_r2b .section}

Modify services is usually for changing calculation engine services and DataWorks. To change services, you must purchase a service, and then choose a corresponding service to modify it. Based on your purchase, the payment mode is automatically displayed . You can top up, upgrade, downgrade, and renew your MaxCompute.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440548735_en-US.jpg)

-   Top up: You can top up your services when the services receive an overdue warning.
-   Upgrade/Downgrade: If your Pay-As-You-Go resource of MaxCompute is unable to meet your business demand, you can upgrade the resource by purchasing more services.
-   Renew: When the package expires, you can renew your package or the system freezes corresponding instances contained in this package.

**Note:** 

-   Subscription: Only displays the Top Up button.
-   Pay-As-You-Go: All buttons are displayed.

## Delete or disable a workspace {#section_vcm_f5g_r2b .section}

Click **More** after the corresponding item name to delete and disable the item.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440548736_en-US.jpg)

-   Delete a workspace

    After selecting **Delete Workspace**, enter the verification code in the dialog box and click **confirm**.

    **Note:** 

    -   The Delete Workspace verification code is always YES .
    -   The delete workspace operation is irreversible,exercise caution when performing this operation.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440548737_en-US.jpg)

-   Disable a workspace

    Once a workspace is disabled, the cycle scheduling task stops generating instances. The instances generated runs automatically before being disabled. However, you cannot log on to the workspace to view their status.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16187/15481440548738_en-US.jpg)


