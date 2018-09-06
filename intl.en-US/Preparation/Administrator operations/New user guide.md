# New user guide {#concept_l5r_z4p_r2b .concept}

If you are using the DTplus products for the first time, log on by using your Alibaba Cloud account \(primary account\), and check whether the account is available by following the steps in Prepare Alibaba Cloud account. If the account is available, follow these steps to create a project.

## Procedure {#section_msf_kpp_r2b .section}

1.  Real-name registration.

    To make purchases and use cloud products, your Alibaba Cloud account must be registered with a real-name. Log on to the Real-name registration page to complete the registration steps.

    **Note:** We recommend completing Real-name registration process in advance to assure smooth operation of the subsequent processes.

2.  Create an AccessKey.

    To assure smooth processing of tasks of data in DataWorks, you must create an AccessKey. Similar to the way you use the account name and password to log on, an AccessKey is primarily used to access permission verification between various Alibaba Cloud products and services. For more information, see [Prepare Alibaba Cloud account](intl.en-US/Preparation/Administrator operations/Prepare Alibaba Cloud account.md#).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16173/15362020748929_en-US.jpg)

    An AccessKey comprises of an AccessKey ID and an AccessKeySecret. Generally, the AccessKey is secured and kept confidential. Once the AccessKey is disabled, the services that use this AccessKey fail to run and report exceptions to users. If an AccessKey information changes, it can impact the products and services that use this AccessKey. Therefore, you must keep a close eye on such changes and take appropriate actions on time.

    ![](images/8930_en-US.jpg)

    **Note:** The AccessKey is highly critical to your account. After creating the AccessKey, you must secure the AccessKey ID and AccessKeySecret. Do not reveal this information to others on any condition. If you suspect of any information leak or other risks, you must promptly disable or modify your AccessKey.

3.  Select region and services.

    If a region does not contain any project, it automatically shows that you have no project. You can proceed to create a project. The data development, O&M center, and data management are selected by default.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16173/15362020748931_en-US.jpg)

4.  Create a project.

    Only a primary account, which has the data integration function can create a project, and the other sub-accounts are treated only as users in the project.

    **Note:** This makes it necessary for a primary account to prepare a project environment for other sub-accounts.

    ![](images/8932_en-US.jpg)

    -   **Project name**: The length of the project name is at least 3 and must not exceed 27 characters.
    -   **Display name**: The length of the display name must not exceed 27 characters.
    -   **Enable scheduling frequency**: Control the current project whether to enable or disable the scheduling system. If it is disabled, it cannot schedule tasks periodically.
    -   **Allows editing of tasks and code directly in this project**: the current project member has permission to create/edit code files in this project, and if closed, cannot create/edit code files.
    -   **Enable select result downloads in this project**: Whether data results from the selected statements can be downloaded in this project. If it is disabled, it cannot download the data query results from a selected statement.
    -   **MaxCompute project name**: A same project name of MaxCompute can be created when you create a project.
    -   **MaxCompute access identity**: The personal account and the system account.
    -   **Quota group**: Quota is used to implement disk quotas.
5.  Click **Create Project**.

    After a project is successfully created, the project appears on the project list page. This page usually displays the project according to its time of creation or latest time of use.

    ![](images/8933_en-US.jpg)


