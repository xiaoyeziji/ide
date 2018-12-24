# Table management {#concept_qmg_yrt_q2b .concept}

The Table management module categorizes data tables and helps to manage information and operations for different tables in various categories. This enables the developers to manage their own data tables.  On the Manage Data Tables page, you can follow these steps on your tables: setting the lifecycle, managing tables \(including modifying the category, description, field, and partition of a table\), hiding and unhiding tables, and deleting tables.

## Table category {#section_i4p_xxv_q2b .section}

-   My favorite tables

    This section lists your favorite data tables. You can also remove the table from your favorite list.

-   My recently used tables

    This section displays the tables that you recently used. You can set the table lifecycle, manage tables \(including modifying the category, description, field, and partition of a table\), hiding and unhiding tables, and deleting tables. For more information, see the Manage tables section in this article.

-   Individual account table

    This section lists the data tables you have created within the organization. In other words, you are the owner of the tables as you are the current logon user.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826148590_en-US.png)

    You can search for the tables by table names and filter the tables according to the projects where the tables belong.  The operations available here are the same as those for My recently used tables.

-   Production account table

    This section lists the tables with owners configured as Computing Engine Accounts \(namely, the production account\) with a MaxCompute access identity.  The operations available here are the same as those for My recently used tables.

-   My managed tables

    If you are the project administrator, all the data tables in the project spaces you managed are displayed on this page. As an administrator, you can perform various operations on the tables such as modifying the table owner.


## Manage tables {#section_rkq_pyy_q2b .section}

-   Add tables to favorites

    The Data Management module allows you to add tables to your favorites list. You can click **Add to favorites** on the table details page to add the table to your favorite list. Similarly, to remove a table from favorites list, click **remove**, on the My Favorite Tables page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826148591_en-US.png)

-   Modify the lifecycle
    1.  Click the **LifeCycle** in the actions column of the list.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826148592_en-US.png)

    2.  Modify the table lifecycle in the **Lifecycle** dialog box.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826148593_en-US.png)

-   Modify table structure
    1.  Click **More** in the Actions column of the list and select **Table Management** to modify the table structure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826148594_en-US.png)

    2.  Modify the related information on the Table Management page.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826148595_en-US.png)

    3.  Click **Submit** to confirm the changes.
-   Hide a table

    The table owner or project administrator can hide a table to make table invisible to other members. 

    Click More in the Actions column of the list and select Hide to hide a table.  To unhide the table, select **Unhide**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826158608_en-US.png)

    A hidden table is marked with Hidden behind its name.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826158609_en-US.png)

    **Note:** The master account hidden table sub-accounts cannot view the hidden table content, click the appropriate prompt: table is hidden, contact the administrator or owner, sub-account hidden table master account can query the table contents.

-   Modify table owner

    The project administrator can modify the table owner by completing the following steps:

    1.  In the My managed tables section, click **More**in the Actions column of the list and select **Modify Owner**.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826158616_en-US.png)
    2.  Enter the cloud account name of the new owner in the Modify table owner dialog box. Note that the new owner must be a member of the project.
    3.  After the modification is complete, click Submit.
-   Delete a table
    1.  Click **More** in the Actions column of the list and select **Delete**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826158621_en-US.png)

    2.  Click **OK** to confirm the action. Once a data table is deleted, the table structure. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16351/15390826158622_en-US.png)

        Note that once you delete a table, all table data gets deleted and cannot be recovered. So, proceed with caution.


