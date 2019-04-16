# Add task resources {#concept_wfz_j45_q2b .concept}

Project administrators can create and modify scheduled resources on the **Data Integration** \> **Synchronous Resource Management** \> **Resource Group** page.

When the default scheduling resource is unable to connect your complex network environment with the deployment of the Data Integration agent, the data transfer synchronization between any network environment can be reached. For more information, see [Data integration when the network of data source \(one side only\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#) and [Data sync when the network of data source \(both sides\) is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).

**Note:** 

-   Scheduling resources added in Data Integration can only be used for data integration.
-   Admin permission is required for customizing some files running on a resource group. For example, calling shell files, SQL on custom ECS in a shell script task that you write documents, and others.

## Purchase the ECS cloud server {#section_rp5_qp5_q2b .section}

Purchase the ECS cloud server.

**Note:** 

-   centos6, centos7, or AliOS is recommended.
-   If the added ECS instance must run MaxCompute or synchronization tasks, verify whether the current ECS instance Python version is 2.6 or 2.7. \(The Python version of CentOS 5 is 2.4, while those of other operating systems are later than 2.6.\)
-   Ensure that the ECS instance has a public IP address.
-   The ECS configuration is recommended for 8-core processor with 16G RAM.

## View the ECS host name and the internal network IP address {#section_qst_vp5_q2b .section}

You can go to the**Cloud Server ECS** \> **Instance** page to view the ECS host name and purchased IP address .

## Provision 8000 port to read log { .section}

**Note:** If you are using a VPC network type, a provision 8000 port is not required.

1.  Add security group rules

    Go to the **Cloud Server ECS** \> **Network and Security** \> **Security Group** page, and click **Configuration Rules**, and then enter the configuration rules page.

2.  Go to the **Security Group Rules** \> **Intranet Entry Direction**page, and click **Add Security Group Rules** in the upper right corner.
3.  Complete the configuration information in the **Add Security Group Rule** dialog box, and configure the IP address to 10.116.134.123, and access port 8000.

## Add scheduling resources { .section}

1.  Enter the DataWorks management console as a developer, and click **Enter Workspace** in the corresponding project action bar.
2.  Click **Data Integration** in the top menu bar to navigate to **Resource Management** \> **New Resource Groups**.
3.  Click **Next** to **Add Purchased ECS** cloud server to the Resource Group in the Add Server dialog box.

    Configurations:

    -   Network type
        -   Classic network: The IP addresses are allocated in a unified manner by Alibaba Cloud that is easy to configure . This network type is suitable for users, who require high usability of operations and need to use ECS quickly.
        -   This type refers to logically isolated private networks. Users can customize network topology and IP addresses, and the network supports leased line connections. VPC is suitable for users familiar with network management.
    -   Server name
        -   Alibaba cloud Classic Network: Log in to ECS, execute the hostname command, and obtain the return value.
        -   Private Network: Log in to ECS, execute `dmidecode | grep UUID`, and obtain the return value.
    -   Maximum concurrency
        -   Count concurrency: The concurrency count calculator is based on the CPU number and memory size.
        -   Add server: The content is related to the network type selected above. If you select classic networks, you can only add classic networks. If you select a VPC network, the content of the VPC network type is displayed.
    **Note:** 

    -   When you want to make an ECS in a VPC as the server, you should enter the ECS UUID as the server name. Logging on to the ECS machine to perform `dmidecode | grep UUID` can be obtained.
    -   For example, to execute `dmidecode | grep UUID`, the return result is UUID: 713f4718-8446-4433-a8ec-6b5b62d75a24, the corresponding UUID is 713F4718-8446-4433-A8EC-6B5B62D75A24.
4.  Install Agent and initialize.

    If you are adding a newly added server, follow these steps.

    1.  Log into the ECS server as a root user.
    2.  Execute the following command:

        ```
        chown admin:admin /opt/taobao
        wget https://alisaproxy.shuju.aliyun.com/install.sh --no-check-certificate
        sh install.sh --user_name=xxxxxxxxxx19d --password=yyyyyygh1bm --enable_uuid=false
        ```

    3.  Later on the Add Server Page, click **Refresh** to see if the service status becomes **available**.
    4.  Provision port 8000 of the server.

        **Note:** If an error occurs during install.sh an Sh or a re-execution is required at install.sh, and the same directory of SH runs `rm –rf install.sh` to delete the files that have been generated. Then execute `install.sh`. The preceding initialization interface is different from each user command, please execute the relevant commands according to your own initialization interface.


After performing this operation, if the service status has been **stopped**, you may encounter the following problems:

The error shown in the preceding figure indicates that no host was bound. To fix the errors, follow these steps:

1.  Switch to the admin database.
2.  Execute `hostname -i` to see how the host is bound.
3.  Execute `vim/etc/hosts` and add the IP address and host name.
4.  Refresh the page service status if the CS Server registration is successful.

**Note:** 

-   If you are still stopping after the refresh, you can restart the alias command.

    Switch to the admin account and execute the following command:

    ```
    /home/admin/alisatasknode/target/alisatasknode/bin/serverct1 restart
    ```

-   If your AccessKey information is in the command, please do not reveal it to others.

