# Beginner's guide {#concept_l5r_z4p_r2b .concept}

If you are using DTplus products for the first time, log on to DTplus with your Alibaba Cloud account \(primary account\), and check whether the account is available by following the steps listed in Prepare Alibaba Cloud account topic. If the account is available, follow these steps to create a project.

## Procedure {#section_msf_kpp_r2b .section}

1.  Create an AccessKey.

    To assure smooth processing of data tasks in DataWorks, you must create an AccessKey. Similar to logging on with an account name and password, an AccessKey is primarily used to access permission verification between various Alibaba Cloud products and services. For more information, see [Alibaba Cloud Account Preparations](reseller.en-US/Preparation/Administrator Operations/Alibaba Cloud Account Preparations.md#).

    An AccessKey comprises of an AccessKey ID and an AccessKeySecret. Generally, the AccessKey is secured and kept confidential. Once the AccessKey is disabled, services that use this AccessKey fails to run and reports exceptions to users. If an AccessKey information changes, it can affect products and services that use this AccessKey. Therefore, you must keep a close eye on such changes and take appropriate actions timely.

    **Note:** The AccessKey is critical to your account. After creating the AccessKey, you must secure the AccessKey ID and AccessKeySecret. Do not reveal the AccessKey information to others under any conditions. If you suspect an information leak or other risks, you must promptly disable or modify your AccessKey.

2.  Select region and services.

    If a region does not contain any projects, it automatically shows you have no projects. You can proceed with create a project. By default, data development, O&M center, and data management are selected.

3.  Create a project.

    Only a primary account that has the data integration function can create a project, all other RAM users are treated only as users in the project.

    **Note:** This makes it necessary for a primary account to prepare a project environment for other RAM users.

    -   **Workspace name**: The project name must be 3 to 27 characters in length.
    -   **Display name**: The display name must not exceed 27 characters in length.
    -   **Enable scheduling frequency**: Controls scheduling system frequency in the current project. If it is disabled, it cannot schedule tasks periodically.
    -   **Allows editing tasks and code directly in this project**: the current project member has permission to create or edit code files in this project. If this function is disabled, the project member cannot create or edit code files.
    -   **Enable select result downloads in this project**: Determines whether data results from the selected statements can be downloaded in this project. If it is disabled, it cannot download the data query results from a selected statement.
    -   **MaxCompute project name**: You can create a MaxCompute project with the same name as the project name. 
    -   **MaxCompute access identity**: The personal account and the system account.
    -   **Quota group**: Quota is used to implement disk quotas.
4.  Click **Create Workspace**.

    After a workspace\(project\) is created, the project appears on the project list page. This page usually displays the project according to the time of creation or latest time of use.


