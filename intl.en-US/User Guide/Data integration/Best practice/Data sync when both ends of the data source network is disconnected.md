# Data sync when both ends of the data source network is disconnected {#concept_gbk_hx1_r2b .concept}

## Scenario {#section_hbk_hx1_r2b .section}

The following are characteristics of the complex network environment in the following two scenarios:

-   When the data source or the data target is in the private network environment.
    -   VPC environment \(with the exception of RDS\) <-\>Public network environment
    -   Financial Cloud environment <-\> Public network environment
    -   Local user-created environment without public network <-\> Public network environment
-   When both the data source and target are in the private network environment.
    -   VPC environment \(with the exception of the RDS\) <-\> VPC environment \(with the exception of the RDS\)
    -   Financial Cloud environment <-\> Financial Cloud environment
    -   Local user-created environment without public network <-\> Local user-created environment without the public network
    -   Local user-created environment without public network <-\> VPC environment \(with the exception of the RDS\)
    -   Local user-created environment without public network <-\> Financial Cloud environment

Data Integration provides network penetration capability in complex network environment. By deploying Data Integration agents, the synchronous data transmission can be implemented between any network environment. The following describes the specific implementation logic and procedures assumes that the data source network on both ends cannot be connected. For more information on scenarios where one end is unreachable, see [Data sync when both ends of the data source network is disconnected](reseller.en-US/User Guide/Data integration/Best practice/Data sync when both ends of the data source network is disconnected.md#).

## Implementation logic {#section_lbk_hx1_r2b .section}

For complex network environments where both ends of the data source are in the private network environment, you must deploy the Data Integration agent for both ends in the same network environment. In this case, the source agent is used to push data to the Data Integration server, and the target agent is for pulling the data to the local device. Data transmission timeliness and security are ensured by data blocking, compression, and encryption during data transmission.

## Procedure {#section_nbk_hx1_r2b .section}

Configure the Data Source

1.  Log on to the [DataWorks console](https://partners-intl.aliyun.com) as a developer, and click Enter Project to enter the project management page.
2.  Click **Data Integration** from the upper menu and go to the Offline Sync \> Data Sources page.
3.  Click **New Source** to show the supported data source types.
4.  Select the data source without a public IP address from the FTP data sources.

    Add a data source.

    Configuration item description:

    -   Type: The data source without a public IP address.
    -   Name:The name can contain letters, numbers, and underscores \(\_\). It must start with a letter or an underscore \(\_\) and cannot exceed 60 characters in length.
    -   Description: The brief description of the data source that cannot exceed 80 characters in length.
    -   Select resources group:The machine on which the agent is deployed. The resource agent to push data to the Data Integration server. For more information on how to add the resource group, see [Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).
    -   Protocol: FTP or SFTP.
    -   \*Host: The default FTP port is port 21, while the default SFTP port is port 22.
    -   Username/Password: The user name and password used for connecting the database.
    -   Test connectivity: The data sources with public IP addresses do not support connectivity tests. Click Finish to complete the source-end configuration.
    Add a target data source

    Resource group: The machine on which the target agent is deployed. The target agent is used for pulling data to the local device.  For more information on how to add the resource group, see [Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).


## Select the Script Mode {#section_qbk_hx1_r2b .section}

1.  Click Data Integration from the upper menu, and go to Sync Tasks page.
2.  Choose **New** \> **Script Mode** on the page.

    On the Script Mode page, select a template that contains the key parameters for synchronization tasks, and enter the required information. Note that the Script Mode cannot be switched to Wizard Mode.

3.  Select the FTP to FTP import template.
    -   Source type: The data source name is automatically selected based on the data source selected in the Wizard Mode.
    -   Target type: You can select a target data source from the drop-down menu.

        **Note:** If the database supports adding data sources on the page, you can select data sources from the template. Otherwise, you must edit relevant data source information in the JSON code section of the template, and then click Add Data Source.

4.  Configure a synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16272/15515104568574_en-US.png)

    -   Because machine 1 cannot access the public network, the agent deployment requires machine 2to belong in the same network segment and have access to the public network.
    -   Set machine 2 as the scheduling resource group, and run the synchronization task on the machine.

## Procedure {#section_tbk_hx1_r2b .section}

Configure the Data Source

1.  Enter the [DataWorks management console](https://partners-intl.aliyun.com) as a developer, and click Enter workspace in the corresponding project Action column.
2.  Click Data Integration in the top menu bar and go to the Data Source page.
3.  Click Add Data Source to display the supported data source types.
4.  Select the data source without a public IP address from the data sources from the relational database MySQL.
    -   The data source \(without public IP address\).

        The configuration items are as follows:

        -   Data source type: The data source without a public IP address.
        -   Data source name: The name must contain letters, numbers, and underscores \(\_\). It must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length.
        -   Data source description:A brief description of the data source that cannot exceed 80 characters in length.
        -   Resource group: The machine in which the target agent is deployed to connect to the external public network. The synchronization task of the data source in the special network environment can run in the resource group. To add a resource group, see Add Scheduling Resources. For more information about adding resource groups, see [Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).
        -   JDBC URL: The JDBC URL in the format: jdbc:mysql://ServerIP:Port/Database.
        -   User name/Password: The user name and password used to connect to the database.
        -   Test Connectivity: The data source for public network IP address that does not support test connectivity, and click **Finish**.
        -   Target data source \(with a public network\).

            Parameters:

            -   Data source name:The name contains letters, numbers, and underscores \(\_\). It must start with a letter or underscore \(\_\) and cannot exceed 60 characters in length.
            -   Data source description: A brief description of the data source that cannot exceed 80 characters in length.
            -   MaxCompute endpoint: By default, the endpoint is read-only. The endpoint value is automatically read from the system configuration.
            -   MaxCompute project name: The MaxCompute project indicator.
            -   Access Id: The Access ID of the MaxCompute project owner account.
            -   Access Key: The Access Key of the MaxCompute project owner account, that is used with the Access ID. The access key is equivalent to the logon password.
            -   Connectivity test: The connectivity test is supported.

## Configure a synchronization task {#section_dck_hx1_r2b .section}

1.  Select the source.

    Because the data source has no public IP address, the data source network is unavailable. You must run the synchronization task in Script Mode. Click Switch Script.

2.  Import a template.

    Parameter description:

    -   Source type: The data source name is automatically selected based on the selected data source in Wizard Mode.
    -   Target type: You can select a target data source from the drop-down menu.
    **Note:** If the database supports adding data sources on the page, you can select data sources from the template. Otherwise, you must edit relevant data source information in the JSON code section of the template, and click Add Data Source.

3.  An example of how to switch the Script Mode.

    Configure the resource groups: You can edit and view resource groups of the synchronization task. The default source and target groups are the resource groups you selected when adding the data source.

    ```
    {
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
         "fieldDelimiter": ",",//Delimiter
         "encoding": "UTF-8",//Encoding format
         "column": //Data source column
           {
             "index": 0,
             "type": "string",
           },
           {
             "index": 1,
             "type": "string",
           }
         ],
         "path": //File path
           "/home/wb-zww354475/ww.txt"
         ],
         "datasource": "lzz_test3"//Data source name, which must be consistent with the name of the added data source
       },
       "plugin": "ftp"
     },
     "writer": {
       "parameter": {
         "writeMode": "truncate",//Writing mode
         "fieldDelimiter": ",",//Delimiter
         "fileName": "ww",//File name
         "path": "/home/wb-zww354475/ww_test",//File path
         "dateFormat": "yyyy-MM-dd HH:mm:ss",
         "datasource": "lzz_test4",//Data source name, which must be consistent with the name of the added data source
         "fileFormat": "csv"//File type
       },
       "plugin": "ftp"
     }
    },
    "Type": "job ",
    "version": "1.0"
    }
    ```


## Run a synchronization task {#section_jck_hx1_r2b .section}

You can run the synchronization task using the following methods:

-   Click Run on the page of Data Integration.
-   Schedule the task. For more information on how to configure related scheduling, see [Scheduling configuration](reseller.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

