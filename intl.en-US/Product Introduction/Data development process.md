# Data development process {#concept_kz4_1jp_r2b .concept}

The data development process comprises of data generation, data collection and storage, data analysis and processing, data extraction, and data presentation and sharing. See the following for a graphical representation of the process:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16170/15389843558922_en-US.png)

**Note:** 

In the preceding figure, the data development processes inside the dotted box are completed on the Alibaba Cloud Big Data Platform.

The data development process is explained as follows:

-   **Data generation**

    A business system generates a large amount of structured data every day. The data is stored in business system databases, such as MySQL, Oracle, and RDS.

-   **Data collection and storage**

    To use MaxCompute’s massive data storage and processing capabilities for data analysis, you must synchronize the data from different business systems to MaxCompute.

    DataWorks provides data integration services for you to synchronize various types of data from business systems to MaxCompute according to predefined scheduling periods.

-   **Data analysis and processing**

    Next, you can start to process \(ODPS\_SQL and OPEN\_MR\), analyze, and mine \(data analysis and data mining\) the data on MaxCompute to find valuable information.

-   **Data extraction**

    The data after analysis and processing must be synchronized to your business system for further use.

-   **Data presentation and sharing**

    Finally, the results of big data analysis and processing are presented and shared as reports, geographical information systems, and in number of different accessible formats.


