# Data Integration when one side of the data source is disconnected {#concept_j41_l1v_q2b .concept}

This topic describes how to migrate a full MySQL database to MaxCompute with the Full-Database Migration feature.

## Scenario {#section_fn2_s1v_q2b .section}

The following are complex network environment features under the following two scenarios:

-   When either the data source or the data target is in the private network environment.
    -   VPC environment \(with the exception of RDS\) <-\> Public network environment
    -   Financial Cloud environment <-\> Public network environment
    -   Local user-created environment without the public network <-\> Public network environment
-   When both the data source and target are in the private network environment.
    -   VPC environment \(with the exception of RDS\) <-\> VPC environment \(with the exception of RDS\)
    -   Financial Cloud environment <-\> Financial Cloud environment
    -   Local user-created environment without the public network lt;-\> Local user-created environment without public network
    -   Local user-created environment without the public network <-\> VPC environment \(with the exception of RDS\)
    -   Local user-created environment without the public network <-\> Financial Cloud environment

Data Integration provides the network penetration capability in complex network environments. By deploying Data Integration agents, the synchronous data transmission can be implemented between any network environments.  The following describes the specific implementation logic and procedures under the assumption that both ends of the data source network cannot be connected.

## Implementation logic {#section_a5m_wfv_q2b .section}

In complex network environments, where either the data source or the data target is in the private network environment, deploy the Data Integration Agent on the machine in the same network environment as that in the network end. In the private environment, connect the external public network through the agent, where the private network environments characteristics meet the following two conditions:

-   The database built on ECS is purchased without public IP address or an assigned public elastic IP address .
-   Type: Data source without a public IP address.

## ECS {#section_hwc_yfv_q2b .section}

The data synchronization method in this scenario is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16272/15515089668568_en-US.png)

-   Because ECS2 server cannot access the public network, an ECS1 machine in the same network segment as ECS2 with the capability to access public network is required for agent deployment.
-   Set ECS1 as the resource group, and run the synchronization task on the machine.

**Note:** 

You need to grant database permissions on the ECS2 server to access relevant database to read the data of the database in ECS1. The command for granting permissions is as follows:

```
grant all privileges on *.* to 'demo_test'@'%' identified by ''Password'; --> % means granting permissions to any IP addresses<br>.
```

The user-created data source synchronization task on ECS2 runs in the custom resource group. To authorize the machine of the custom resource group, you must add internal and external IP addresses and the port of ECS2 to the ECS1 security group. For more information, see [Add security group](reseller.en-US/User Guide/Data integration/Common configuration/Add security group.md#).

## Local IDC without public IP address {#section_ojs_3hv_q2b .section}

The data synchronization method in this scenario is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16272/15515089668574_en-US.png)

-   Because machine 1 cannot access the public network, a machine 2 in the same network segment with access to the public network is required for agent deployment.
-   Set machine 2 as the scheduling resource group, and runs the synchronization task on the machine.

## Procedure {#section_izp_flv_q2b .section}

Configure the Data Source

1.  Enter the [DataWorks management console](https://partners-intl.aliyun.com) as a developer, and click Enter Workspace in the project Action column.
2.  Click Data Integration in the top menu pane and go to the Data Source page.
3.  Click Add Data Source to display the supported data source types.
4.  Select the data source without a public IP address from the data sources of the relational database MySQL.
    -   The data source \(without public IP address\).

        The configuration items are as follows:

        -   Data source type: The data source without a public IP address.
        -   Data source name: The name can contain letters, numbers, and underscores \(\_\) It must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length.
        -   Data source description: A brief description of the data source that cannot exceed 80 characters in length.
        -   Resource group: The machine in which the target agent is deployed to connect to the public network. The synchronization task of the data source in the special network environment can run in the resource group. For more information on how to add a resource group, see Add scheduling resources. For more information on adding a resource groups, see [Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).
        -   JDBC URL: The JDBC URL in the format: jdbc:mysql://ServerIP:Port/database.
        -   User name/Password: The user name and password used to connect to the database.
        -   Test connectivity: The data source for public network IP address does not support test connectivity, click **Finish**.
        -   Target data source \(with a public network IP address\).

            Parameters:

            -   Data source name: The name can contain letters, numbers, and underscores \(\_\). It must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length.
            -   Data source description: The brief description of the data source that does not exceed 80 characters in length.
            -   MaxCompute endpoint: By default, the endpoint is read-only. The value is automatically read from the system configuration.
            -   MaxCompute project name: The corresponding MaxCompute project indicator.
            -   Access ID: The Access ID of the MaxCompute project owner account.
            -   Access Key: The Access Key of the MaxCompute project owner account that is used with the Access ID. The Access Key is equivalent to the logon password.
            -   Connectivity test: The connectivity test is supported.

**Configure a synchronization task**

1.  Select the data source.

    Because the data source does not have a public IP address, you must run the synchronization task in Script Mode. Click Switch Script.

2.  Import a template.

    Parameter description:

    -   Source type: The data source name is automatically selected based on the selected data source from Wizard Mode.
    -   Target type: You can select a target data source from the drop-down menu.
    **Note:** If adding data sources on the page is supported by the database, you can select data sources from the template. If you cannot select a data source from the template, you must edit the relevant data source information in the JSON code section of the template, and then click Add Data Source.

3.  An example of how to switch the Script Mode.

    Configure the resource groups:You can edit and view the Resource Groups for the synchronization task, which by default is collapsed.

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

You can run the synchronization task using the following methods:

-   Click Run on the Data Integration page.
-   Schedule the task. For more information on the related scheduling configuration, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Parameter configuration.md#).

