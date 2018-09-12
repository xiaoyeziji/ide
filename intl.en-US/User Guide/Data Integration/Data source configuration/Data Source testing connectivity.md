# Data Source testing connectivity {#concept_ovl_zgv_42b .concept}

|Data Source|Data Source Type|Network Type|Do you support test connectivity?|Add custom resource group|
|:----------|:---------------|:-----------|:--------------------------------|:------------------------|
|MySQL|ApsaraDB|Classic network|Yes|-|
|VPC network|Yes|-|
|With public IP address|Yes|-|
|Without public IP address|No|Yes|
|Self-built ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|SQL Server|ApsaraDB|Classic network|Yes|-|
|VPC network|Yes|-|
|With public IP address|Yes|-|
|Without public IP address|No|Yes|
|Self-built ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|PostgreSQL|ApsaraDB|Classic network|Yes|-|
|VPC network|Yes|-|
|With public IP address|Yes|-|
|Without public IP address|No|Yes|
|Self-built ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|Oracle|With public IP address|Yes|-|
|Without public IP address-|No|Yes|
|Self-built ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|DRDS|ApsaraDB|Classic network|Yes|-|
|VPC network|In scheduling|Yes|
|HybridDB for MySQL|ApsaraDB|Classic network|Yes|-|
|VPC network|In scheduling|Yes|
|HybridDB for PostgreSQL released|ApsaraDB|Classic network|Yes|-|
|VPC network|In scheduling|Yes|
|MaxCompute \(for ODPS data sources\)|ApsaraDB|Classic network|Yes|-|
|AnalyticDB \(for ADS data sources\)|ApsaraDB|Classic network|Yes|-|
|VPC network|In scheduling|Yes|
|OSS|ApsaraDB|Classic network|Yes|-|
|VPC network|Yes|-|
|HDFS|With public IP address|Yes|-|
|Self-built ECS|Classic network|Yes|-|
|VPC network|No|-|
|FTP|With public IP address|Yes|-|
|Without public IP address|No|-|
|Self-built ECS|Classic network|Yes|-|
|VPC network|No|-|
|MongoDB|ApsaraDB|Classic network|Yes|-|
|VPC network|In scheduling|Yes|
|With public IP address|Yes|-|
|Self-built ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|Memcache|ApsaraDB|Classic network|Yes|-|
|VPC network|In scheduling|Yes|
|Redis|ApsaraDB|Classic network|Yes|-|
|VPC network|In scheduling|Yes|
|With public IP address|Yes|-|
|Self-built ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|Table Store \(for OTS data sources\)|ApsaraDB|Classic network|Yes|-|
|VPC network|In scheduling|Yes|
|DataHub|ApsaraDB|Classic network|Yes|-|
|VPC network|No|-|

**Note:** Whether to add a Custom Resource Group, refer to [Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).

## Description {#section_uy4_1hv_42b .section}

In the preceding table, "-" means that this item is unavailable, "No" means that the connectivity test fails and a custom resource group must be added but the synchronization tasks can be configured.

-   Data sources in VPC environment:
    -   Connectivity test for RDS data sources in VPC environment is supported.
    -   Other data sources in VPC environment are under planning.
    -   Connectivity tests are not supported for Financial Cloud networks.
-   User-created ECS data sources:
    -   The classic network supports JDBC-based connectivity tests normally on the public network.
    -   The VPC does not support connectivity tests for now.
    -   Connectivity tests for cross-region sources are not supported for now.
    -   Connectivity tests are not supported for Financial Cloud networks.

Currently, data synchronization is implemented solely by adding a custom resource group. For details, see [Data Synchronization Configuration for the VPC \(Financial Cloud\)](https://www.alibabacloud.com/help/zh/doc-detail/55474.html).

For user-created ECS data sources, ensure to add the IP address of the scheduling cluster to the security group for both inbound and outbound traffic \(which is true for both the public network and the classic network\). If the security group is not added, disconnection can occur during synchronization. For more information, see [Add security group](intl.en-US/User Guide/Data Integration/Common configuration/Add security group.md#).

You cannot add an extensive range of ports on the ECS security group page. To add them, use the security group API of ECS. For details, see [AuthorizeSecurityGroup](https://www.alibabacloud.com/help/doc-detail/25554.htm).

-   Data sources created in local IDCs or on the ECS server without public IP addresses:
    -   Connectivity tests are not supported.
    -   A custom resource group must be added for configuring synchronization tasks.
-   Data sources created in local IDCs or on the ECS server with public IP addresses:

    For such data sources, public-network-based JDBC is applied to connectivity tests. If the connectivity test fails, check the constraints of the local network or relevant databases.


**Note:** 

For connectivity tests of data sources, you may concern the charge for public-network-based synchronization the most. The following example describes the charge for synchronizing data from RDS to MaxCompute:

Currently, Data Integration is free of charge but still may involve certain charged products. For configuring MaxCompute data synchronization in DataWorks, it is free of charge. Charge is applied only when you want to manually add a parameter in the script mode to set a public IP address for the MaxCompute tunnel \(however, this parameter is unavailable in the template generated in the script mode.\)

## Conclusion { .section}

When test connectivity fails, you need to verify that the data source area, the network type, the RDS whitelist Add the full instance id, the database name, and the user name is correct. Examples of common errors are as follows:

-   The Database Password is incorrect, as shown below.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16196/15367204097520_en-US.png)

-   The network doesn't have a diagram, as shown below.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16196/15367204097521_en-US.png)

-   During synchronization, there is a network disconnect and so on.

    First look at the full log to determine which scheduled resource it is and whether it is a custom resource.

    If so, check whether the IP address of the custom resource group has been added to the whitelist of the data source such as the RDS \(this also applies to the MongoDB\).

    Check whether the connectivity test between both data sources is succeeded and their whitelists are complete \(if the whitelists are incomplete, the test result varies randomly; specifically, the test is succeeded if the task is assigned to the added scheduling server and is failed if no scheduling server has been added.\)

-   For the condition where the task is displayed as succeeded but the disconnection error 8000 can be found in the log:

    This condition occurs when the custom scheduling resource group is used and the IP address 10.116.134.123 and port 8000 are not set as permitted in the security group for inbound traffic. In this condition, add the IP address and the port, and run the task again.


## Connectivity test failure examples {#section_unr_1kv_42b .section}

Example 1

-   Problem

    Test connection failed. Connectivity test of data source failed. An error occurred while connecting to the database. The database connection string is "jdbc:mysql://xx.xx.xx.x:xxxx/t\_uoer\_bradef"; the user name is "xxxx\_test"; and the exception message is "Access denied for user "xxxx\_test"@"%" to database "yyyy\_demo"".

-   Troubleshooting
    1.  Check whether the entered information is correct.
    2.  Check whether the password, whitelist, or your account has the permission to access the database. You can add the required permissions in the RDS console.
-   Example 2
    -   Problem

        Test connection failed. Connectivity test of data source failed. The error message is as follows:

        ```
        error message: Timed out after 5000 ms while waiting for a server that matches ReadPreferenceServerSelector{readPreference=primary}. Client view of cluster state is {type=UNKNOWN, servers=[(xxxxxxxxxx), type=UNKNOWN, state=CONNECTING, exception={com.mongodb.MongoSocketReadException: Prematurely reached end of stream}}]
        ```

    -   Troubleshooting

        For non-VPC MongoDB, you must add a whitelist for the connectivity test of the MongoDB data source. For details, see Add a Whitelist[Add a whitelist](intl.en-US/User Guide/Data Integration/Common configuration/Add whitelist.md#).


