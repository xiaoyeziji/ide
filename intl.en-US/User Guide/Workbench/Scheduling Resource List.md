# Scheduling Resource List {#concept_t2k_lvg_r2b .concept}

On the DataWorks console, you can view all scheduling resources under the current account of the Scheduling Resource List page. On this page, you can create scheduling resources, and search for resources by its name, and perform operations on the expected resource.

## Procedure {#section_dhr_f4g_r2b .section}

1.  Log on to the [DataWorks](https://www.alibabacloud.com/product/ide) product details page as an organization administrator \(main account\).
2.  Click **DataWorks console** to enter the console overview page.
3.  Navigate to the **Scheduling Resource List** page.
    -   Description of the items listed on this page is as follows:
        -   Resource name: The scheduling resource group name must be 60 characters in length, and consist of letters, underscores\(\_\), and numbers . The resource name cannot be changed.
        -   Network type: ECS server network types including VPC and classic networks are added as a scheduling resource.
            -   Classic network: IP addresses are centrally allocated by Alibaba Cloud. Classic networks are easy to configure and use. This network type is suitable for users who require quick accessibility to ECS and emphasize easy and convenient operations.
            -   VPC: A VPC is a logically isolated private network. Network topologies and IP addresses can be customized in VPC, and supports private line connection which makes it suitable for users that are familiar with network management.
        -   Server: The server name contained in the current scheduling resource.
        -   Operation type:
            -   Initialize the server: Enter the machine initialization statement.
            -   Modify the server: Modify current scheduling resource server configurations, such as adding or deleting a server and changing the maximum number of concurrent server tasks.
            -   Modify owner project: You can allocate the current scheduling resource to a specific project. This operation can only be performed by the main account that activated the service. After creating the project, you can use an existing ECS by modifying the owner project.

## What are scheduling resources? {#section_ndz_nwg_r2b .section}

Scheduling resources are used to perform or distribute tasks from the scheduling system. DataWorks scheduling resources are divided into the following two types.

-   Default scheduling resource.
-   Custom scheduling resource.

    Custom scheduling resources are user-purchased ECSs, which can be configured as scheduling servers for performing distributed tasks. The organization administrator \(main account\) can create custom scheduling resources, which contains several physical machines or ECSs to perform data synchronization, SHELL, ODPS\_SQL, and OPEN\_MR tasks.


## Usages of scheduling resource list {#section_pdz_nwg_r2b .section}

-   Add resource groups and resource group servers.
-   Manage the relationship between resource groups and projects so that a resource group can be shared by multiple projects.
-   You can purchase ECSs and configure them as scheduling resources when a number of tasks are waiting for resources, which improves running scheduling tasks efficiency.

