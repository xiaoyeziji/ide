# SHELL node {#concept_m5l_3z4_p2b .concept}

SHELL tasks support standard SHELL syntax but not interactive syntax. SHELL task can run on the default resource group. If you want to access an IP address or a domain name, add the IP address or domain name to the whitelist by choosing **Project Management** \> **Project Configuration**.

## Procedure {#section_r4m_kz4_p2b .section}

1.  Right-click **Business Flow** under **Data Development**, select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16292/15367327257651_en-US.png)

2.  Right-click **Data Development**, and select **Create Data Development Node** \> **SHELL**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16296/15367327257752_en-US.png)

3.  Set the node type to SHELL, enter the node name, select the target folder, and click **Submit**.
4.  Edit the node code.

    Go to the SHELL node code editing page and edit the code.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16296/15367327257753_en-US.png)

    If you want to call the System Scheduling Parameters in a SHELL statement, compile the SHELL statement as follows:

    ```
    echo "$1 $2 $3"
    ```

    **Note:** Parameter 1 Parameter 2... Multiple parameters are separated by spaces. For more information on the usage of system scheduling parameters, see [Parameter configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#).

5.  Node scheduling configuration.

    Click the **Scheduling Configuration** on the right of the node task editing area to go to the  node scheduling configuration page. For more information, see [Scheduling configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

6.  Submit the node.

    After the configuration is completed, click **Save** in the upper left corner of the page or press Ctrl+S to submit \(and unlock\) the node to the development environment.

7.  Release a node task.

    For more information about the operation, see [Release management](intl.en-US/User Guide/Data development/Publish management.md#).

8.  Test in the production environment.

    For more information about the operation, see [Cyclic tasks](intl.en-US/User Guide/Operation center/Task list/Cyclic task.md#).


## Use cases {#section_vlg_w1p_p2b .section}

Connect to a database using SHELL

-   If the database is built on Alibaba Cloud and the region is China \(Shanghai\), you must open the database to the following whitelisted IP addresses to connect to the database.

    10.152.69.0/24,10.153.136.0/24,10.143.32.0/24,120.27.160.26,10.46.67.156,120.27.160.81,10.46.64.81,121.43.110.160,10.117.39.238,121.43.112.137,10.117.28.203,118.178.84.74,10.27.63.41,118.178.56.228,10.27.63.60,118.178.59.233,10.27.63.38,118.178.142.154,10.27.63.15,100.64.0.0/8

    **Note:** If the database is built on Alibaba Cloud but the region is not China \(Shanghai\), we recommend that you use the Internet or buy an ECS instance in the same region of the database as the scheduling resource to run the SHELL task on a custom resource group.

-   If the database is built locally, we recommend that you use the Internet connection and open the database to the preceding whitelisted IP addresses.

    **Note:** If you are using a custom resource group to run the SHELL task, you must add the IP addresses of machines in the custom resource group to the preceding whitelist.


