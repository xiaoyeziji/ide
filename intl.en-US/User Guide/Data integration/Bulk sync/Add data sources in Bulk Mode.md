# Add data sources in Bulk Mode {#concept_vfk_rk1_pfb .concept}

This article will show you how to add data sources in Bulk Mode.

**Note:** 

-   Fast cloud currently only supports three types of data sources: MySQL, Oracle, and SQL Server.
-   Add data sources in Bulk Mode is currently only available for Data Source Type **Has Public Network IP**.
-   After adding MySQL and Oracle, SQL Server data sources ,**Batch testing connectivity** are required. Only when **Connected State** is **Success**, the specific bulk data source would be an available data source option for **Bulk Sync**.

1.  Log in to the [DataWorks Console](https://partners-intl.aliyun.com)as Project Administrator.
2.  Click**The Data Integration** in a specific Workspace.
3.  In**Data Integration** \> **Sync Resource** \> **Data Source** Page, click **Add Data Source**.
4.  In Add Data Source window, you can select **MySQL**,**Oracle** , or **SQL Server**.

    |Configuration|Instructions|
    |:------------|:-----------|
    |**Data Source Type**|Select **Has Public Network IP**.|
    |**Configuration**|Select **Bulk Mode**.|
    |**The script upload**|Click **Template** to download the template file, input your data source name, data source description, link address, user name, and password into the downloaded template file.**Note:** In general, there is an existing data source, you can just delete and add your own data source information.

|
    |**Select a file**|Click **Select a file** to choose an existing template in local.|
    |**Start new**|After file uploaded successfully, click **Start new**, the information of data source uploading will display in the text box, such as the number of successes, the number of failures, cause of the failures, etc.|

5.  Click **Finish** when uploading process completes.
6.  In Data Source Page, select specific data source, click **Bulk testing connectivity**.

    **Note:** Only if the Connected Status of the data source is **Success**, you can operate the Bulk Sync.

7.  Select the data sources that you want to upload then click **Bulk Sync**.

