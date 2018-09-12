# RDS synchronization failure converted to JDBC format {#concept_zgb_33b_r2b .concept}

## Issue Description {#section_bhk_mhb_r2b .section}

When synchronizing data from RDS \(MySQL/SQL Server/PostgreSQL\) to user-created MySQL/SQL Server/PostgreSQL, the error message "DataX cannot connect to the corresponding database" appears.

## Solution {#section_v3x_shb_r2b .section}

Taking data synchronization from RDS \(MySQL\) to user-created SQL Server as an example, you must complete the following operations:

1.  1. Create a data source, and configure the data source as MySQL-\>JDBC format;
2.  Use the new data source to configure synchronization tasks and re-execute them.

    **Note:** Note: For data synchronization between RDS \(MySQL\) -\> RDS \(SQL Server\) and other cloud products, we recommend that you select RDS \(MySQL\) -\> RDS \(SQL Server\) data source to configure synchronization tasks.


