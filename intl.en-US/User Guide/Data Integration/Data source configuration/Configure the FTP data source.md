# Configure the FTP data source {#concept_n1l_d21_p2b .concept}

The FTP data source allows you to read data from and write data to FTP, and supports configuring synchronization tasks in wizard mode and script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15367206957534_en-US.png)

4.  In the new data source pop-up box, select the data source type as **FTP**.
5.  Configure the information items of the FTP data source.

    You can create either of the following two FTP data sources as required:

    -   With public IP address

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15367206957535_en-US.png)

        Configurations:

        -   Type: With a public IP address.
        -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
        -   Description: It is a brief description of the data source with no more than 80 characters.
        -   Protocol: Currently only FTP and SFTP are supported.
        -   Host: The FTP host IP address.
        -   Port: If you select the FTP protocol, the port defaults to 21. If SFTP is selected, the port 22 is used by default.
        -   Username/Password: The account and password for accessing the FTP service.
    -   Without public IP address

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15367206957536_en-US.png)

        Configurations:

        -   Data source type: data source without a public IP address. The data source of this type must use custom scheduling resources so that it can synchronize data. For details, click **Help Manual**.
        -   Data source name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
        -   Data source description: It is a brief description of the data source with no more than 80 characters.
        -   - Resource Group: It is used to run synchronization tasks, and generally multiple machines can be bound when you add a resource group. For details, see[Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#)
        -   Protocol: Currently only FTP and SFTP are supported.
        -   Host: The FTP host IP address.
        -   Port: If you select the FTP protocol, the port defaults to 21. If SFTP is selected, the port 22 is used by default.
        -   User Name/Password: The account and password for accessing the FTP service.
6.  Click **Test Connectivity**
7.  When the connectivity test is passed, click **Complete**.

    Provides the ability to test connectivity to determine if the information entered is correct.


## Connectivity test description {#section_rrz_yc1_p2b .section}

-   The connectivity test is available in the classic network to identify whether the input host, port, user name, and password information is correct.
-   The data source connectivity test is currently not supported by the proprietary network, and you can click **confirm**.

## Next step {#section_dqv_5d1_p2b .section}

Now you have learned how to configure the FTP data source. The document explains how to configure the FTP Writer plug‑in later. For more information, see [Configure the FTP writer](intl.en-US/User Guide/Data Integration/Task Configuration/Configure Writer plug-in/Configure FTP Writer.md#).

