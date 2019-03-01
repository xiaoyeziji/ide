# Data development process {#concept_kz4_1jp_r2b .concept}

The data development process comprises data generation, data collection and storage, data analysis and processing, data extraction, and data presentation and sharing. See the following graphical process representation .

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16170/15514201088922_en-US.png)

**Note:** 

In the preceding figure, data development processes within the dotted box are completed on the Alibaba Cloud Big Data Platform.

The data development process is as follows:

-   **Data generation**

    A business system generates a large amount of structured data every day. The data is stored in business system databases, such as MySQL, Oracle, and RDS.

-   **Data collection and storage**

    To use MaxCompute’s massive data storage and processing capabilities for data analysis, you must synchronize data from different business systems to MaxCompute.

    DataWorks provides data integration services so you can synchronize various data types from business systems to MaxCompute according to predefined scheduling periods.

-   **Data analysis and processing**

    Next, you can process \(MaxCompute\_SQL and OPEN\_MR\), analyze, and mine \(data analysis and data mining\) the data on MaxCompute to find valuable information.

-   **Data extraction**

    The data after analysis and processing must be synchronized to your business system for further use.

-   **Data presentation and sharing**

    Finally, the results of big data analysis and processing are presented and shared as reports, geographical information systems, and in different accessible formats.


