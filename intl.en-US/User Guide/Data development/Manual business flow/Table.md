# Table {#concept_vk4_stk_q2b .concept}

## Create a table {#section_x2y_35k_q2b .section}

1.  Click **Manual Business Flow**, and select **Create Business Flow**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15525334987961_en-US.png)

2.  Right-click **Table**, and select **Create Table**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15525334987962_en-US.png)

3.  Set basic attributes.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15525334987963_en-US.png)

    -   Chinese Name: The Chinese name of the created table.
    -   Level-1 Topic: The name of the level-1 target folder of the created table.
    -   Level-2 Topic: The name of the level-2 target folder of the created table.
    -   Description: The description of the created table.
    -   Click **Create Topic**. On the displayed Topic Management page, create level-1 and level-2 topics.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15525334987965_en-US.png)

4.  Create a table in DDL mode

    Click **DDL Mode**. In the displayed dialog box, enter the standard table creation statements.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16319/15525334987966_en-US.png)

    After editing the table creation SQL statements, click **Generate Table Structure** to automatically enter information in the Basic Attributes, Physical Model Design, and Table Structure Design areas.

5.  Create a table on the GUI

    If creating a table in DDL mode is not applicable, you can create the table on the GUI by performing the following settings.

    -   Physical model design
        -   Partition Type: It can be set to Partitioned Table or Non-partitioned Table.
        -   Life Cycle: The life cycle function of MaxCompute. Data in the table \(or partition\) that is not updated within a period specified by the Life Cycle \(unit: day\) will be cleared.
        -   Level: It can be set to DW, ODS, or RPT.
        -   Physical Category: It can be set to Basic Business Layer, Advanced Business Layer, or Other. Click **Create Level**. On the displayed Level Management page, create level.
    -   Table structure design
        -   English Field Name: The English name of a field can contain letters, numbers, and underscores \(\_\).
        -   Chinese Name: The abbreviated Chinese name of a field.
        -   Field Type: The MaxCompute data type, which can only be String, Bigint, Double, Datetime, or Boolean.
        -   Description: The detailed description of a field.
        -   Primary Key: Select this parameter to indicate the field is the primary key or a field in the joint primary key.
        -   Click **Add Field** to add a column for a new field.
        -   Click **Delete Field** to delete a created field.

            **Note:** If you delete a field from the created table and submit the table again, you must drop the current table and create one with the same name. This operation is not allowed in the production environment.

        -   Click **Move Up** to adjust the field order of the created table. However, to adjust the field order of a created table, you must drop the current table and create one with the same name. This operation is not allowed in the production environment.
        -   Click **Move Down**, so the operation is the same as that of **Move Up**.
        -   Click **Add Partition** to create a partition for the current table. To add a partition to the created table, you must drop the current table and create one with the same name. This operation is not allowed in the production environment.
        -   Click **Delete Partition** to delete a partition. To delete a partition from a created table, you must drop the current table and create one with the same name. This operation is not allowed in the production environment.
        -   Action: You can confirm to submit a new field, delete a field, and edit more attributes.

            More attributes include information related to the data quality, which is provided for the system to generate the verification logic. They are used in scenarios, such as data profiling, SQL scan, and test rule generation.

            -   0 Allowed: If it is selected, the field value can be zero. This option is applicable only to Bigint and Double fields.
            -   Negative value allowed: If it is selected, the field value can be a negative number. This option is applicable only to Bigint and Double fields.
            -   Security level: It can be set to Non-sensitive, Sensitive, or Confidential.

                ```
                C: Customer data, B: Company data, S: Business data。 
                C1—C2, B1, and S1 are non-sensitive data.
                C3, B2–B4, S2, and S3 are sensitive data. 
                C4, S4, and B4 are confidential data. 
                ```

            -   Unit: The amount unit, which can be in dollars or cents. This option is not required for fields unrelated to the amount.
            -   Lookup table name/key value: It is applicable to enumerated value-type fields, such as the member type and status. You can enter the name of the dictionary table \(or dimension table\) corresponding to the field. For example, the name of the dictionary table corresponding to the member status is dim\_user\_status. If you use a globally unique dictionary table, enter the corresponding key\_type of the field in the dictionary table. For example, the corresponding key value of the member status is TAOBAO\_USER\_STATUS.
            -   Value range: The maximum and minimum values of the current field. It is applicable only to Bigint and Double fields.
            -   Regular expression verification: The regular expression used by the current field. For example, if a field is a mobile phone number, the value can be limited to an 11-digit number through regular expression \(or more strict limitations\).
            -   Maximum length: The maximum number of characters of the field value. It is applicable only to string fields.
            -   Date precision: The precision of the date, which can be set to Hour, Day, Month, or others. For example, the precision of month\_id in the monthly summary table is Month, although, the field value is 2014-08-01 \(it seems that the precision is Day\). It is applicable to date values of the datetime or string type.
            -   Date format: The format is applicable only to the date values of the string type. The format of the date value stored in the field is similar to yyyy-mm-dd hh:mm:ss.
            -   KV primary separator/secondary separator: It is applicable to a large field \(of the string type\) combined with KV pairs. For example, if a product expansion attribute has a value similar to "key1:value1;key2:value2;key3:value3;...", the semicolon \(;\) is the primary separator of the field that separates the KV pairs, and the colon \(:\) is the secondary separator that separates the key and value in a KV pair.
    -   Partition field design: This option is displayed only when the Partition Type in the Physical Model Design area is set to Partitioned Table.
    -   Field type: We recommend that you use the string type for all fields.
    -   Date partition format: If a partition field is a date \(although its data type may be string\), and select or enter a date format, such as yyyymmmdd.
    -   Date partition granularity: For example, Day, Month, or Hour.

## Submit a table {#section_d52_21l_q2b .section}

After editing the table structure information, submit the new table to the development environment and production environment.

-   Click **Load from Development Environment**. If the table has been submitted to the development environment, this button is highlighted. After you click the button, the information of the created table in the development environment overwrites the information on the current page.
-   Click **Submit to Development Environment**, the system checks whether all required items on the current editing page are completely set. If any omission exists, an alarm is reported to prevent you from submitting the table.
-   Click **Load from Production Environment**, to submit the detailed information of the table to the production environment. Information on the current page will be overwritten.
-   Click **Create in Production Environment**, to create the table in the project of the production environment.

