# Add scheduling resources {#concept_wfz_j45_q2b .concept}

Project administrators can create new and modify scheduled resources on the **data integration** \> **synchronous Resource Management** \> **Resource Group** page.

When the default scheduling resource is unable to connect to your complex network environment, with the deployment of the data integration agent, the synchronization of data transfer between any network environment can be reached, see[Data integration when the network of data source \(one side only\) is disconnected](intl.en-US/User Guide/Data Integration/Best practice/Data integration when the network of data source (one side only) is disconnected.md#)and[Data sync when the network of data source \(both sides\) is disconnected](intl.en-US/User Guide/Data Integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#) for details.

**Note:** 

-   Currently only East China 1, East China 2, South China 1 and North China 2 support adding custom resources \(deployment agent \), the ability of other regions to add custom resource groups is still in the pipeline.
-   A machine can add only one custom Resource Group, you can select only one network type per Custom Resource Group, you can set up scheduling resource sharing in the add-on console. Custom scheduling resource groups currently only support odps\_sql/odps\_MR/Shell/ synchronization tasks, other tasks are not supported.
-   Admin permission is required to customize some files running on a resource group, for example, calling shell files, SQL on custom ECs in a shell script task that you write yourself documents, etc.

## An ECS instance must be used to import commands. {#section_rp5_qp5_q2b .section}

Purchase the ECS cloud server.

**Note:** 

-   centos6、centos7 or aliyunos is recommended.
-   If the added ECS instance needs to run MaxCompute or synchronization tasks, verify whether the current Python version of the ECS instance is 2.6 or 2.7 \(The Python version of CentOS 5 is 2.4 while those of other operating systems are later than 2.6\).
-   Ensure that the ECS instance has a public IP address.
-   The configuration of the ECS is recommended for the 8-core 16g.

## View the ECS host name and the internal network IP address {#section_qst_vp5_q2b .section}

You can go to the**cloud server ECS** \> **instance** page to view the ECS host name and IP purchased.

![](images/8542_en-US.png)

## Provision 8000 port to read log { .section}

**Note:** If it is a VPC network type, there is no need to provision a 8000 port.

1.  Add security group rules

    Navigate to the **cloud server ECS** \> **network and security** \> **Security Group** page, click **configuration rules**, and enter the configuration rules page.

    ![](images/8543_en-US.png)

2.  Go to the **security group rules** \> **Intranet entry direction**page, and click in the upper right corner to **add security group rules**.

    ![](images/8544_en-US.png)

3.  Complete the configuration information in the **add security group Rule** dialog box, configure IP as 10.116.134.123, and access port 8000.

    ![](images/8545_en-US.png)


## Add scheduling resources { .section}

1.  Enter the dataworks management console as a developer, and click **Enter workspace** in the corresponding project action bar.
2.  Click **data integration** in the top menu bar to navigate to **resource management** \> **new resource groups**.

    ![](images/8546_en-US.png)

3.  Click **Next** to **add the purchased ECS** cloud server to the Resource Group in the Add Server dialog box.

    ![](images/8547_en-US.png)

    Configurations:

    -   Network Type
        -   Classic network: IP addresses are allocated in a unified manner by Alibaba Cloud, featuring easy configuration and convenient use. This network type is suitable for users who require high ease-of-use of operations and need to use ECS quickly.
        -   This type refers to logically isolated private networks. Users can customize network topology and IP addresses, and the network supports leased line connections. VPC is suitable for users familiar with network management.
    -   Server name
        -   Alibaba cloud Classic Network: log in to ECs, execute the hostname command, and get the return value.
        -   Private Network: log in to ECS, execute `dmidecode | grep UUID`, and get the return value.
    -   Maximum concurrency
        -   Count concurrency: The concurrency count calculator is based on the CPU number and memory size.
        -   Add Server: The content is related to the network type selected above. If you select classic networks, you can only add classic networks. If you select a proprietary network, the content of the proprietary network type is displayed.
    **Note:** 

    -   When you want to make an ECS in a VPC as the server, you should fill in the ECS UUID as the server name. Logging in to the ECS machine to perform `dmidecode | grep UUID` can be obtained.
    -   For example, to execute `dmidecode | grep UUID`, the return result is UUID: 713f4718-8446-4433-a8ec-6b5b62d75a24, the corresponding UUID is 713F4718-8446-4433-A8EC-6B5B62D75A24.
4.  Install Agent and initialize.

    ![](images/8551_en-US.png)

    If you are adding a newly added server, follow these steps.

    1.  Log into the ECS server as a root user.
    2.  Execute the following command:

        ```
        wget https://alisaproxy.shuju.aliyun.com/install.sh --no-check-certificate
        
        sh install.sh --user_name=xxxxxxxxxx19d --password=yyyyyygh1bm --enable_uuid=false
        ```

    3.  Later on the Add Server Page, click **Refresh** to see if the service status becomes **available**.
    4.  Provision port 8000 of the server.

        **Note:** If you do install.sh an error occurred during Sh or a re-execution is required at install.sh the same directory of SH runs `rm –rf install.sh` to delete the files that have been generated. Then execute `install.sh`. The initialization interface above is different for each user's command, please execute the relevant commands according to your own initialization interface.

        ![](images/8555_en-US.jpg)


After doing so, if the service status has been **stopped**, you may encounter the following problems.

![](images/8558_en-US.png)

The error shown in the preceding figure indicates that no host was bound. To fix the error, follow these steps:

1.  Switch to the admin database.
2.  Execute `hostname -i` to see how the host is bound.
3.  Execute `vim/etc/hosts` and add the IP address and host name.
4.  Refresh the page service status if the CS Server registration is successful.

**Note:** 

-   If you are still stopping after the refresh, You can restart the alisa command.

    Switch to the admin account and execute the following command.

    ```
    /home/admin/alisatasknode/target/alisatasknode/bin/serverct1 restart
    ```

-   If your AK information is involved in the command, please do not expose it to others easily.

