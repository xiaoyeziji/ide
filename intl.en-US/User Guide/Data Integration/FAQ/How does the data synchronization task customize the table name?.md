# How does the data synchronization task customize the table name? {#concept_dyc_kmb_r2b .concept}

## Data backdrop {#section_tqt_lmb_r2b .section}

Data Background: The tables are identified by days \(such as orders\_20170310, orders\_20170311, and orders\_20170312\) on a one-table-for-one-day basis with the same table structure.

## Achieving demand {#section_nlz_xmb_r2b .section}

Requirement: Create only one data synchronization task to import the table data of the previous day read from the source database into MaxCompute with a custom table name every morning \(for example, on March 15, 2017, orders\_20170314 table data is read automatically from the source database and imported, and so on\).

## Implementation {#section_uml_fnb_r2b .section}

1.  Log in to the dataworks console and navigate to the **data integration** page.
2.  Create a Data Synchronization task in wizard mode, and select a table name as the name for the data source table when you configure it. Configure and save the synchronization task following the normal procedure.
3.  Click **convert script** to convert the wizard mode to script mode.
4.  Use a variable as the name of the source table in the script mode, such as orders\_$\{tablename\}.

    Assign the variable "tablename" a value in parameter settings of the task. Since the table names in "Data Background" are identified by days, which requires reading the table of the previous day, the assigned value is $yyyymmdd-1.

    **Note:** Or you can use orders\_$\{bdp.system.bizdate\} as the variable to name the source table.


After completing the configuration above, save and submit before following up.

