# Add whitelist {#concept_jz3_bl5_q2b .concept}

This topic describes how to add a corresponding whitelist and security group when you use DataWorks in different regions.

To ensure the database security and stability, you can add IP addresses or IP segments for database access to the whitelist or [Add security group](reseller.en-US/User Guide/Data integration/Common configuration/Add security group.md#) of the target instance before using certain database instances. 

**Note:** You can only add whitelists for Data Integration tasks. Adding whitelists in other types of tasks are not supported.

## Add whitelist {#section_djc_kj5_q2b .section}

1.  Enter the [DataWorks management console](https://partners-intl.aliyun.com)as a developer and go to the **Project List** page.
2.  Select a project region.

    Currently, the supported regions are China East 2 \(Shanghai\), China South 1 \(Shenzhen\), Hong Kong, and Asia Pacific SOU 1 \(Singapore\). The default region is China East 2, and you can switch to other regions where your project is located, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16265/15514338898537_en-US.jpg)

3.  Select the whitelist for your project region.

    Some data sources have whitelist restrictions and require adding Data Integration IP addresses to whitelists. Common data sources, such as RDS, MongoDB, and Redis, require adding IP addresses to whitelists in their consoles. The following are two scenarios for adding a whitelist:

    -   When a sync task is running on the custom resource group, you must authorize machines for the custom resource group, and add the machine intranet IP addresses and Internet IP addresses to the data source whitelist.
    -   Each region has different whitelist entries, and select the region whitelist from the following table.

        |Region|Whitelist|
        |:-----|:--------|
        |China East 1 \(Hangzhou\)|100.64.0.0/8,11.193.102.0/24,11.193.215.0/24,11.194.110.0/24,11.194.73.0/24,118.31.157.0/24,47.97.53.0/24,11.196.23.0/24,47.99.12.0/24,47.99.13.0/24,114.55.197.0/24,11.197.246.0/24,11.197.247.0/24|
        |China East 2 \(Shanghai\)|11.193.109.0/24,11.193.252.0/24,47.101.107.0/24,47.100.129.0/24,106.15.14.0/24,10.117.28.203,10.117.39.238,10.143.32.0/24,10.152.69.0/24,10.153.136.0/24,10.27.63.15,10.27.63.38,10.27.63.41,10.27.63.60,10.46.64.81,10.46.67.156,11.192.97.0/24,11.192.98.0/24,11.193.102.0/24,11.218.89.0/24,11.218.96.0/24,11.219.217.0/24,11.219.218.0/24,11.219.219.0/24,11.219.233.0/24,11.219.234.0/24,118.178.142.154,118.178.56.228,118.178.59.233,118.178.84.74,120.27.160.26,120.27.160.81,121.43.110.160,121.43.112.137,100.64.0.0/8|
        |China South 1 \(Shenzhen\)|100.106.46.0/24,100.106.49.0/24,10.152.27.0/24,10.152.28.0/24,11.192.91.0/24,11.192.96.0/24,11.193.103.0/24,100.64.0.0/8,120.76.104.0/24,120.76.91.0/24,120.78.45.0/24|
        |Hong Kong|10.152.162.0/24,11.192.196.0/24,11.193.11.0/24,100.64.0.0/8,11.192.196.0/24,47.89.61.0/24,47.91.171.0/24,11.193.118.0/24,47.75.228.0/24|
        |Asia Pacific SE 1 \(Singapore\)|100.106.10.0/24,100.106.35.0/24,10.151.234.0/24,10.151.238.0/24,10.152.248.0/24,11.192.153.0/24,11.192.40.0/24,11.193.8.0/24,100.64.0.0/8,100.106.10.0/24,100.106.35.0/24,10.151.234.0/24,10.151.238.0/24,10.152.248.0/24,11.192.40.0/24,47.88.147.0/24,47.88.235.0/24,11.193.162.0/24,11.193.163.0/24,11.193.220.0/24,11.193.158.0/24,47.74.162.0/24,47.74.203.0/24,47.74.161.0/24,11.197.188.0/24|
        |Asia Pacific SE 2 \(Sydney\)|11.192.100.0/24,11.192.134.0/24,11.192.135.0/24,11.192.184.0/24,11.192.99.0/24,100.64.0.0/8,47.91.49.0/24,47.91.50.0/24,11.193.165.0/24,47.91.60.0/24|
        |China North 2 \(Beijing\)|100.106.48.0/24,10.152.167.0/24,10.152.168.0/24,11.193.50.0/24,11.193.75.0/24,11.193.82.0/24,11.193.99.0/24,100.64.0.0/8,47.93.110.0/24,47.94.185.0/24,47.95.63.0/24,11.197.231.0/24,11.195.172.0/24,47.94.49.0/24,182.92.144.0/24|
        |US West 1|10.152.160.0/24,100.64.0.0/8,47.89.224.0/24,11.193.216.0/24,47.88.108.0/24|
        |US East 1|11.193.203.0/24,11.194.68.0/24,11.194.69.0/24,100.64.0.0/8,47.252.55.0/24,47.252.88.0/24|
        |Asia Pacific SE 3 \(Malaysia\)|11.193.188.0/24,11.221.205.0/24,11.221.206.0/24,11.221.207.0/24,100.64.0.0/8,11.214.81.0/24,47.254.212.0/24,11.193.189.0/24|
        |EU Central 1 \(Germany\)|11.192.116.0/24,11.192.168.0/24,11.192.169.0/24,11.192.170.0/24,11.193.106.0/24,100.64.0.0/8,11.192.116.14,11.192.116.142,11.192.116.160,11.192.116.75,11.192.170.27,47.91.82.22,47.91.83.74,47.91.83.93,47.91.84.11,47.91.84.110,47.91.84.82,11.193.167.0/24,47.254.138.0/24|
        |Asia Pacific NE1 \(Japan\)|100.105.55.0/24,11.192.147.0/24,11.192.148.0/24,11.192.149.0/24,100.64.0.0/8,47.91.12.0/24,47.91.13.0/24,47.91.9.0/24,11.199.250.0/24,47.91.27.0/24|
        |Middle East 1 \(Dubai\)|11.192.107.0/24,11.192.127.0/24,11.192.88.0/24,11.193.246.0/24,47.91.116.0/24,100.64.0.0/8|
        |Asia Pacific SE 1 \(Mumbai\)|11.194.10.0/24,11.246.70.0/24,11.246.71.0/24,11.246.73.0/24,11.246.74.0/24,100.64.0.0/8,149.129.164.0/24,11.194.11.0/24|
        |UK|11.199.93.0/24,100.64.0.0/8|
        |Asia Pacific SE 5 \(Jakarta\)|11.194.49.0/24,11.200.93.0/24,11.200.95.0/24,11.200.97.0/24,100.64.0.0/8,149.129.228.0/24,10.143.32.0/24,11.194.50.0/24|


## Add an RDS whitelist {#section_nwx_mm5_q2b .section}

The RDS data source can be configured with the following two methods:

-   RDS instance

    In this case, a data source is created using an RDS instance. Currently, the connectivity test including the RDS in the VPC environments are supported. If the connectivity test fails, you can try adding the data source with JDBC URL.

-   JDBC URL

    For the IP address in the JDBC URL, enter either an intranet IP address or an Internet IP address \(if the intranet IP address is unavailable\). The intranet IP address features faster synchronization because the address is relevant to Alibaba Cloud data centers, while the Internet IP address synchronization speed depends on the bandwidth.


RDS whitelist configuration

When Data Integration is connected to the RDS for data synchronization, the database standard protocol must be connected to the database. By default, the RDS allows all IP address connections. If you specify an IP whitelist during RDS configuration, you must add an IP whitelist of the Data Integration execution nodes. If no whitelists are specified then none are provided for Data Integration.

If you have configured an IP whitelist for your RDS, go to the RDS [Management Console](https://account.alibabacloud.com/login/login.htm), and go to **Security Control** to configure the whitelist settings based on the preceding [Whitelist](https://www.alibabacloud.com/help/doc-detail/26198.htm) configurations.

**Note:** If you use a custom Resource Group to schedule the RDS data synchronization task, you must add the IP address of the computer host of the custom Resource Group to the RDS whitelist.

