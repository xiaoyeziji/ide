# Terms {#concept_sqz_z2v_42b .concept}

## DMU {#section_vfw_cfv_42b .section}

Data Migration Unit \(DMU\) is used to measure the amount of resources consumed by data integration, including CPU, memory, and network. One DMU represents the minimum amount of resources used for a data synchronization task.

## Slot {#section_lxt_gtl_ggb .section}

By default, the resource group provides you 50 slots and each DMU occupies 2 slots. This means the default resource group supports 25 DMUs at the same time. You can submit a ticket to apply for more slots in the default resource group.

## Number of concurrencies {#section_wfw_cfv_42b .section}

Concurrency indicates the maximum number of threads used to concurrently read or write data in the data storage of a data synchronization task.

## Speed limit {#section_xfw_cfv_42b .section}

The speed limit indicates the maximum speed of synchronization tasks.

## Dirty data {#section_yfw_cfv_42b .section}

Dirty data indicates invalid or incorrectly formatted data. For example, if the source has varchar type data, but is written to a destination column as an int type data. If a data conversion exception occurs, the data cannot be written to the destination column.

## Data sources {#section_zfw_cfv_42b .section}

The data source processed by DataWorks can be from a database or a data warehouse. DataWorks supports various data source types, and supports different data source conversions.

