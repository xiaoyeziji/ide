# Encoding principles and standards for the SQL code {#concept_q3g_zg2_q2b .concept}

## Encoding principles {#section_a44_2h2_q2b .section}

SQL code is encoded as follows:

-   Coding principles
-   Code lines are clear, neat, and nice looking.
-   The code lines are well arranged and have a good hierarchical structure.
-   Comments should be provided whenever necessary to enhance readability of codes.
-   Requirements in this convention are not mandatory constraints for coding behaviors of developers. In practice, on the precondition that general requirements are not violated, rational deviations from this convention is acceptable if they are beneficial to code development, and this convention will be continuously improved and supplemented.
-   All keywords and reserved words used in SQL codes are in lower case. Examples of such words include select, from, where, and, or, union, insert, delete, group, having, and count.
-   In addition to keywords and reserved codes used in SQL codes, other codes \(such as field names and table alias\) must be in lower case.
-   Four spaces are equivalent to an indention unit. All indentions must be the integer multiples of an indention unit and aligned according to the code hierarchy.
-   It is prohibited to use the select \* operation. The column name must be specified in all operations.
-   The corresponding rackets must be on the same column.

## SQL coding specification {#section_yf2_xh2_q2b .section}

The code specification for SQL code is as follows:

-   Code header

    Information, such as the subject, function description, author, and date, should be added to the code header. The log and title bars should be reserved so that later users can add change records. Note that each line cannot have more than 80 characters. The following is a template:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15367335687938_en-US.png)

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

-   Field arrangement requirements
    -   For fields selected for the select statement, each field occupies one line.
    -   One indention next to the word "select" is directly followed by the first selected field. That is, the field is two indentions away from the start of the line.
    -   Each of other fields starts with two indentions, followed by a comma \(,\) and then the field name.
    -   The comma \(,\) between two fields is right before the second field.
    -   The as statement should be in the same line as the related fields. It is recommended that the "as" statements with multiple fields be aligned in the same column.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15367335688881_en-US.jpg)

-   Insert sub-statement arrangement requirements

    The Insert sub-statement should be written on the same row. Line feed is prohibited.

-   Select sub-statement arrangement requirements

    Sub-statements used by the select statements, such as from, where, group by, having, order by, join, and union, must conform to the following requirements:

    -   Line feed.
    -   The sub-statements must be left aligned with the select statement.
    -   Two indentions should be reserved between the first letter of a sub-statement and its subsequent code.
    -   The logical operators \(such as "and" and "or"\) in a "where" sub-statement must be left aligned with where.
    -   If the length of a sub-statement exceeds two indentions, add a space to the sub-statement and then write the subsequent code, such as "order by" and "group by".

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15367335688882_en-US.jpg)

-   Requirements for spacing before and after an operator One space must be reserved before and after an arithmetic operator or a logical operator. Operators must be written on the same line unless the line length exceeds 80 characters.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15367335688883_en-US.jpg)

-   Compiling of the "case" statement

    In a "select" statement, the "case" statement is used to judge or assign values to fields. Correct compiling of the "case" statement is critical for enhancing readability of code lines.

    The following conventions are stipulated for compiling of the "case" statement:

    -   The "when" sub-statement is in the same line as the "case" statement and starts after one indention.
    -   One "when" sub-statement occupies one line. Line feed is acceptable if the statement is too long.
    -   A "case" statement must contain the "else" sub-statement. The "else" sub-statement must be aligned with the "when" sub-statement.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15367335688884_en-US.jpg)

-   Nesting query compiling specification

    Nesting sub-query is often used in ETL development of the data warehouse system. Therefore, it is important to arrange codes in a hierarchical manner. Example:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15367335698885_en-US.jpg)

-   Table alias definition convention
    -   Alias must be added to all tables. Once an alias is defined for an operation table in a "select" statement, the alias must be used whenever the statement references the table. To facilitate code compiling, alias must be simple and concise whenever possible and keywords should be avoided.
    -   The table alias is defined using simple characters. It is recommended that aliases be defined in alphabetical order.
    -   Before multi-layered nesting sub-query of an alias, the hierarchy must be shown. The SQL statement alias is defined by layer. Layer 1 to layer 4 are represented by P \(Part\), S \(Segment\), U \(Unit\), and D \(Detail\), respectively. Alternatively, layer 1 to layer 4 can be represented by a, b, c, and d. Sub-statements at the same layer are differentiated from each other by the numbers \(such as 1, 2, 3, and 4\) behind the letter that represents the layer. A comment can be added for a table alias if necessary.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15367335698886_en-US.jpg)

-   SQL comments

    -   A comment must be added for each SQL statement.
    -   The comment for each SQL statement exclusively occupies one line, and is placed in front of the statement.
    -   The comment of a field comes next to the field.
    -   Comments should be added for branch condition expressions that are not easy to understand.
    -   Comments must be added for important calculations to describe their functions.
    -   If a function is too long, its statement should be segmented based on the implemented functions, and comments should be added to describe each segment.
    -   For a constant or variable, it is mandatory to add a comment to explain the meaning of the saved value, and optional to add a comment to explain the valid value range.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16308/15367335697939_en-US.png)


