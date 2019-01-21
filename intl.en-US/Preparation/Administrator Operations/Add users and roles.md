# Add users and roles {#concept_ey2_vnp_r2b .concept}

You can skip this article if DTplus is for your personal use. If DTplus is used for collaboration projects, you must create a Resource Access Management \(RAM\) user according to [Create a RAM user](reseller.en-US/Preparation/Administrator Operations/Create a RAM user.md#) and [Create Workspace](reseller.en-US/Preparation/Administrator Operations/Create Workspace.md#), then complete configurations as follows.

Alibaba Cloud DTplus products adopt the [RAM](https://www.alibabacloud.com/help/doc-detail/28627.htm) primary account or RAM user logon system.

## RAM user billing {#section_vgt_l1q_r2b .section}

Alibaba Cloud account \(or the primary account\) represents the ownership of Alibaba Cloud resources and is the main account for metering and billing resource usage. The primary account allows you to create RAM users for your enterprise to manage, and authorize permissions.

Created and managed by the primary account in RAM , the RAM user has no resources, and therefore cannot be metered and billed separately. All RAM users are controlled and paid by their primary account.

## Procedure { .section}

1.  Go to the Workspace List page of DataWorks, and click **Enter Project** after the corresponding project list.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16177/15480513348952_en-US.png)

2.  Click **Project Management** to go to the project management page.
3.  Click **Project Member Management** in the left-side navigation pane.
4.  Click **Add Members** to display the following dialog box.

    Click **Refresh** to synchronize the RAM user under the current Alibaba Cloud account in the RAM console to the selected list.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16177/154805133510476_en-US.png)

    **Note:** To add a new RAM user, click **RAM Console** in the preceding figure to open the Alibaba Cloud RAM page. For more information about adding and delivering a RAM user, see [Alibaba Cloud Account Preparations](reseller.en-US/Preparation/Administrator Operations/Alibaba Cloud Account Preparations.md#).

5.  Check RAM users in the New Members dialog box, select a role permission for these RAM users in batches, and click **OK** to add members.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16177/15480513358953_en-US.png)

    The existing members and their roles in the project can be viewed in the list and modified as required. You can also remove RAM users from the project.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16177/15480513358955_en-US.png)

    The project members can be assigned five roles namely, project administrator, deployer, developer, guest, and maintainer. The project creator is the project administrator by default.

    A brief description about the roles is as follows.

    -   **Development**: Manages the design of data development page and workflow maintenance.
    -   **O&M**: Manages the running conditions of all tasks in the O&M center page and handles them accordingly.
    -   **Project administrator**:

        In addition, to having full permissions as to that of developer and maintainer, the project administrator can perform project level operations, such as adding or removing project members, granting roles, and creating custom resource groups.

    -   **Deploy**: Reviews the task code and decides whether to submit it to the maintainer under the multi-project mode.
    -   **Visitor**: The visitor can view the workflow design and code content on the data development page with read-only permissions.
    -   **Safety Manager**: Only has s Data Security Guard permissions.

**Note:** If you already added the RAM user to the project, you can use the RAM user to log in to DataWorks. After logging in, you need to update the personal AccessKey information of the RAM user in the number plus console. For more information, see [RAM User Operations](reseller.en-US/Preparation/RAM User Operations.md#).

