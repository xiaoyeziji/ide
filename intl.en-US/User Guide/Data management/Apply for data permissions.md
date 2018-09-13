# Apply for data permissions {#concept_w5f_1rt_q2b .concept}

Alibaba Cloud DTplus DataWorks provides the following three data types:

-   Table: Namely the data tables.
-   Function: Namely the UDF, functions that can be used in SQL.
-   Resource: For example, the text files and MapReduce JAR files.

These three data types have a strict permission control feature. You can use them after applying for the required permissions.

## Apply for table permissions {#section_ny4_dx5_q2b .section}

1.  Find the data table that needs to apply for permission by **Data Management** \> **All Data** page.
2.  Click **Application permissions** in the Actions column of the data table.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16345/15368064888552_en-US.png)

3.  Complete the configurations in the **Apply for authorization** dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16345/15368064888553_en-US.png)

    Parameters:

    -   Permission owner: Select Self Apply or Apply as agent.
        -   Self Apply: With this option selected, the permission is granted to the you, because you being the current logon user, after the application is approved.
        -   Apply as agent: With this option selected, enter the account \(the logon name in the upper-right corner of the system\) to whom you want to apply the permission for.  Once the application is approved, the permission is granted to the specified account.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16345/15368064888554_en-US.png)

    -   Permission expiration date: The duration of the applied table permission. The unit is in days. If not specified, the permission does not expire permanently by default. When the validity period expires, the permission is automatically revoked by the system.
    -   Application reason: Enter a brief application reason for faster approval.
4.  Click **OK** to submit the application and wait for approval.  You can check the application status in **Permission Management \> Application** History.

## Apply for function and resource permissions {#section_cjr_fz5_q2b .section}

1.  Enter the **Data Management** \> **Query Data** page.
2.  Click Apply for data permission in the upper-right corner of the list.

    ![](images/8556_en-US.png)

3.  Complete the configurations in the Apply for authorization dialog box.

    ![](images/8557_en-US.png)

    Parameters:

    -   Application type: Select Function or Resource.

    -   Permission owner: Select Self Apply or Apply as agent.

        -   Self Apply: With this option selected, the permission is granted to the you because you being the current logon user, after the application is approved.

        -   Apply as agent: With this option selected, enter the account \(the logon name in the upper-right corner of the system\) to whom you want to apply the permission for.  Once the application is approved, the permission is granted to the specified account.

    -   Project name: Select the project name \(MaxCompute project name\) where the function or resource that you want to apply for permissions resides. Fuzzy searches within the organization is supported.

    -   Function name/Resource name: Enter the name of the function or the resource in the project.  Enter the full name of the resource, including the file suffix, such as my\_mr.jar.

    -   Permission expiration date: The duration of the applied permission. The unit is in days. If not specified, the permission does not expire permanently by default.  When the expiration date arrives, the permission is automatically revoked by the system.

    -   Application reason: Enter a brief application reason for faster approval.

4.  Click **OK** to submit the application and wait for approval.  You can check the application status in Permission Management \> Application History.

