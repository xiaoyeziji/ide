# Basic terms {#concept_sqz_z2v_42b .concept}

## DMU {#section_vfw_cfv_42b .section}

DMU is used to measure the amount of resources, including CPU, memory, and network used for data integration. One DMU represents the minimum amount of resources used for a data synchronization task.

## Number of concurrencies {#section_wfw_cfv_42b .section}

Concurrency indicates the maximum number of threads used to concurrently read data from or write data in the data storage end in a data synchronization task.

## Speed Limit {#section_xfw_cfv_42b .section}

The speed limit indicates the maximum speed for synchronization tasks.

## Dirty data {#section_yfw_cfv_42b .section}

Dirty data indicates invalid data or incorrectly formatted data. For example, if the source has varchar type data but is written to a destination column having int type data, a data conversion exception occurs and the data cannot be written to the destination column.

## Data sources {#section_zfw_cfv_42b .section}

The source of data processed by DataWorks can be a database or a data warehouse. Dataworks supports various types of data sources, and supports transformation between data sources.

