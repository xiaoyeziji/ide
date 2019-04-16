# SQL code encoding principles and standards {#concept_q3g_zg2_q2b .concept}

**Short Description:** This topic describes the basic SQL code encoding principles and standards.

## Encoding principles {#section_a44_2h2_q2b .section}

The SQL code is encoded as follows:

-   The code is comprehensive and healthy.
-   The code lines are clear, neat, and orderly.
-   The code lines are well arranged and have a good hierarchical structure.
-   The comments must be provided to improve the code's readability.
-   The principle requires no constraint conventions for developers coding behavior. In practice, the general requirement preconditions are not violated, rational deviations from this convention are acceptable. If they are beneficial to code development then this convention can be continuously improved and supplemented.
-   All keywords and reserved words used in SQL codes are in lowercase, such as the following: Select, From, Where, And, Or, Union, Insert, Delete, Group, Having, and Count.
-   Keywords and reserved codes used in SQL codes, and other codes including field names and table alias must be in lowercase.
-   Four spaces are equivalent to an indention unit. All indentions must be the integer multiples of an indention unit and aligned according to the code hierarchy.
-   You are not allowed to use the select asterisk \(\*\) operation. The column name must be specified in all operations.
-   The corresponding brackets must be on the same column.

## SQL coding specification {#section_yf2_xh2_q2b .section}

The SQL code specification is as follows:

-   Code header

    The code header must have the following information such as: subject, function description, author, and date. The log and title bars must be reserved so that other users can edit records. Note that each line must not exceed 80 characters in length. The following is a template:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15519375557938_en-US.png)

    ```
    -- MaxCompute(ODPS) SQL
    --**************************************************************************
    -- Subject: Transaction
    -- Function description: Transaction refund analysis 
     -- Author: With code
     -- Create time: 20170616 
     -- Change log:
     -- Modified on  Modified by  Content
    -- yyyymmdd name comment 
    -- 20170831 Without code Add a judgment on the transaction biz_type=1234 
    --**************************************************************************
    ```

-   Field arrangement requirements:
    -   Each selected field for the SELECT statement occupies one line.
    -   One indention next to the word "select" is followed by the first selected field. That is, the field is two indentions away from the line start .
    -   Each alternating field starts with two indentions, followed by a comma \(,\) and then the field name.
    -   The comma \(,\) between two fields come before the second field.
    -   The as statement must be in the same line as the related fields. We recommend that the "as" statements with multiple fields must be aligned in the same column.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15519375558881_en-US.jpg)

-   INSERT sub-statement arrangement requirements

    The INSERT sub-statement must be written in the same row. You are not allowed to use the line feed.

-   SELECT sub-statement arrangement requirements

    Sub-statements used by the SELECT statements, include From, Where, Group by, Having, Order by, Join, and Union, must conform to the following requirements:

    -   The line feed.
    -   The sub-statements must be left-aligned with the SELECT statement.
    -   You must reserve two indentions between the first letter of a sub-statement and its subsequent code.
    -   The logical operators, such as "AND" and "OR" in a "WHERE" sub-statement must be left-aligned with WHERE.
    -   If the length of a sub-statement exceeds two indentions, add a space to the sub-statement, and write the subsequent code. For example: "order by" and "group by"

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15519375558882_en-US.jpg)

-   The spacing requirements before and after an operator as follows: A space must be reserved before and after an arithmetic operator or a logical operator, and operators must be written on the same line unless the line exceeds 80 characters in length.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15519375558883_en-US.jpg)

-   Compiling the "CASE" statement

    In a "SELECT" statement, the "CASE" statement is used to judge or assign field values. The correct compiling of the "CASE" statement is critical for enhancing the code lines readability.

    The following conventions are stipulated for compiling the "CASE" statement:

    -   The "WHEN" sub statement is in the same line as the "CASE" statement and starts after one indention.
    -   Each "WHEN" sub statement occupies one line. The line feed is acceptable if the statement is too long.
    -   A "CASE" statement must contain the "ELSE" sub statement. The "ELSE" sub statement must be aligned with the "WHEN" sub statement.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15519375558884_en-US.jpg)

-   Nesting query compiling specification

    The nesting sub-query is often used in Extract, transform, load \(ETL\) development of the data warehouse system. Therefore, it is important to arrange codes in a hierarchical manner. For example:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15519375558885_en-US.jpg)

-   Table alias definition convention
    -   The alias must be added to all tables. Once an alias is defined for an operation table in a "SELECT" statement, the alias must be used whenever there are table statement references. To facilitate the code compiling, the alias must be simple and concise whenever possible and keywords must be avoided.
    -   The table alias is defined with simple characters. We recommend that aliases are defined in alphabetical order.
    -   The hierarchy must be shown before using the multi-layered nesting sub-query of an alias. The SQL statement alias is defined by the layer. Layer 1 to 4 are represented by P \(Part\), S \(Segment\), U \(Unit\), and D \(Detail\), respectively. Alternatively, Layer 1 to 4 can be represented by a, b, c, and d. Sub-statements in the same layer are differentiated from each other by numbers, such as 1, 2, 3, and 4 behind the letter that represents the layer. A comment can be added for a table alias.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15519375558886_en-US.jpg)

-   Comments within the SQL statement

    -   The comment must be added for each SQL statement.
    -   The comment for each SQL statement exclusively occupies a single line, and is placed in front of the statement.
    -   The field comment must be added behind the field.
    -   Comments must be added to branch condition expressions that are difficult to understand.
    -   Comments must be added to describe important calculation functions.
    -   If a function is too long, the statement must be segmented based on the implemented functions, and comments must be added to describe each segment.
    -   Comments must be added to a constant or variable to explain the saved value, but comments are optional for a valid value range.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15519375557939_en-US.png)


