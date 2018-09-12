# Synchronous table column name is a key and task fails {#concept_bnt_kkb_r2b .concept}

## Issue Description {#section_bhk_mhb_r2b .section}

When you perform a synchronization task, the task fails as the column name of the synchronized table is a keyword.

## Solution {#section_v3x_shb_r2b .section}

Take MySQL data source as an example:

1.  Create a new table aliyun, and the table creation statement is as follows:

    ```
    create table aliyun (`table` int ,msg varchar(10));
    ```

2.  Creates a view, giving the table column an alias.

    ```
    create view v_aliyun as select `table` as col1,msg as col2 from aliyun;
    ```

    **Note:** 

    -   Table is the MySQL keyword, And the mosaic code will be reported wrong when the data is synchronized. So bypass this restriction by creating a view and assigning an alias to the table column.
    -   Keywords are not recommended as column names for tables.
3.  The above statement gives an alias for a column that has a keyword, so when you configure a Data Synchronization task, you can choose the maid view instead of the aliyun table.

**Note:** 

-   The Escape Character for MySQL is 'key '.
-   The escape characters for Oracle and PostgreSQL are "keywords ".
-   The Escape Character for SQL Server is the \[Key\].

