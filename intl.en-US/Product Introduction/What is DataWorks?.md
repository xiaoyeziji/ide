# What is DataWorks? {#concept_wqv_qbp_r2b .concept}

DataWorks is an important Alibaba Cloud Platform as a service \(PaaS\) product. It offers fully hosted workflow services and a one-stop data development and management interface to help enterprises mine and comprehensively explore data value.

DataWorks uses MaxCompute as the core computing and storage engine to provide massive offline data processing, analysis, and mining capabilities. For more information, see [MaxCompute overview](../../../../../reseller.en-US/Product Introduction/What is MaxCompute.md#).

DataWorks simplifies data transmission and conversion .You can import data from different storage services, convert and ultimately extract data that is transmitted to other data systems. See the following figure for a comprehensive overview of DataWorks data analysis process.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16167/15476086968910_en-US.png)

## Features {#section_nbf_cdp_r2b .section}

-   **Fully-hosted scheduling**

    DataWorks provides powerful scheduling capabilities. Based on Directed Acyclic Graph \(DAG\) relationships, the time-based or dependency-based tasks trigger configurations to perform tens of millions of tasks punctually and precisely daily. The multiple scheduling frequency configurations are supported by minute, hourly, daily, weekly, and monthly basis.

    The fully-hosted service eliminates all server resource scheduling concerns. The system isolates different tenants to guarantee tasks run independently.

-   **Supports various task types**

    DataWorks supports multiple task types, such as data synchronization, SHELL, MaxCompute SQL, and MaxCompute MR tasks. Complex data analysis processes are based on dependencies between tasks.

    -   Powered by MaxCompute, DataWorks provides powerful data conversion capabilities to guarantee high performance of big data analysis.
    -   For data synchronization, DataWorks relies on powerful data integration capabilities to support more than 20 types of data sources and provide stable and highly-efficient data transmission. For more information, see [Data Integration Overview](../../../../../reseller.en-US/User Guide/Data integration/Data integration introduction/Data Integration Overview.md#).
-   **Data visualization development**

    This product offers visualization code development and workflow designer pages. No additional development tools are required to drag and drop components to develop complex data analysis tasks. Development tasks can be performed from anywhere in the globe through Internet connection and web browsers.

-   **Monitoring and alarms**

    The O&M center provides visual task monitoring and management tools, and displays global conditions in DAG format when tasks are running.

    SMS alarms can be easily configured to notify the relevant alarm contact of task errors for immediate troubleshooting.


## Constraints and limits { .section}

-   DataWorks only supports Chrome 54 or later versions.
-   Currently, DataWorks only supports SQL operations on Alibaba Cloud's MaxCompute.

