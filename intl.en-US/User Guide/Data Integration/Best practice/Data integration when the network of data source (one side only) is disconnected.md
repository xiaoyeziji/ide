# Data integration when the network of data source \(one side only\) is disconnected {#concept_j41_l1v_q2b .concept}

This article demonstrates how to migrate a full MySQL database to MaxCompute with the full-database migration feature.

## Scenario {#section_fn2_s1v_q2b .section}

Complex network environments are characteristic of the following two conditions.

-   Either the data source or the data target is in the private network environment.
    -   VPC environment \(except the RDS\) <-\> Public network environment
    -   Financial Cloud environment <-\> Public network environment
    -   Local user-created environment without the public network <-\> Public network environment
-   Both the data source and target are in the private network environment.
    -   VPC environment \(except the RDS\) <-\> VPC environment \(except the RDS\)
    -   Financial Cloud environment <-\> Financial Cloud environment
    -   Local user-created environment without the public network lt;-\> Local user-created environment without the public network
    -   Local user-created environment without the public network <-\> VPC environment \(except the RDS\)
    -   Local user-created environment without the public network <-\> Financial Cloud environment

Data Integration provides the network penetration ability in the complex network environments. By deploying Data Integration agents, synchronous data transmission can be implemented between any network environments.  The following describes the specific implementation logics and procedures and assumes that the network of both ends of data sources cannot be connected.

## Implementation logics {#section_a5m_wfv_q2b .section}

For the complex network environments where either the data source or the data target is in the private network environment, deploy the Data Integration agent on the machine in the same network environment as that of the end which is in the private environment and connect to the external public network through the agent.  Private network environments are characteristic of the following two conditions:

-   The database built on ECS is purchased with no public IP address or elastic public IP address assigned.
-   Type: Data source without a public IP address.

## ECS {#section_hwc_yfv_q2b .section}

The data synchronization method in this scenario is shown in the following figure:

![](images/8568_en-US.png)

-   Because ECS2 server cannot access the public network, an ECS1 machine that is in the same network segment as ECS2 and has the ability to access the public network is required for agent deployment.
-   Set ECS1 as the resource group, and run the synchronization task on the machine.

**Note:** 

You need to grant database permissions to the ECS2 server to access relevant database and read the data of the database to ECS1. The command for granting permissions is as follows:

```
grant all privileges on *.* to 'demo_test'@'%' identified by ''Password'; --> % means granting permissions to any IP addresses<br>.
```

The user-created data source synchronization task on ECS2 runs in the custom resource group. To authorize the machine of the custom resource group, you must add internal and external IP address and the port of ECS2 to the safety group of ECS1. See[Add security group](intl.en-US/User Guide/Data Integration/Common configuration/Add security group.md#) for more information.

## Local IDC with no public IP address {#section_ojs_3hv_q2b .section}

The data synchronization method in this scenario is shown in the following figure:

![](images/8574_en-US.png)

-   Because machine 1 cannot access the public network, an machine 2 that is in the same network segment as machine 1 and has the ability to access the public network is required for agent deployment.
-   Set machine 2 as the scheduling resource group, and run the synchronization task on the machine.

## Procedure {#section_izp_flv_q2b .section}

Configure the Data Source

1.  Enter the[DataWorks management console](https://account.aliyun.com/login/mixlogin.htm?oauth_callback=https%3A%2F%2Fsso.data.aliyun.com%2Faliyun%2FaliyunCallback%3Fredirect%3Dhttps%253A%252F%252Fworkbench.data.aliyun.com%252Fconsole%253Fspm%253D5176.doc47762.2.4.fUQkD2) as a developer, and click Enter workspace in the corresponding project action bar.
2.  Click Data Integration from the top menu bar and navigate to the Data Source page.
3.  2. Click Add Data Source to show the supported data source types.  As shown in the following figure:

    ![](images/8575_en-US.png)

4.  Select the data source without a public IP address from the data sources for the relational database MySQL.

    -   Source data source \(with no public IP\):
    -   ![](images/8578_en-US.png)

        The configuration items are as follows:

        -   Data source type: data source without a public IP address.
        -   Data source name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
        -   Data source description: It is a brief description of the data source with no more than 80 characters.
        -   Resource group: The machine on which the target agent is deployed to connect to the external public network. The synchronization task of data source in special network environment can run in the resource group. To add source group, see Add Scheduling Resources. For more information on adding resource groups, see[Add scheduling resources](intl.en-US/User Guide/Data Integration/Common configuration/Add scheduling resources.md#).
        -   JDBC URL: the JDBC URL. Format: jdbc:mysql://ServerIP:Port/database.
        -   User name/Password: The user name and password used to connect to the database.
        -   Test Connectivity: the data source for public network IP does not support test connectivity, just click **Finish**.
        -   Target data source \(with a public network\):

            ![](images/8580_en-US.png)

            Parameters:

            -   Data source name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
            -   Data source description: It is a brief description of the data source with no more than 80 characters.
            -   ODPS endpoint: defaults to read-only. The value is automatically read from the system configuration.
            -   ODPS project name: the corresponding MaxCompute project indicator.
            -   Access ID: the Access ID corresponding to the MaxCompute project owner's cloud account.
            -   Access Key: The Access Key of the MaxCompute Project Owner cloud account, used in combination with the Access ID. The access key [accesskey \(AK\)](https://help.aliyun.com/document_detail/53045.html) is equivalent to the login password.
            -   Connectivity test: the connectivity test is supported.

**Configure a synchronization task**

1.  Select the source

    Select the source. Because the data source has no public IP, the network of the data source is unavailable. You must run the synchronization task in the script mode. Click Switch Script button directly.

    ![](images/8586_en-US.png)

2.  Import a template

    ![](images/8587_en-US.png)

    Parameter description:

    -   Source type: The data source name is automatically selected base on the data source selected in the wizard mode.
    -   Target type: You can select a target data source from the drop-down list.
    **Note:** If adding data sources on the page is supported by the database, you can select data sources from the template. If not, you must edit relevant data source information in JSON code section of the template and then click Add Data Source directly.

3.  An example of how to switch into the script mode.

    ![](images/8588_en-US.png)


Configure the resource groups:You can change and view the resource groups for the synchronization task. Collapsed by default.

```
{
"type": "job",
"configuration": {
 "setting": {
   "speed": {
     "concurrent": "1",//Number of concurrent tasks
     "mbps": "1"//Maximum task speed
   },
   "errorLimit": {
     "record": "0"//Maximum number of error records
   }
 },
 "reader": {
   "parameter": {
     "Splitpk": "ID", // cut key
     "column": [//Target column name
       "name",
       "tag",
       "age",
       "balance",
       "gender",
       "birthday"
     ],
     "table": "source", // source name
     "where": "ds = '20171218"", // filter criteria
     "datasource": "private_source"//Data source name, which must be consistent with the name of the added data source
   },
   "plugin": "mysql"
 },
 "writer": {
   "parameter": {
     "partition": "pt=${bdp.system.bizdate}",//The partition information.
     "truncate": true,
     "column": [//Target column name
       "name",
       "tag",
       "age",
       "balance",
       "gender",
       "birthday"
     ],
     "table": "random_generated_data",//Table name of the target end
     "datasource": "odps_mrtest2222"//Data source name, which must be consistent with the name of the added data source
   },
   "plugin": "odps"
 }
},
"version": "1.0"
}
```

## Run a synchronization task {#section_gcf_npv_q2b .section}

You can run the synchronization task in the following methods:

-   Click Run in the page of the Data Integration.
-   Schedule the task. For the configuration of related scheduling, see [scheduling configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#).

![](images/8589_en-US.png)

