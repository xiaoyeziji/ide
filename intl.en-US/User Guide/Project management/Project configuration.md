# Project configuration {#concept_dv1_m4h_r2b .concept}

You can use the **Project Management** page in the administration console, manages and configures the properties of the current project space.

## Procedure {#section_zsd_t4h_r2b .section}

1.  Log in to the dataworks management console and navigate to the **Project List** page.
2.  Click **Config** after the corresponding project to enter the dataworks project configuration page.
3.  1. Configure your project as needed,

    ![](images/8744_en-US.png)

    -   Basic Attributes
        -   Project name: the name of the current project in dataworks, only letters or numbers \(must begin with letters\) are supported, not case-sensitive. It is the unique identifier of the project and cannot be changed once created.
        -   Project display name: The project display name of the current project in dataworks, used to identify the project, letters, numbers, or Chinese are supported and can be modified.
        -   Project Owner: the owner of the current project, who has permission to delete and disable the project, and the identity cannot be changed.
        -   Creation date:The date on which the current project is created. Alibaba Cloud's Chinese sites observes the time zone UTC+08:00 and cannot be changed.
        -   Status: the item is divided into four states: initialization, initialization failure, normal, and disable.
            -   The status of a new project is initializing.
            -   The status becomes initialization failed if the creation fails, in which case you can try it again.
            -   The normal item can be disabled by the Administrator, and all features of the item are unavailable and data is retained, tasks that have been submitted perform normally.
            -   The disabled project can be reset to be normal by using the restoration function.
        -   Description: The description of the current project, which is used to comment on the project-related information, you can edit the changes, supports 128 Chinese, letters, symbols, or numbers.
        -   Project mode: simple mode and standard mode.
        -   Enable scheduling cycle: This option determines whether to enable the scheduling system for the current project. If it is off, you cannot schedule tasks cyclically.
    -   SandBox whitelist \(IP address or domain name that can be accessed by configuring Shell\)

        With SandBox whitelist configured here, even if the Shell task run on the default Resource Group, you can also access the IP directly \(where the whitelist can be configured with IP and domain names \).

    -   Calculation engine information

        ![](images/8745_en-US.png)

        -   Development Environment Project name: Current dataworks project, project name of the maxcompute Project Development Environment used by the underlying layer \(this maxcompute project acts as a resource for calculation and storage\).
        -   Production Environment Project name: the name of the project for the current dataworks project, the maxcompute project production environment that is used at the bottom.
        -   Development Environment access identity: default is a personal account, not modifiable.
        -   Production Environment System Account: Default select SYSTEM account. Project leader's account execution SQL uses the main account's AK, personal Accounts Execute SQL using sub-account AK, the system account has the highest authority to operate a table of all the items under this account, A personal account can only operate on a table with permission.

            **Note:** When the production environment system account is using a personal account, tasks that run in a production environment may fail in large quantities due to insufficient permissions, please be careful.


