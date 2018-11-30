# Data sync when the network of data source \(both sides\) is disconnected {#concept_gbk_hx1_r2b .concept}

## Scenario {#section_hbk_hx1_r2b .section}

Complex network environments are characteristic of the following two conditions.

-   Either the data source or the data target is in the private network environment.
    -   VPC environment \(except the RDS\) <-\>Public network environment
    -   Financial Cloud environment <-\> Public network environment
    -   Local user-created environment without the public network <-\> Public network environment
-   Both the data source and target are in the private network environment.
    -   VPC environment \(except the RDS\) <-\> VPC environment \(except the RDS\)
    -   Financial Cloud environment <-\> Financial Cloud environment
    -   Local user-created environment without the public network <-\> Local user-created environment without the public network
    -   Local user-created environment without the public network <-\> VPC environment \(except the RDS\)
    -   Local user-created environment without the public network <-\> Financial Cloud environment

Data Integration provides the network penetration ability in the complex network environments. By deploying Data Integration agents, synchronous data transmission can be implemented between any network environments.  The following describes the specific implementation logics and procedures and assumes that the network of both ends of data sources cannot be connected. For the scenarios where only one end is unreachable, see [Data sync when the network of data source \(both sides\) is disconnected](intl.en-US/User Guide/Data integration/Best practice/Data sync when the network of data source (both sides) is disconnected.md#).

## Implementation logics {#section_lbk_hx1_r2b .section}

For the complex network environments where both ends of data sources are in the private network environment, deploy the Data Integration agent for the both ends under the same network environment, where the source agent is for pushing data to the Data Integration server and the target agent is for pulling the data to the local device. During data transmission, the transmission timeliness and security are ensured by data blocking, compression, and encryption.

## Procedure {#section_nbk_hx1_r2b .section}

Configure the Data Source

1.  Log on to the [DataWorks console](https://workbench.shuju.aliyun.com/console) as a developer and click Enter Project to enter the project management page.
2.  Click **Data Integration** from the upper menu and navigate to the Offline Sync \> Data Sources page.
3.  Click **New Source** to show the supported data source types.
4.  Select the data source without a public IP address from the FTP data sources.

    Add a source data source.

    Configuration item description:

    -   Type: Data source without a public IP address.
    -   Name: It is a combination of letters, numbers, and underscores \(\). It must begin with a letter or an underscore \(\) and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source up to 80 characters.
    -   Select resources group: It is the machine on which the agent is deployed. The source agent is for pushing data to the Data Integration server.  To add source group, see [Add scheduling resources](intl.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).
    -   Protocol: ftp or sftp.
    -   \*Host: The default ftp port is port 21 and the default sftp port is port 22.
    -   Username/Password: The username and password used to connect to the database.
    -   Test Connectivity: Data sources with public IP addresses do not support connectivity tests. Click Finish to complete the source-end configuration.
    Add a target data source

    Resource group: The machine on which the target agent is deployed. The target agent is for pulling data to the local device.  To add source group, see [Add scheduling resources](intl.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).


## **Select the script mode** {#section_qbk_hx1_r2b .section}

1.  Click Data Integration from the upper menu, and go to Sync Tasks page.
2.  Choose **New** \> **Script Mode** on the page.

    On the script mode page, select an appropriate template that contains key parameters of synchronization tasks, and enter the required information. Note that the script mode cannot be switched to the wizard mode.

3.  Select the ftp-to-ftp import template.
    -   Source type: The data source name is automatically selected base on the data source selected in the wizard mode.
    -   Target type: You can select a target data source from the drop-down list.

        **Note:** If adding data sources on the page is supported by the database, you can select data sources from the template. If not, you must edit relevant data source information in JSON code section of the template and then click Add Data Source directly.

4.  Configure a synchronization task.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16272/15435669118574_en-US.png)

    -   Because machine 1 cannot access the public network, an machine 2 that is in the same network segment as machine 1 and has the ability to access the public network is required for agent deployment.
    -   Set machine 2 as the scheduling resource group, and run the synchronization task on the machine.

## Procedure {#section_tbk_hx1_r2b .section}

Configure the Data Source

1.  Enter the [DataWorks management console](https://partners-intl.aliyun.com) as a developer, and click Enter workspace in the corresponding project action bar.
2.  Click Data Integration from the top menu bar and navigate to the Data Source page.
3.  Click Add Data Source to show the supported data source types.
4.  Select the data source without a public IP address from the data sources for the relational database MySQL.
    -   Source data source \(with no public IP\).

        The configuration items are as follows:

        -   Data source type: data source without a public IP address.
        -   Data source name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
        -   Data source description: It is a brief description of the data source with no more than 80 characters.
        -   Resource group: The machine on which the target agent is deployed to connect to the external public network. The synchronization task of data source in special network environment can run in the resource group. To add source group, see Add Scheduling Resources. For more information on adding resource groups, see [Add scheduling resources](intl.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#).
        -   JDBC URL: the JDBC URL. Format: jdbc:mysql://ServerIP:Port/Database.
        -   User name/Password: The user name and password used to connect to the database.
        -   Test Connectivity: the data source for public network IP does not support test connectivity, just click **Finish**.
        -   Target data source \(with a public network\).

            Parameters:

            -   Data source name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
            -   Data source description: It is a brief description of the data source with no more than 80 characters.
            -   ODPS endpoint: defaults to read-only. The value is automatically read from the system configuration.
            -   ODPS project name: the corresponding MaxCompute project indicator.
            -   Access Id: the Access ID corresponding to the MaxCompute project owner's cloud account.
            -   Access Key: The Access Key of the MaxCompute Project Owner cloud account, used in combination with the Access ID. The access key is equivalent to the login password.
            -   Connectivity test: the connectivity test is supported.

**Configure a synchronization task**

1.  Select the source.

    Because the data source has no public IP, the network of the data source is unavailable. You must run the synchronization task in the script mode. Click Switch Script button directly.

2.  Import a template.

    Parameter description:

    -   Source type: The data source name is automatically selected base on the data source selected in the wizard mode.
    -   Target type: You can select a target data source from the drop-down list.
    **Note:** If adding data sources on the page is supported by the database, you can select data sources from the template. If not, you must edit relevant data source information in JSON code section of the template and then click Add Data Source directly.

3.  An example of how to switch into the script mode.

    Configure the resource groups: You can change and view the resource groups for the synchronization task. The default source and target groups are the resource groups that you selected when adding the data source.

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

You can run the synchronization task in the following methods:

-   Click Run in the page of the Data Integration.
-   Schedule the task. For the configuration of related scheduling, see [scheduling configuration](intl.en-US/User Guide/Data development/Scheduling Configuration/Basic attributes.md#).

