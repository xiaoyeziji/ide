# Configure FTP data source {#concept_n1l_d21_p2b .concept}

This topic describes how to configure the FTP data source. The FTP data source allows you to read/write data to FTP, and supports configuring synchronization tasks in wizard and script mode.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** in the Actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** in the supported data source pop-up window.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15514307847534_en-US.png)

4.  Select the data source type **FTP** in the new data source pop-up window.
5.  Complete the FTP data source information items configuration.

    You can create either one of the following two FTP data sources:

    -   FTP data sources with public IP address

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15514307857535_en-US.png)

        Configurations:

        -   Type: A FTP data source with a public IP address.
        -   Name: The name must start with a letter or underscores \(\_\), and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
        -   Description: A brief description of the data source that cannot exceed 80 characters in length.
        -   Protocol: Currently, only supports FTP and SFTP.
        -   Host: The FTP host IP address.
        -   Port: If you select the FTP protocol, the default port is 21. If SFTP is selected, port 22 is used by default.
        -   Username and password: The account and password for accessing the FTP service.
    -   FTP data sources without public IP address

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16201/15514307857536_en-US.png)

        Configurations:

        -   Data source type: The FTP data sources without a public IP address. This data source type must use custom scheduling resources so that it can synchronize data. For details, click **Help Manual**.
        -   Data source name: The name must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length. It can contain letters, numbers, and underscores \(\_\).
        -   Data source description: A brief description of the data source that does not exceed 80 characters in length.
        -   - Resource Group: The resource group is used to run synchronization tasks. You can bound multiple machines when you add a resource group. For details, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).
        -   Protocol: Currently, only FTP and SFTP are supported.
        -   Host: The FTP host IP address.
        -   Port: If you select the FTP protocol, the port defaults to 21. If SFTP is selected, the port 22 is used by default.
        -   Username and password: The account and password for accessing the FTP service.
6.  Click **Test Connectivity**
7.  When the test connectivity is finished, click **Complete**.

    The test connectivity capability provided determines if the information entered is correct.


## Connectivity test description {#section_rrz_yc1_p2b .section}

-   The connectivity test is available in the classic network to identify whether the entered host, port, user name, and password information is correct.
-   The data source connectivity test is currently not supported by the VPC network, and you can click **Confirm**.

## Next step {#section_dqv_5d1_p2b .section}

For more information on how to configure the FTP Writer plug‑in, see [Configure FTP Writer](reseller.en-US/User Guide/Data integration/Task configuration/Configure writer plug-in/Configure FTP Writer.md#).

