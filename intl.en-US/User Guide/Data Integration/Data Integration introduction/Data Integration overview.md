# Data Integration overview {#concept_dr3_k2v_42b .concept}

The Alibaba Group offers Data Integration - a data synchronization platform that provides stable, efficient, and elastically scalable services. The Data Integration is designed to implement fast and stable data movement and synchronization between multiple heterogeneous data sources in complex network environments.

## Offline \(batch\) data synchronization {#section_ubn_pn5_42b .section}

The offline \(batch\) data channel provides a set of abstract data extraction plug-ins \(Readers\) and data writing plug-ins \(Writers\) by defining the source and target databases and data sets. Also, it designs a set of simplified intermediate data transmission formats based on the framework to transfer the data between any structured and semi-structured data sources.

![](images/7498_en-US.png)

## Supported data source types {#section_ikd_b45_42b .section}

Data Integration provides extensive options for data sources shown as follows:

-   Text storage \(FTP/SFTP/OSS/Multimedia files\),
-   Database \(RDS/DRDS/MySQL/PostgreSQL\),
-   NoSQL \(Memcache/Redis/MongoDB/HBase\),
-   Big data \(MaxCompute/AnalyticDB/HDFS\),
-   MPP database \(HybridDB for MySQL\).

For more information, see [Supported data sources](intl.en-US/User Guide/Data Integration/Data source configuration/Supported data sources.md#).

**Note:** The configuration information of different data sources varies dramatically from each other, and the parameter configuration information needs to be queried in detail based on the actual use case. For this reason, detailed description of parameters is available on the data source configuration and job configuration pages, which can be queried and used as needed.

## Description of synchronization development {#section_ct3_k45_42b .section}

Synchronous development provides both the wizard mode and the script mode.

-   **Wizard**: Provides wizard-like visualized development guidance and comprehensive details about configuration of data sync tasks. This mode is cost-effective but lacks certain advanced functions.
-   **Script**: Allows you to directly write a data sync JSON script to complete data sync development. It is suitable for the advanced users but incurs a high learning cost. It also provides a rich set of flexible functions for refined configuration management.

**Note:** 

-   The code generated in the wizard mode can be converted to the script mode code. The conversion is unidirectional, and the code cannot be converted back to the wizard mode code. This is because that the capabilities of the script mode are a superset of those of the wizard mode.
-   Always configure the data source and create the target table before writing codes.

## Description of network types {#section_nzk_gw5_42b .section}

Networks can be classified as the classic network, VPC, and local IDC network \(planning\).

-   Classic network: A network that is centrally deployed on the Alibaba Cloud public infrastructure network and planned and managed by Alibaba Cloud. This type of network suits for customers with demanding ease-of-use requirements.
-   VPC: An isolated network environment created based on Alibaba Cloud. For this type of network, you have full control over your virtual network, including customizing the IP address range, partitioning network segments, and configuring routing tables and gateways.
-   Local IDC network: The network environment of your server room, which is isolated from the Alibaba Cloud network.

See classic network and VPC Frequently Asked Questions for questions related to [classic and proprietary networks](https://www.alibabacloud.com/help/doc-detail/54489.htm).

Note:

-   Public network access is supported - just select the classic network as the network type. Note the speed of the public network bandwidth and relevant network traffic charges when using this type of network. It is not recommended except for special cases.
-   Network connections are planned for data synchronization, you can use the locally added resource + Script Mode scheme for synchronous data transfer, you can also use the shell + datax scheme.
-   The Virtual Private Cloud \(VPC\) creates an isolated network environment and allows you to customize the IP address range, network segments, and gateways. VPC applications expand as the VPC security improves, and thus Data Integration provides RDS for MySQL, RDS for SQL Server, and RDS for PostgreSQL and eliminates the need to purchase extra ECSs that reside on the same network as the VPC. Instead, the system ensures interconnectivity by detecting devices automatically through the reverse proxy. The support for other Alibaba Cloud databases including PPAS, OceanBase, Redis, MongoDB, Memcache, TableStore, and HBase will also be available in the future. For any non-RDS data sources, an ECS on the same network is required for configuring data integration synchronization tasks on the VPC network and ensuring interconnectivity.

## Limits {#section_isw_gw5_42b .section}

-   Only structured \(such as RDS and DRDS\), semi-structured, and non-structured \(such as OSS and TXT, but the specific synchronization data must be abstracted as structured data\) data synchronization are supported. That is, Data Integration supports data synchronization that is capable of transmitting data that can be abstract to a logical two-dimensional table, other fully unstructured data, such as a section of MP3 stored in Oss, the data integration does not yet support synchronizing it to maxcompute, which is implemented later.
-   Data synchronization and exchange between a single and certain cross-region data storage are supported.

    For certain regions, cross-region data transmission is supported but not guaranteed by the classic network. If you do need to use this function while the classic network is tested disconnected, consider using the public network connection instead.

-   Only data synchronization \(transmission\) is performed and no consumption plans of data stream is provided.

## References {#section_q1q_hw5_42b .section}

-   For a detailed description of the data synchronization task configuration, see [creating a data synchronization task](intl.en-US/User Guide/Data Integration/Task Configuration/Data Synchronization task configuration/Wizard mode configuration.md#).
-   For a detailed introduction to processing unstructured data such as OSS, see [accessing OSS unstructured data](https://www.alibabacloud.com/help/doc-detail/45389.html).

