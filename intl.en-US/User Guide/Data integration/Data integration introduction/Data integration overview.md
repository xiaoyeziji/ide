# Data integration overview {#concept_dr3_k2v_42b .concept}

The Alibaba Cloud Data Integration is a data synchronization platform that provides stable, efficient, and elastically scalable services. Data integration is designed to implement fast and stable data migration and synchronization between multiple heterogeneous data sources in complex network environments.

## Offline \(batch\) data synchronization {#section_ubn_pn5_42b .section}

The offline \(batch\) data channel provides a set of abstract data extraction plug-ins \(Readers\) and data writing plug-ins \(Writers\) by defining the source and target databases and datasets. Also, it designs a set of simplified intermediate data transmission formats based on the framework to transfer data between any structured and semi-structured data sources.

## Supported data source types {#section_ikd_b45_42b .section}

Data integration supports diverse data sources as follows:

-   Text storage \(FTP, SFTP, OSS, Multimedia files\),
-   Database \(RDS,DRDS,MySQL,PostgreSQL\),
-   NoSQL \(Memcache,Redis,MongoDB,HBase\),
-   Big data \(MaxCompute,AnalyticDB,HDFS\),
-   MPP database \(HybridDB for MySQL\).

For more information, see [Supported data sources](reseller.en-US/User Guide/Data integration/Data source configuration/Supported data sources.md#).

**Note:** The data sources configured information varies greatly from each other, and the parameter configuration information must be queried in detail based on the actual scenario. For this reason, the detailed parameter descriptions are available on the data source configuration and job configuration pages, which can be queried and used as needed.

## Synchronous development description {#section_ct3_k45_42b .section}

Synchronous development provides both wizard and script modes.

-   **Wizard**: Provides a visualized development guide and comprehensive details about data sync task configuration. This mode is cost-effective, but lacks certain advanced functions.
-   **Script**: Allows you to directly write a data sync JSON script for completing the data sync development. It is suitable for advanced users, but has a high learning cost. It also provides diverse and flexible functions for delicacy configuration management.

**Note:** 

-   The code generated in wizard mode can be converted to script mode code. The code conversion is unidirectional, and cannot be converted back to wizard mode format. This is because the script mode capabilities are a superset of the wizard mode.
-   Always configure the data source and create the target table before writing codes.

## Description of network types {#section_nzk_gw5_42b .section}

The networks can be classified as classic network, VPC network, and local IDC network \(planning\).

-   Classic network: A network that is centrally deployed on the Alibaba Cloud public infrastructure network planned and managed by Alibaba Cloud. This network type suits customers that have ease-of-use requirements.
-   VPC network: An isolated network environment created on Alibaba Cloud. In this network type, you have full control over the virtual network, including customizing the IP address range, partitioning network segments, and configuring routing tables and gateways.
-   Local IDC network: The network environment of your server room, which is isolated from the Alibaba Cloud network.

See classic network and VPC FAQ page for questions related to [classic and VPC networks](https://www.alibabacloud.com/help/doc-detail/54489.htm).

Note:

-   The public network access is supported. The public network access only selects the classic network as the network type. Note the public network bandwidth speed and relevant network traffic charges when using this network type. We do not recommend this configuration except in special cases.
-   Network connections are planned for data synchronization, you can use the locally added resource + Script Mode scheme for synchronous data transfer, you can also use the Shell + DataX scheme.
-   The Virtual Private Cloud \(VPC\) creates an isolated network environment that allows you to customize the IP address range, network segments, and gateways. The VPC applications have expanded the scope of VPC security, as a result data integration provides RDS for MySQL, RDS for SQL Server, and RDS for PostgreSQL and eliminates the need to purchase extra ECSs that reside on the same network as the VPC. Instead, the system guarantees interconnectivity by detecting devices automatically through the reverse proxy. The VPC supports other Alibaba Cloud databases including PPAS, OceanBase, Redis, MongoDB, Memcache, TableStore, and HBase. For any non-RDS data sources, an ECS on the same network is required for configuring data integration synchronization tasks on the VPC network and ensuring interconnectivity.

## Limits {#section_isw_gw5_42b .section}

-   Supports the following data synchronization types: structured \(such as RDS and DRDS\), semi-structured, and non-structured, such as OSS and TXT.The specified synchronization data must be abstracted as structured data. That is, data integration supports data synchronization that can transmit data that can be abstracted to a logical two-dimensional table, other fully unstructured data, such as a MP3 section stored in OSS. Data integration does not support synchronizing dataset to MaxCompute, which is still in development.
-   Supports data synchronization and exchange between single region and cross-region data storage.

    For certain regions, cross-region data transmission is supported, but not guaranteed by the classic network. If you need to use this function, while the tested classic network is disconnected, consider using the public network connection instead.

-   Only data synchronization \(transmission\) is performed and no consumption plans of data stream is provided.

## References {#section_q1q_hw5_42b .section}

-   For a detailed description of data synchronization task configuration, see [create a data synchronization task](reseller.en-US/User Guide/Data integration/Task configuration/Configure reader plug-in/Wizard mode configuration.md#).
-   For a detailed introduction to processing unstructured data such as OSS, see [access OSS unstructured data](https://www.alibabacloud.com/help/doc-detail/45389.html).

