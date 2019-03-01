# Test data source connectivity {#concept_ovl_zgv_42b .concept}

|Data source|Data source type|Network type|Supports test connectivity|Add custom resource group|
|:----------|:---------------|:-----------|:-------------------------|:------------------------|
|MySQL|ApsaraDB|Classic network|Yes|-|
|VPC network|Yes|-|
|With public IP address|Yes|-|
|Without public IP address|No|Yes|
|On-premise ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|SQL Server|ApsaraDB|Classic network|Yes|-|
|VPC network|Yes|-|
|With public IP address|Yes|-|
|Without public IP address|No|Yes|
|On-premise ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|PostgreSQL|ApsaraDB|Classic network|Yes|-|
|VPC network|Yes|-|
|With public IP address|Yes|-|
|Without public IP address|No|Yes|
|On-premise ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|Oracle|With public IP address|Yes|-|
|Without public IP address-|No|Yes|
|On-premise ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|DRDS|ApsaraDB|Classic network|Yes|-|
|VPC network|Under development|Yes|
|HybridDB for MySQL|ApsaraDB|Classic network|Yes|-|
|VPC network|Under development|Yes|
|HybridDB for PostgreSQL released|ApsaraDB|Classic network|Yes|-|
|VPC network|Under development|Yes|
|MaxCompute \(for MaxCompute data sources\)|ApsaraDB|Classic network|Yes|-|
|AnalyticDB \(for ADS data sources\)|ApsaraDB|Classic network|Yes|-|
|VPC network|Under development|Yes|
|OSS|ApsaraDB|Classic network|Yes|-|
|VPC network|Yes|-|
|HDFS|With public IP address|Yes|-|
|On-premise ECS|Classic network|Yes|-|
|VPC network|No|-|
|FTP|With public IP address|Yes|-|
|Without public IP address|No|-|
|On-premise ECS|Classic network|Yes|-|
|VPC network|No|-|
|MongoDB|ApsaraDB|Classic network|Yes|-|
|VPC network|Under development|Yes|
|With public IP address|Yes|-|
|On-premise ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|Memcache|ApsaraDB|Classic network|Yes|-|
|VPC network|Under development|Yes|
|Redis|ApsaraDB|Classic network|Yes|-|
|VPC network|Under development|Yes|
|With public IP address|Yes|-|
|On-premise ECS|Classic network|Yes|-|
|VPC network|No|Yes|
|Table Store \(for OTS data sources\)|ApsaraDB|Classic network|Yes|-|
|VPC network|Under development|Yes|
|DataHub|ApsaraDB|Classic network|Yes|-|
|VPC network|No|-|

**Note:** For more information about when to add a Custom Resource Group, see [Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

## Description {#section_uy4_1hv_42b .section}

In the preceding table, "-" means this item is unavailable. "No" means the connectivity test failed and a custom resource group must be added, and the synchronization task can be configured.

-   Data sources in VPC environment:
    -   Connectivity tests for RDS data sources in VPC environment is supported.
    -   Other data sources in VPC environment are under development.
    -   Financial Cloud networks does not support connectivity tests.
-   User-created ECS data sources:
    -   The classic network typically supports JDBC-based connectivity tests on the public network.
    -   The VPC does not support connectivity tests for now.
    -   Currently, cross-region sources connectivity tests is not supported.
    -   Financial Cloud networks do not support connectivity tests.

Currently, data synchronization is implemented solely by adding a custom resource group. For more information, see [Data Synchronization Configuration for the VPC \(Financial Cloud\)](https://www.alibabacloud.com/help/zh/doc-detail/55474.html).

For created ECS data sources, add the scheduling cluster IP address to the security group for both inbound and outbound traffic in the public network and classic network. If the security group is not added, a disconnection error may occur during synchronization. For more information, see [Add security group](reseller.en-US/User Guide/Data integration/Common configuration/Add security group.md#).

You cannot add an extensive port range on the ECS security group page. To add them, use the security group API of ECS. For more information, see [AuthorizeSecurityGroup](https://www.alibabacloud.com/help/doc-detail/25554.htm).

-   Data sources created in local IDCs or on the ECS server without public IP addresses:
    -   Connectivity tests are not supported.
    -   A custom resource group must be added for configuring the synchronization tasks.
-   Public-network-based JDBC is applied to data sources created in local IDCs or on the ECS server with public IP addresses for connectivity tests. If the connectivity test fails, check the limits of the local network or relevant databases.


**Note:** 

The following example describes the billing of synchronizing data from RDS to MaxCompute:

Currently, data integration is free of charge, but you might still be billed for certain products. Configuring MaxCompute data synchronization in DataWorks is free of charge, but you will be billed for manually adding the parameter in the script mode to set a public IP address for the MaxCompute tunnel. However, this parameter is unavailable in the template generated in the script mode.

## Conclusion { .section}

When a connectivity test fails, you need to verify the data source region, network type, and whether the full instance ID, database name and user name are valid in the RDS whitelist. Examples of common errors are as follows:

-   The Database Password is invalid as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16196/15514304407520_en-US.png)

-   The network connection failed as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16196/15514304407521_en-US.png)

-   The network disconnected during synchronization or because of other conditions.

    View the full log to locate the scheduled resource and to determine whether it is a custom resource.

    If so, check whether the IP address of the custom resource group has been added to the data source whitelist, such as RDS. This also applies to MongoDB.

    Check whether connectivity tests between both data sources was successful and if their whitelists are complete. The test result will vary, if the whitelists are incomplete. Specifically, the test is successful if the task is assigned to the added scheduling server or failed if no scheduling server has been added.

-   For tasks that are successful, but the disconnection error 8000 is found in the log:

    This condition occurs when the custom scheduling resource group is used and the IP address 10.116.134.123 and port 8000 does not have security group inbound traffic permission. Under this condition, add the IP address and the port, and run the task again.


## Connectivity test exception examples {#section_unr_1kv_42b .section}

-   Example 1

    A database test connection error occurred resulting in a data source connectivity test exception. The database connection string is "jdbc:mysql://xx.xx.xx.x:xxxx/t\_uoer\_bradef", the user name is "xxxx\_test", and the exception message is "Access denied for user "xxxx\_test"@"%" to database "yyyy\_demo"".

-   Troubleshooting
    1.  Check if the entered information is valid.
    2.  Check if the password, whitelist, or your account has permission to access the database. You can add the required permissions in the RDS console.
-   -   Example 2

    A test connection exception occurred resulting in the data source connectivity test exception. The displayed error message is as follows:

    ```
    error message: Timed out after 5000 ms while waiting for a server that matches ReadPreferenceServerSelector{readPreference=primary}. Client view of cluster state is {type=UNKNOWN, servers=[(xxxxxxxxxx), type=UNKNOWN, state=CONNECTING, exception={com.mongodb.MongoSocketReadException: Prematurely reached end of stream}}]
    ```

-   Troubleshooting

    If you are using MongoDB without VPC connection. You must add a whitelist for the connectivity test of the MongoDB data source. For more information, see [Add whitelist](reseller.en-US/User Guide/Data integration/Common configuration/Add whitelist.md#).


