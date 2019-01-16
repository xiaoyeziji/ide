# Create a table {#concept_art_brt_q2b .concept}

Generally, you must create tables during data development to store the results of data synchronization and data processing. The Data Management module of Alibaba Cloud DTplus platform provides two ways to create a table.

**Note:** Statement-based table creation The classification can facilitate metadata management for numerous businesses in the organization. For more information on creating tables with the maxcompute client, see [Create tables](intl.en-US/User Guide/Data management/Create a table.md#).

## Prerequisites {#section_c1g_ll1_r2b .section}

-   Real-name registration for cloud accounts to generate the access ID and AccessKey.

    The cloud account used to build the table is the current logon account, you must have access Sid and accesskey to request a table to be built by maxcompute, so the cloud account must have real name authentication to generate access Sid and accesskey. For more information, see [Prepare Alibaba Cloud account](../../../../../intl.en-US/Preparation/Administrator Operations/Alibaba Cloud Account Preparations.md#).

-   Log on to Alibaba Cloud official website using the cloud account.

    You must authorize the Alibaba Cloud account before creating tables. MaxCompute project owners can directly run the authorization statement to authorize the permissions. Examples are as follows:

    ```
    use projectname;  --Open a project
    add user aliyun$Alibaba Cloud account;  --Add an user
    Grant CreateInstance,CreateTable,List ON PROJECT projectname TO aliyun$Alibaba Cloud account;  --Authorize the user
    ```

    **Note:** \>The tables are created using the Alibaba Cloud account currently logged on, so the owner of the tables is the account currently logged on.


## Visualization of creating a table {#section_hlh_wq1_r2b .section}

1.  Enter the [DataWorks management console](https://workbench.data.aliyun.com/console) as a developer, and click Enter workspace after the corresponding project under the project list.
2.  Click Data Management in the upper navigation pane and navigate to Manage Data Tables page.
3.  Click **Create table**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16346/15476194008649_en-US.png)

4.  Complete the configurations of the Basic information steps in the Create table dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16346/15476194008650_en-US.png)

    Parameters:

    -   Project Name: The list shows the MaxCompute projects that the user is currently in.
    -   Table Name: It may contain letters, digits, and underscores.
    -   Alias: Chinese name of the table to be created.
    -   Category: the current table is in a category that supports a maximum of four levels. Class navigation, configuration see [Manage config](intl.en-US/User Guide/Data management/Manage config.md#).
    -   Description: brief description of the table to be created.
    -   Lifecycle: The lifecycle function of MaxCompute. Data in the table \(or partition\) that has not been updated within the period of time specified by "Lifecycle" \(in days\) will be cleared. Five options are available, including 1 day, 7 days, 32 days, Permanent, and User-defined.
5.  Click **Next**.
6.  Fill in configuration items on the Create a Table \> Field and Partition Info. tab page.
    -   Add the field settings.
    -   Set the partitions.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16346/15476194008651_en-US.png)

        Parameters:

        -   Field English Name: English name of a field, which may contain letters, digits, and underscores.
        -   - Field type: MaxCompute data type \(string, bigint, double, datetime, or boolean\).
        -   Description: detailed description of a field.
        -   Operation: The options include Move Up, Move Down, and Delete.
        -   Whether to Set Partitions: If you select "Yes", you need to configure the partition key information. The string and bigint data types are supported.
7.  Click Submit.

    Upon successful commit of the new table, the system will automatically jump back to the data table management interface, click the tables that I manage to view the new table.


## Statement to create a table {#section_vcb_ss1_r2b .section}

1.  Enter the [DataWorks management console](https://workbench.data.aliyun.com/console) as a developer, and click Enter workspace after the corresponding project under the project list.
2.  On the top menu bar, choose **Data Management**. Navigate to Table Management on the left.
3.  Click **new table**, and then select**DDL build table**.
4.  Write DDL statements to create a table. Examples are as follows:

    ```
    
    create table if not exists table2
    (
     id string comment'user ID', 
     name string comment 'user name'
    ) partitioned by(dt string) 
    LIFECYCLE 7;
    ```

5.  Click Submit and the following page appears:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16346/15476194008653_en-US.png)

    Except Alias, Category, and Lifecycle, all the other configuration items on the Basic Information page are automatically filled in. You need to edit and provide the names and the security levels of fields on the Field and Partition Information page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16346/15476194008654_en-US.png)

6.  Fill in the remaining configuration items on the Basic Info. tab page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16346/15476194008655_en-US.png)

7.  Click **Next step**.
8.  Click **Submit**.

    After the created table is submitted, the system automatically jumps back to the Data Table Management page. Click **My Tables** to view the created table.


