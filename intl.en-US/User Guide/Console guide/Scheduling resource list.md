# Scheduling resource list {#concept_t2k_lvg_r2b .concept}

On the DataWorks console, you can view all the scheduling resources under the current account on the Scheduling Resource List page. On this page, you can create scheduling resources, search for a resource by entering its name, and can also perform operations on the expected resource.

## Procedure {#section_dhr_f4g_r2b .section}

1.  Log on to the [DataWorks](https://www.alibabacloud.com/product/ide) product details page as an organization administrator \(main account\).
2.  Click **DataWorks console** to enter the console overview page.
3.  Navigate to the **Scheduling Resource List** page.
    -   Description of the items listed on this page is as follows:
        -   Resource name: The name of the scheduling resource group, which consists of letters \(a-z\), underscores\(\_\), and numbers \(0-9\) with a length no greater than 60 characters. Once created, the name cannot be changed.
        -   Network type: The network type used by the ECS server is added as a scheduling resource. The types include VPC and classic networks.
            -   Classic network: IP addresses are centrally allocated by Alibaba Cloud. Classic networks are easy to configure and use. This network type is suitable for users who demand quick accessibility to ECS and emphasize on easy and convenient operations.
            -   VPC: A VPC is a logically isolated private network. Network topologies and IP addresses can be customized. VPC supports private line connection and is suitable for users who are familiar with network management.
        -   Server: The name of the server contained in the current scheduling resource.
        -   Operation type:
            -   Initialize the server: Enter a machine initialization statement as prompted.
            -   Modify the server: Modify server configurations of the current scheduling resource, such as adding or deleting a server and changing the maximum number of concurrent server tasks.
            -   Modify owner project: You can allocate the current scheduling resource to a specific project. This operation can only be performed by the main account that activated the service. After creating the project, you can use an existing ECS by modifying the owner project.
    -   Add scheduling resources: For more information, see [Add scheduling resources](intl.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).

## What are scheduling resources? {#section_ndz_nwg_r2b .section}

Scheduling resources are used to perform or distribute the tasks from the scheduling system. The scheduling resources of DataWorks are divided into the following two types.

-   Default scheduling resource.
-   Custom scheduling resource.

    Custom scheduling resources are the user-purchased ECSs, which can be configured as scheduling servers for performing distributed tasks. The organization administrator \(main account\) can create custom scheduling resources, which contain several physical machines or ECSs to perform data synchronization, SHELL, ODPS\_SQL, and OPEN\_MR tasks.


## Usages of scheduling resource list {#section_pdz_nwg_r2b .section}

-   Add resource groups and resource group servers.
-   Manage the relationship between resource groups and projects so that a resource group can be shared by multiple projects.
-   You can purchase ECSs and configure them as scheduling resources when a number of tasks are waiting for resources, which improves the efficiency in running scheduling tasks.

