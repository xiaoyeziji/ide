# Supported data sources {#concept_uzy_hgv_42b .concept}

Data Integration is a stable, efficient, and elastically scalable data synchronization platform provided by the Alibaba Group to external users. It provides offline \(batch\) data access channels for Alibaba Cloud's big data computing engines \(including MaxCompute, AnalyticDB, and OSS\).

The following table lists the data source types supported by Data Synchronization:

|Data Source category|Data source type|Extraction \(Reader\)|Import \(Writer\)|Support Methods|Supported types:|
|:-------------------|:---------------|:--------------------|:----------------|:--------------|:---------------|
|Relational Databases|MySQL|Yes|Yes|Wizard/script|Alibaba Cloud/self-built|
|Relational Databases|SQL Server|Yes|Yes|Wizard/script|Alibaba Cloud/self-built|
|Relational Database|PostgreSQL|Yes|Yes|Wizard/script|Alibaba Cloud/self-built|
|Relational Databases|Oracle|Yes|Yes|Wizard/script|Self-developed|
|Relational Databases|DRDS|Yes|Yes|Wizard/script|Alibaba Cloud|
|Relational Databases-|DB2|Yes|Yes|script|Self-developed-|
|Relational Databases|DM|Yes|Yes|script|Self-developed|
|Relational Databases|RDS for PPAS|Yes|Yes|script|Alibaba Cloud|
|MPP|HybridDB for MySQL|Yes|Yes|Wizard/script|Alibaba Cloud|
|MPP|HybridDB for PostgreSQL released|Yes|Yes|Wizard/script|Alibaba Cloud|
|Big data storage|Maxcompute \(corresponding data source name ODPS\)|Yes|Yes|Wizard/script|Alibaba Cloud|
|Big data storage|DataHub|No|Yes|script|Alibaba Cloud|
|Big data storage|ElasticSearch|No|Yes|script|Alibaba Cloud|
|Big data storage|Analyticdb \(corresponding data source name: ADS\)|No|Yes|Wizard/script|Alibaba Cloud|
|Unstructured storage|OSS|Yes|Yes|Wizard/script|Alibaba Cloud|
|Unstructured storage|HDFS|Yes|Yes-|script|Self-developed|
|Unstructured storage|FTP|Yes|Yes|Wizard/script|Self-developed|
|Message Queue|LogHub|Yes|No|Wizard/script|Alibaba Cloud|
|NoSQL|HBase|Yes|Yes|script|Alibaba Cloud/self-built|
|NoSQL|MongoDB|Yes|Yes|script|Alibaba Cloud/self-built|
|NoSQL|Memcache|No|Yes|script|Alibaba Cloud/self-built memcached|
|NoSQL|Table store \(corresponding data source name: OTs\)|Yes|Yes|script|Alibaba Cloud|
|NoSQL|OpenSearch|No|Yes|script|Alibaba Cloud|
|NoSQL|Redis|No|Yes|script|Alibaba Cloud/self-built|
|Performance Testing|Stream|Yes|Yes|script|-|

