# Add users and roles {#concept_ey2_vnp_r2b .concept}

If the DTplus is for your personal use, you may ignore this article. If the DTplus is for your collaboration with other users, first you must create a sub-account according to [Create a sub-account](intl.en-US/Preparation/Administrator operations/Create a sub-account.md#) and [Create a project](intl.en-US/Preparation/Administrator operations/Create a project.md#), then complete configurations as follows.

Like Alibaba Cloud, Alibaba Cloud DTplus products adopt the [RAM](https://www.alibabacloud.com/help/doc-detail/28627.htm) primary account/sub-account logon system.

## Sub-account billing {#section_vgt_l1q_r2b .section}

Alibaba Cloud account \(or the primary account\) represents the ownership of Alibaba Cloud resources and is the basic specification for metering and billing resource usage. The primary account allows you to create sub-accounts for your enterprise, manage, and authorize these sub-accounts.

Created and managed by the primary account in the RAM system, the sub-account has no resources, and therefore cannot perform metering and billing independently. All sub-accounts are controlled and paid by their primary account.

## Procedure { .section}

1.  Go to the project list page of the DataWorks, and click the **Enter Project** after the corresponding project list.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16177/15435669688952_en-US.png)

2.  Click **Project Management** to go to the project management page.
3.  Click **Project Member Management** in the left-side navigation pane.
4.  Click **Add Members** to display the following dialog box.

    Click **Refresh** to synchronize the sub-account under the current Alibaba Cloud account in the RAM console to the list to be selected.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16177/154356696910476_en-US.png)

    **Note:** To add a new sub-account, click **RAM Console** in the preceding figure to open the Alibaba Cloud Resource Access Management page. For specific procedures to add a sub-account and deliver it for use, see [Prepare Alibaba Cloud account](intl.en-US/Preparation/Administrator operations/Prepare Alibaba Cloud account.md#).

5.  Check sub-accounts in the New Members dialog box, select a role to be granted to these sub-accounts in batches, and click **OK** to add members.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16177/15435669698953_en-US.png)

    The existing members and their roles in the project can be viewed in the list and modified as needed. You can also remove sub-accounts from the project.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16177/15435669698955_en-US.png)

    The project members can be assigned five roles namely, project administrator, deployer, developer, guest, and maintainer. The project creator is the project administrator by default.

    A brief description about the roles is as follows.

    -   **Development**: Manages the design of data development page and the maintenance of workflow.
    -   **O&M**: Manages the running conditions of all tasks in the O&M center page and handles them accordingly.
    -   **Project administrator**: In addition to having full permissions of the developer and the maintainer, the project administrator can also perform operations on the project level, such as adding/removing the project members, granting roles, and creating custom resource groups.
    -   **Deploy**: Reviews the task code and decide whether to submit it to the maintainer only in the multi-project mode.
    -   **Visitor**: The visitor can view the workflow design and code content on the data development page with the read-only permissions.

**Note:** If you have already added the sub-account to the project, you can use the sub-account to log in to DataWorks. After logging in, you need to update the personal AK information of the sub-account in the number plus console. For more information, see [Sub-account operations](intl.en-US/Preparation/Sub-account operations.md#).

