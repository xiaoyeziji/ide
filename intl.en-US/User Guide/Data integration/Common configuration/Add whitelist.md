# Add whitelist {#concept_jz3_bl5_q2b .concept}

This article describes how to add a corresponding whitelist and security group when you are using DataWorks in different regions.

To make sure the security and stability of databases, you can add the IP addresses or IP segments used for accessing the database to the whitelist or [Add security group](intl.en-US/User Guide/Data integration/Common configuration/Add security group.md#) of the target instance before using certain database instances. 

## Add a whitelist {#section_djc_kj5_q2b .section}

1.  Enter the [DataWorks management console](https://workbench.data.aliyun.com/console)as a developer and navigate to the **project list** page.
2.  Select a project region.

    Currently, the supported regions are China East 2 \(Shanghai\), China South 1 \(Shenzhen\), Hong Kong, and Asia Pacific SOU 1 \(Singapore\). The default region is China East 2, and you can switch to other regions where your project is located, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16265/15417295558537_en-US.jpg)

3.  Select the whitelist for your project region.

    Some data sources have a whitelist restrictions currently and need to add IPs of data integration to whitelists. Common data sources, such as RDS, MongoDB, and Redis, need to add IPs to whitelists in their consoles.  Adding a whitelist has the following two case:

    -   When a sync task is running on the custom resource group. You must authorize machines for the custom resource group, and add machines' intranet IPs and extranet IPs to the whitelist of data source.
    -   The whitelist entries differs from region to region. Select the whitelist for the selected region from the following table.

        |region|Whitelist|
        |:-----|:--------|
        |China \(Hangzhou\)|100.64.0.0/8,11.193.102.0/24,11.193.215.0/24,11.194.110.0/24,11.194.73.0/24,118.31.157.0/24,47.97.53.0/24|
        |China \(Shanghai\)|11.193.109.0/24,11.193.252.0/24,47.101.107.0/24,47.100.129.0/24,106.15.14.0/24,10.117.28.203,10.117.39.238,10.143.32.0/24,10.152.69.0/24,10.153.136.0/24,10.27.63.15,10.27.63.38,10.27.63.41,10.27.63.60,10.46.64.81,10.46.67.156,11.192.97.0/24,11.192.98.0/24,11.193.102.0/24,11.218.89.0/24,11.218.96.0/24,11.219.217.0/24,11.219.218.0/24,11.219.219.0/24,11.219.233.0/24,11.219.234.0/24,118.178.142.154,118.178.56.228,118.178.59.233,118.178.84.74,120.27.160.26,120.27.160.81,121.43.110.160,121.43.112.137,100.64.0.0/8|
        |China \(Shenzhen\)|100.106.46.0/24,100.106.49.0/24,10.152.27.0/24,10.152.28.0/24,11.192.91.0/24,11.192.96.0/24,11.193.103.0/24,100.64.0.0/8,120.76.104.0/24,120.76.91.0/24,120.78.45.0/24|
        |Hong Kong|10.152.162.0/24,11.192.196.0/24,11.193.11.0/24,100.64.0.0/8,11.192.196.0/24,47.89.61.0/24,47.91.171.0/24|
        |Singapore|100.106.10.0/24,100.106.35.0/24,10.151.234.0/24,10.151.238.0/24,10.152.248.0/24,11.192.153.0/24,11.192.40.0/24,11.193.8.0/24,100.64.0.0/8,100.106.10.0/24,100.106.35.0/24,10.151.234.0/24,10.151.238.0/24,10.152.248.0/24,11.192.40.0/24,47.88.147.0/24,47.88.235.0/24,11.193.162.0/24,11.193.163.0/24,11.193.220.0/24,11.193.158.0/24,47.74.162.0/24,47.74.203.0/24,47.74.161.0/24|
        |China \(Beijing\)|100.106.48.0/24,10.152.167.0/24,10.152.168.0/24,11.193.50.0/24,11.193.75.0/24,11.193.82.0/24,11.193.99.0/24,100.64.0.0/8,47.93.110.0/24,47.94.185.0/24,47.95.63.0/24,11.197.231.0/24,11.195.172.0/24,47.94.49.0/24,182.92.144.0/24|
        |US West|10.152.160.0/24,100.64.0.0/8,47.89.224.0/24|
        |Asia Pacific SE 3 \(Malaysia\)|11.193.188.0/24,11.221.205.0/24,11.221.206.0/24,11.221.207.0/24,100.64.0.0/8,11.214.81.0/24,47.254.212.0/24|
        |Germany|11.192.116.0/24,11.192.168.0/24,11.192.169.0/24,11.192.170.0/24,11.193.106.0/24,100.64.0.0/8,11.192.116.14,11.192.116.142,11.192.116.160,11.192.116.75,11.192.170.27,47.91.82.22,47.91.83.74,47.91.83.93,47.91.84.11,47.91.84.110,47.91.84.82|
        |Japan|100.105.55.0/24,11.192.147.0/24,11.192.148.0/24,11.192.149.0/24,100.64.0.0/8,47.91.12.0/24,47.91.13.0/24,47.91.9.0/24|
        |United Arab Emirates \(Dubai\)|11.192.107.0/24,11.192.127.0/24,11.192.88.0/24,11.193.246.0/24,47.91.116.0/24,100.64.0.0/8|
        |India \(Mumbai\)|11.194.10.0/24,11.246.70.0/24,11.246.71.0/24,11.246.73.0/24,11.246.74.0/24,100.64.0.0/8,149.129.164.0/24|
        |UK|11.199.93.0/24,100.64.0.0/8|


## Add an RDS whitelist {#section_nwx_mm5_q2b .section}

The RDS data source can be configured in the following two ways.

-   RDS instance

    In this case, a data source is created by using an RDS instance. Currently, the connectivity test \(including the RDS in VPC environments\) is supported. If the connectivity test fails, you can try to add the data source using the JDBCURL.

-   JDBCURL

    For the IP in JDBCURL, enter an intranet IP address or an Internet IP address if no intranet IP address is available. The intranet IP address features faster synchronization because the address is relevant to Alibaba Cloud data centers, while the synchronization speed of the internet IP address is subject to the available internet bandwidth.


RDS whitelist configuration

When Data Integration is connected to RDS for data synchronization, the database standard protocol must be connected to the database. The RDS permits all IP connections by default. If you specify an IP whitelist during RDS configuration, you must add an IP whitelist of Data Integration execution nodes. If no RDS whitelist is specified, no whitelist is provided for Data Integration.

If you have set up an IP white list for your RDS, go to the RDS [Management Console](https://account.alibabacloud.com/login/login.htm), and navigate to **security control** to make the whitelike settings based on the [whitelike list](https://www.alibabacloud.com/help/doc-detail/26198.htm) above.

**Note:** If you use a custom resource group to schedule the RDS data synchronization task, you must add the IP address of the computer hosting the custom resource group to the RDS whitelist.

