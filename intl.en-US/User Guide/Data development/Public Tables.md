# Public Tables {#concept_ct4_v14_q2b .concept}

In the Public Table area, you can view tables created in all projects under the current tenant.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16331/15367344538216_en-US.png)

-   Project: Project name. The prefix "odps." is added to each project name. For example, if a project name is "test", "odps.test" is displayed.
-   Table Name: Name of the table in the project.

Click a table name to view the column and partition information of the table, and preview the table data.

-   Column Information: Click it to view the field quantity, field type, and field description of the table.
-   Partition Information: Click it to view the partition information and partition quantity of the table. A maximum of 60,000 partitions are allowed. If you have set the life cycle, the actual number of partitions depends on the life cycle.
-   Data Preview: Click it to preview data in the current table.

## Environment switchover {#section_h31_1c4_q2b .section}

Similar to Table Management, Public Table supports the development and production environments. The current environment is displayed in blue. After you click an environment to be queried, the corresponding environment is displayed.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16331/15367344538217_en-US.png)

