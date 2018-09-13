# Table Management {#concept_qlb_gd4_q2b .concept}

## Create a table {#section_lgp_ld4_q2b .section}

1.  Click **Table Management** in the upper left corner of the page.
2.  Select the **+** icon to create a table.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16332/15368243858220_en-US.png)

3.  Enter the table name, only MaxCompute tables are supported currently, click **Submit**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16332/15368243858221_en-US.png)

4.  Set basic attributes.
    -   Chinese Name: Chinese name of the table to be created.
    -   Level-1 Topic: Name of the level-1 target folder of the table to be created.
    -   Level-2 Topic: Name of the level-2 target folder of the table to be created.
    -   Description: Description of the table to be created.
    -   Click **Create Topic**. On the displayed Topic Management page, create level-1 and level-2 topics.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15368243857965_en-US.png)

5.  Create a table in DDL mode.

    Click **DDL Mode**. In the displayed dialog box, enter the standard table creation statements.

    After editing the table creation SQL statements, click **Generate Table Structure**. Information in the Basic Attributes, Physical Model Design, and Table Structure Design areas is automatically entered.

6.  Create a table on the GUI

    If creating a table in DDL mode is not applicable, you can create the table on the GUI by performing the following settings.

    -   Physical model design
        -   Table type: It can be set to Partitioned Table or Non-partitioned Table.
        -   Life Cycle:Life cycle function of MaxCompute. Data in the table \(or partition\) that is not updated within a period specified by Life Cycle \(unit: day\) will be cleared.
        -   Level: It can be set to DW, ODS, or RPT.
        -   Physical Category: It can be set to Basic Business Layer, Advanced Business Layer, or Other. Click **Create Level**. On the displayed Level Management page, create a level.
    -   Table structure design
        -   English Field Name: English name of a field, which may contain letters, digits, and underscores \(\_\).
        -   Chinese Name: Abbreviated Chinese name of a field.
        -   Field Type: MaxCompute data type, which can only be String, Bigint, Double, Datetime, or Boolean.
        -   Description: Detailed description of a field.
        -   Primary Key: Select it to indicate the field is the primary key or a field in the joint primary key.
        -   Click **Add Field** to add a column for a new field.
        -   Click **Delete Field** to delete a created field.

            **Note:** If you delete a field from a created table and submit the table again, you must drop the current table and create one with the same name. This operation is not allowed in the production environment.

        -   Click **Move Up** to adjust the field order of the table to be created. However, to adjust the field order of a created table, you must drop the current table and create one with the same name. This operation is not allowed in the production environment.
        -   Click **Move Down**, the operation is the same as that of **Move Up**.
        -   Click **Add Partition** to create a partition for the current table. To add a partition to a created table, you must drop the current table and create one with the same name. This operation is not allowed in the production environment.
        -   Click **Delete Partition** to delete a partition. To delete a partition from a created table, you must drop the current table and create one with the same name. This operation is not allowed in the production environment.
        -   Action: You can confirm to submit a new field, delete a field, and edit more attributes.

            More properties mainly contain information related to data quality, which is provided for the system to generate validation logic. They are used in scenarios such as data profiling, SQL scan, and test rule generation.

            -   0 Allowed: If it is selected, the field value can be zero. This option is applicable only to bigint and double fields.
            -   Negative Value Allowed: If it is selected, the field value can be a negative number. This option is applicable only to bigint and double fields.
            -   Security Level: It can be set to Non-sensitive, Sensitive, or Confidential.

                ```
                C: Customer data; B: Company data; S: Business data. 
                Levels of C1—C2, B1, and S1 are non-sensitive data.
                Levels of C3, B2—B4, and S2—S3 are sensitive data. 
                Levels of C4, S4, and B4 are confidential data.
                ```

            -   Unit: Unit of the amount, which can be dollar or cent. This option is not required for fields unrelated to the amount.
            -   Lookup Table Name/Kay Value: It is applicable to enumerated value-type fields, such as the member type and status. You can enter the name of the dictionary table \(or dimension table\) corresponding to the field. For example, the name of the dictionary table corresponding to the member status is dim\_user\_status. If you use a globally unique dictionary table, enter the corresponding key\_type of the field in the dictionary table. For example, the corresponding key value of the member status is AOBAO\_USER\_STATUS.
            -   Value Range: The maximum and minimum values of the current field. It is applicable only to bigint and double fields..
            -   Regular Expression Verification: Regular expression used by the current field. For example, if a field is a mobile phone number, its value can be limited to an 11-digit number by regular expression \(or more strict limitation\).
            -   Maximum Length: Maximum number of characters of the field value. It is applicable only to string fields.
            -   Date Precision: Precision of the date, which can be set to Hour, Day, Month, or others. For example, the precision of month\_id in the monthly summary table is Month, although the field value is 2014-08-01 \(it seems that the precision is Day\). It is applicable to date values of the Datetime or String type.
            -   Date Format: It is applicable only to date values of the string type. The format of the date value actually stored in the field is similar to yyyy-mm-dd hh:mm:ss.
            -    KV Primary Separator/Secondary Separator: It is applicable to a large field \(of the string type\) combined by KV pairs. For example, if a product expansion attribute has a value similar to "key1:value1;key2:value2;key3:value3;...", the semicolon \(;\) is the primary separator of the field that separates the KV pairs, and the colon \(:\) is the secondary separator that separates the key and value in a KV pair.
    -   Partition Field Design: This option is displayed only when Partition Type in the Physical Model Design area is set to Partitioned Table.
    -   Field Type: We recommend that you use the string type for all fields.
    -   Date Partition Format: If a partition field is a date \(although its data type may be string\), select or enter a date format, such as yyyymmmdd.
    -   Date Partition Granularity: For example, Day, Month, or Hour. Configure the partition granularity as per your needs. By default, if multiple partition granularities are required, the greater the granularity is, the higher the partition level is. For example, if three partitions \(hour, day, and month\) exist, the relationship among the multiple partitions is: level-1 partition \(month\), level-2 partition \(day\), and level-3 partition \(hour\).

## Submit a table {#section_d52_21l_q2b .section}

After editing the table structure information, submit the new table to the development environment and production environment.

-   Click **Load from Development Environment**. If the table has been submitted to the development environment, this button is highlighted. After you click the button, the information of the created table in the development environment overwrites the information on the current page.
-   Click **Submit to Development Environment**. The system checks whether all required items on the current editing page are completely set. If any omission exists, an alarm is reported, forbidding you to submit the table.
-   Click **Load from Production Environment**. The detailed information of the table submitted to the production environment overwrites the information on the current page.
-   Click **Create in Production Environment**. The table is created in the project of the production environment.

## Query tables by type {#section_hzs_wn4_q2b .section}

On the Table Management page, you can select Development Environment or Production Environment to query tables. The query results are sorted by folder of topics.

-   If you select Development Environment, you can only query tables in the development environment.
-   If you select Production Environment, you can query tables in the production environment. Be cautious when operating the tables in the production environment.

