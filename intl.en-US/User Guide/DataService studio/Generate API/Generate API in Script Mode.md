# Generate API in Script Mode {#concept_gpn_xc3_r2b .concept}

This article introduces you to the steps that script mode can take to generate the API.

To meet the needs of high-end users for personalized queries, the Data Service also provides a script pattern for customizing SQL, allows you to write your own SQL queries for the API, multi-Table Association, complex query conditions and Aggregate functions are supported.

## Configure the API basic information {#section_qdt_jzh_r2b .section}

1.  Navigate to the **API Service list** \> **Generate API**.
2.  Click **Script Mode** to fill in the API basics.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16407/15390829418791_en-US.png)

    Note the settings for the API grouping during configuration. An API group includes a collection of APIs that are used for a specific scenario. It is the minimum management unit in API Gateway. In the Alibaba Cloud API Marketplace, each API group corresponds to a specific API product.

    **Note:** The set up example for API grouping is as follows:

    For example, you would like to configure an API product for weather inquiry, weather search API by city name weather search API, scenic spot name search weather API and zip search weather API three kinds of APIS, then you can create an API group called a weather query, and put the above three APIs in this group. The API is shown as a weather query product when published to the marketplace.

    Of course, if your generated API is used in your own app, you can use grouping as a classification.

    Currently, the build API only supports HTTP protocol, GET request mode, and JSON return type.

3.  After providing the API basic information, click **Next** to go to the API parameter configuration page.

## Configure the API Parameters {#section_c1n_g13_r2b .section}

1.  Select the data source and table.

    Navigate to the **data source type** \> **data source name** \> **data table**, click the appropriate table name in the data table list, you can view the field information for this table.

    **Note:** 

    -   You need to configure the data source in advance in the data set formation.
    -   You must select a data source. Table join queries across data sources are not supported.
2.  Write SQL queries for the API.

    You can enter the SQL code in the code box on the right side. The system supports one-click SQL function, checking fields in the list of fields, and clicking **Generate SQL**, the SQL statement for `SELECT xxx FROM xxx` is automatically generated and inserted at the right cursor.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16408/15390829418802_en-US.png)

    **Note:** 

    -   One-click SQL addition is especially useful when the number of fields is relatively large, which can greatly improve the efficiency of SQL writing.
    -   The field of the SELECT query is the return parameter of the API, the parameter at the where condition is the request parameter for the API, And the request parameter is identified with $.
3.  Finally, edit and complete parameter information.

    After writing the API query SQL, click the **parameters** in the upper-right corner to switch to the parameter information Edit page, you can edit the type, sample values, default values, and descriptions of the parameters here, where Type and description are required.

    **Note:** To help the caller of the API get a more comprehensive understanding of the API, please complete the API parameter information as much as possible.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16408/15390829418803_en-US.png)


You need to pay attention to the settings that return result paging during the configuration process.

-   If you do not enable the **response pagination**, the API outputs up to 500 records by default.
-   If the return result may exceed 500, turn on the **response pagination** function.

The following public parameters are available only when the response pagination feature is enabled:

-   Common request parameters
    -   pageNum: the current page number.
    -   Pagesize: The page size, that is, the number of records per page.
-   Common response parameters
    -   pageNum: the current page number.
    -   pageSize: The page size, that is, the number of records per page.
    -   totalNum: the total number of records.

**Note:** SQL rule prompt.

-   Only one SQL statement is supported, and multiple SQL statements are not supported.
-   Only the \`SELECT\` clause is supported. Other clauses such as \`INSERT\`, \`UPDATE\`, and \`DELETE\` are not supported.
-   The query field for select is the return parameter for the API, the variable Param in the $ \{Param\} in the where condition is a request parameter for the API.
-    `SELECT \*` is not supported, columns of the query must be specified explicitly.
-   Single table queries, table join queries, and nested queries within one data source are supported.
-   If the column name of the SELECT query column has a table name prefix \(such as T. name\), the alias must be taken as the return parameter name \(such as T. name as name \).
-   If you use the aggregate function \(min/max/sum/count, etc \), the alias must be taken as the return parameter name \(such as sum \(Num\) as total \\ \_ num \).
-   In SQL, $ \{Param\} is uniform when the request parameter is replaced, contains $ \{Param\} in the string \}. When $ \{Param\} has an escape character \\, it does not do request parameter processing, processed as an ordinary string.
-   Putting $ \{Param\} in quotation marks is not supported, such as '$ \{ID\}', 'ABC $ \{xyz\} 123 ', concat \('abc ', $ \{xyz\}, '123'\) can be passed if necessary '\) implementation.

When the configuration of the API parameters is complete, click **Next** to enter the API testing section.

## API Testing {#section_d5j_nb3_r2b .section}

After completing configuration of API parameters, you can start the API test.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16407/15390829418797_en-US.png)

Set parameters and click **Start Test** to send the API request online. The API request details and response are displayed on the right. If the test fails, read the error message carefully and make the appropriate adjustments to test your API again.

You need to note the settings for the normal return example during the configuration process. When testing an API, the system automatically generates exception examples and error codes. However, normal response examples are not automatically generated. After the test succeeds, you need to click **Save as Normal Response Sample** to save the current test result as the normal response sample. If sensitive data is included in the response, you can manually edit it.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16407/15390829418799_en-US.png)

**Note:** 

-   Normal response examples provide an important reference value for the API callers. Specify an example if possible.
-   The API calling delay is the delay of the current API request, which is used to evaluate the API performance. If the latency is too high, you may consider optimizing your database.

After completing the API test, click **Finish**. The data API is successfully created.

## API details viewing {#section_xp4_mc3_r2b .section}

Back on the API service list page, click **details** in the Action column to view the details of the API. This page displays detailed information about an API from the view of a caller.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16407/15390829418800_en-US.png)

