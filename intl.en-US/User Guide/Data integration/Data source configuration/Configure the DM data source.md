# Configure the DM data source {#concept_kyt_jsv_42b .concept}

The DM relational database data source provides the ability to read data from and write data to DM databases, and supports configuring synchronization tasks in wizard and script modes.

## Procedure {#section_jy4_q4v_42b .section}

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as an administrator and click **Enter Workspace** from the Actions column of the relevant project in the Project List.
2.  Select **Data Integration** in the top navigation bar. Click **Data Source** from the left-side navigation pane.
3.  Click **New source** to pop up the supported data source.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16199/15396612157528_en-US.png)

4.  In the new data source pop-up box, select a data source type of **dream**.
5.  Configure the information items of the DM data source.

    Select either of the following data source types as needed when creating a DM data source:

    -   With public IP address

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16199/15396612167529_en-US.png)

        Parameters:

        -   Type: With a public IP address.
        -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
        -   Description: It is a brief description of the data source with no more than 80 characters.
        -   JDBC URL: In the format of jdbc:mysql://ServerIP:Port/Database.
        -   Username/Password: The user name and password used to connect to the database.
    -   Without public IP address

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16199/15396612167530_en-US.png)

        Parameters:

        -   Type: No public network IP, selecting a data source of this type requires the use of custom scheduling resources for synchronization, you can click **Help manual** for details.
        -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
        -   Description: It is a brief description of the data source with no more than 80 characters.
        -   Resource Group: It is used to run synchronization tasks, and generally multiple machines can be bound when you add a resource group. For more information, see[Add scheduling resources](reseller.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).
        -   JDBC URL: In the format of jdbc:mysql://ServerIP:Port/Database.
        -   Username/Password: The user name and password used to connect to the database.
6.  \(Optional\).Click **Test Connectivity**to test the connectivity after entering all the required information in the relevant fields.
7.  When the connectivity test is passed, click **Complete**.

    Provides the ability to test connectivity to determine if the information entered is correct.


## Connectivity test description { .section}

-   The connectivity test is available in the classic network arrangement, to identify whether the input JDBC URL, user name, and password are correct.
-   Currently, connectivity test is not supported for the VPC and without-public-IP-address data source types. Thus, click **Confirm** directly.

