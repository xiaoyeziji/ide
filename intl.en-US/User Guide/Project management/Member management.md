# Member management {#concept_omy_tmh_r2b .concept}

On the Project Member Management page under the **Project Management** module of the Alibaba Cloud DTplus platform, you can manage and configure members of the current project.

## Page description {#section_zvz_hnh_r2b .section}

Click Project Member Manage in the left-side navigation pane on the Project Management page to enter the Project **User Management** page.

![](images/8741_en-US.png)

Concepts of listed items:

-   Member name: The alias/nick name of the member. The member name is the Alibaba Cloud account currently logged on by default.
-   Login name: The Alibaba Cloud account currently logged on.
-   Member role: The role of a member in the current DataWorks project \(project administrator, developer, maintenance personnel, deployer, or guest\). For specific permissions for different member roles, click the **Permissions List** to view.
-   Add members: The system can synchronize all the sub-user accounts under the main account and provide the searching and filtering functions. You can select one or more matched items in the search result and set roles for them in batch. 
-   Then, you can add selected members to the project, and these members can perform other data and project operations in the current project. You can select one or more matched items in the search result and set roles for them in batch. Then, you can add selected members to the project, and these members can perform other data and project operations in the current project.

    ![](images/8742_en-US.png)

    **Note:** If the member account to be added is not found in the Add member list, click **Refresh**, refresh the sub-account to the Count Plus. After the refresh is successful, the check box for the optional neutron account, transfer the sub-account to the account column that you added on the right, and select the role that you want to grant at the bottom, click **confirm** to complete the add operation.


## View permissions {#section_m1s_f4h_r2b .section}

In a MaxCompute\_SQL task, you can run the following statements to view your permissions:

```
show grants - View the permissions of the current user
show grants for <username> - View the permissions of a specified user, which is only available to the project administrator.
```

For more permission viewing commands, see [Permission check](https://www.alibabacloud.com/help/doc-detail/27936.htm).

