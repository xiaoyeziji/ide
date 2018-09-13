# Full-database Migration Data Type {#concept_yg5_spb_r2b .concept}

Currently, full-database migration only supports synchronizing data from MySQL databases \(including MySQL databases on the RDS server\) to MaxCompute. You can enter the full-database migration page from the added MySQL data source

The following is a description of the data types that are set at the advanced level in the whole library migration.

![](images/8684_en-US.jpg)

The data source types supported by MySQL for the whole library migration source include tinyint, smallint, mediumint, Int, bigint, varchar, Char, tinytext, text, mediumtext, longtext, year, float, double, decimal, date, datetime, timestamp, time, and LOL.

The data source types supported by the target-side MaxCompute are bigint, String, double, datetime, and Boolean.

All those preceding MySQL-supported data types support converting to MaxCompute data source types.

**Note:** Bit in MySQL, if it is more than bit \(2\), conversion with bigint, String, double, datetime, and Boolean is currently not supported. If it is bit \(1\), it is converted to a Boolean.

