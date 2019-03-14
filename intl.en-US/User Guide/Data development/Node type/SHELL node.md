# SHELL node {#concept_m5l_3z4_p2b .concept}

This topic describes the SHELL node. The SHELL node supports standard SHELL syntax but not the interactive syntax. The SHELL task can run on the default resource group. If you want to access an IP address or a domain name, add the IP address or domain name to the whitelist by choosing Project Configuration.

## Procedure {#section_r4m_kz4_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15525323047651_en-US.png)

2.  Right-click **Data Development**, and select **Create Data Development Node** \> **SHELL**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16296/15525323047752_en-US.png)

3.  Set the node type to SHELL, and enter the node name. Select the target folder, and then click **Submit**.
4.  Edit the node code.

    Go to the SHELL node code editing page and edit the code.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16296/15525323047753_en-US.png)

    If you want to call the System Scheduling Parameters in a SHELL statement, then compile the SHELL statement as follows:

    ```
    echo "$1 $2 $3"
    ```

    **Note:** Separate multiple parameters by spaces, for example: Parameter 1 Parameter 2... For more information about the usage of system scheduling parameters, see [Parameter configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Parameter configuration.md#).

5.  Schedule node configuration.

    Click the **Scheduling Configuration** on the right of the node task editing area to go to the  Node Scheduling Configuration page. For more information about Node Scheduling Configuration, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling configuration/Basic attributes.md#).

6.  Submit the node.

    After completing the configuration, click **Save** in the upper-left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

7.  Release a node task.

    For more information about the operation, see Release management.

8.  Test the production environment.

    For more information about the production environment, see [Cyclic task](reseller.en-US/User Guide/O&M Center/Task list/Cyclic task.md#).


## Use cases {#section_vlg_w1p_p2b .section}

Connect to a database with SHELL

-   If the database is built on Alibaba Cloud and the region is China \(Shanghai\), you must open the database with the following whitelisted IP addresses to connect to the database.

    10.152.69.0/24,10.153.136.0/24,10.143.32.0/24,120.27.160.26,10.46.67.156,120.27.160.81,10.46.64.81,121.43.110.160,10.117.39.238,121.43.112.137,10.117.28.203,118.178.84.74,10.27.63.41,118.178.56.228,10.27.63.60,118.178.59.233,10.27.63.38,118.178.142.154,10.27.63.15,100.64.0.0/8

    **Note:** If the database is built on Alibaba Cloud, but the region is not China \(Shanghai\). We recommend that you use the Internet or buy an ECS instance in the same database region, as the scheduling resource to run the SHELL task on a custom resource group.

-   If the database is built locally, we recommend that you use the Internet connection and open the database in the preceding whitelisted IP addresses.

    **Note:** If you are using a Custom Resource Group to run the SHELL task, you must add the IP addresses of machines in the Custom Resource Group to the preceding whitelist.


