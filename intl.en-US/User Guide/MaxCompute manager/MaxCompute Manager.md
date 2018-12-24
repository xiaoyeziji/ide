# MaxCompute Manager {#concept_ey5_rn3_r2b .concept}

The MaxCompute Manager provides system status monitoring, resource group allocation, and task monitoring for system operaters. This article introduces how to use the MaxCompute Manager.

## Prerequisite {#section_att_g43_r2b .section}

-   You should already have purchased MaxCompute pre-paid CU resources and a quantity of 60 CUs or more.

    **Note:** You can only take complete advantage of computing resources and MaxCompute Manager when you have sufficient CUs. If you disable the AK for the master account, it will result in the failure to use MaxCompute Manager with the corresponding sub-account.


You can log on the [DataWorks management console](https://partners-intl.aliyun.com)，click **CU Manage**.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16417/15390838908825_en-US.jpg)

## System Status {#section_mvr_cp3_r2b .section}

On System Status page, you can see the consumption of CU computing resources and current storage.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16417/15390838908827_en-US.png)

-   Quotas: You can select the resource group you want to view and find its consumption information and current storage.
-   Time Length: You can select the time periods for the selected resource group. With different time periods, resource group data are displayed with different time granularities.

## Quota settings {#section_dgn_mp3_r2b .section}

A quota refers to a resource group. For example, if you purchased 100 CUs, you have a total quota of 100 CUs. You can create a new quota using MaxCompute Manager. Operaters can easily isolate the resources of each project to ensure that the calculation resources of the important projects are sufficient.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16417/15390838908829_en-US.png)

-   **Create Quota**: Create a quota, and assign projects to it. Created quotas cannot be deleted if there is an project under the current quota.
-   **Modify CU Usage Limit**: You can modify the minimum CUs used by a quota.
-   **Move Project**: You can move projects under the current quota to another quota.
-   **Delete**: The quota cannot be deleted if there is an project under the current quota.

**Note:** 

Max is the largest assigned resource, and Min is the smallest guaranteed resource.

## Instance Query {#section_nmv_qq3_r2b .section}

You can view the current task queuing status, such as which task has occupied the resource. Then you can analyze your task and decide if you want to stop it.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16417/15390838908833_en-US.png)

You can specify the quota and the project name to filter the tasks.

-   **Instance ID**: Each MaxCompute task has an instance. You can jump to the logview page by clicking instance ID, view specific task progress.
-   **Account**: Based on this account information, you can find the person responsible for the task.
-   **MaxCompute Project**: The project to which the instance belongs.
-   **CPU Usage**: CPU used by the quota.
-   **Memory Usage**: Memory used by the quota.
-   **Submitted At**: The commit time of the current instance.
-   **Waiting Time**: How much time spent on waiting for resources.
-   **Actions**: You can check the status of the instance. Both the current status and historical status are displayed.

