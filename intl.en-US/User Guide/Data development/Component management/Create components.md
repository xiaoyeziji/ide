# Create components {#concept_ex5_2df_q2b .concept}

## Definition of components {#section_lv2_xdf_q2b .section}

A component is an SQL code process template containing multiple input and output parameters. To handle an SQL code process, one or more source data tables are imported, filtered, joined, and aggregated to form a target table required for new business.

## Value of components {#section_hgh_b2f_q2b .section}

In actual businesses, many SQL code processes are similar. The input and output tables in a process have the same or compatible structures but different names. In this case, component developers can abstract such SQL process to an SQL component node, and variable input and output tables in the SQL process to input and output parameters to reuse the SQL code.

When using SQL component nodes, component users only need to select components like their own business flows from the component list, configure specific input and output tables in their own businesses for these components, and generate new SQL component nodes without repeatedly copying the code. This greatly improves the development efficiency and avoids repeated development. Publishing and scheduling of the SQL component nodes after generation is the same as those of common SQL nodes.

## Composition of components {#section_ery_g3f_q2b .section}

Like a function definition, a component consists of the input parameters, output parameters, and component code processes.

## Component input parameters {#section_b5d_33f_q2b .section}

A component input parameter contains the attributes such as the name, type, description, and definition. The parameter type can be table or string.

-   A table-type parameter specifies tables to be referenced in a component process. When using a component, the component user can set the parameter to the table required for the specific business.
-   A string-type parameter specifies variable control parameters in a component process. For example, if a result table of a specific process only outputs the sales amount of top N cities in each region, the value of N can be specified by the string-type parameter.

    If a result table of a specific process needs to output the total sales amount of a province, a province string-type parameter can be set to specify different provinces and obtain the sales amount of the specified province.

-   Parameter description specifies the role of a parameter in a component process.
-   Parameter definition is a text definition of the table structure, which is required only for table-type parameters. When this attribute is specified, the component user must provide an input table that is compatible with the field names and types defined by the table parameter so that the component process can run properly. Otherwise, an error is reported when the component process runs because the specified field in the input table cannot be found. The input table must contain the field names and types defined by the table parameter. The fields and types can be in different orders, and the input table can also contain other fields.  The definition is for reference only. It provides guidance for users and does not need to be immediately and forcibly checked.
-   The recommended definition format of the table parameter is as follows:

    ```
    Field 1 name Field 1 type Field 1 comment 
    Field 2 name Field 2 type Field 2 comment 
    Field n name Field n type Field n comment
    ```

    Example:

    ```
    area_id string ‘Region ID’ 
    city_id string ‘City ID’ 
    order_amt double ‘Order amount’
    ```


## Component output parameters {#section_z45_rjf_q2b .section}

-   A component output parameter contains the attributes such as the name, type, description, and definition. The parameter type can only be table. A string-type output parameter does not have the logical meaning.
-   A table-type parameter: specifies tables to be generated from a component process. When using a component, the component user can set the parameter to the result table that the component process generates for the specific business.
-   Parameter description: specifies the role of a parameter in a component process.
-   Parameter definition: it is a text definition of the table structure. When this attribute is specified, the component user must provide the parameter with an output table that has the same number of fields and compatible type as defined by the table parameter so that the component process can run properly. Otherwise, an error is reported when the component process runs because the number of fields does not match or the type is incompatible. The field names of the output table do not need to be consistent with those defined by the table parameter. The definition is for reference only. It provides guidance for users and does not need to be immediately and forcibly checked.
-   The recommended definition format of the table parameter is as follows:

    ```
    Field 1 name Field 1 type Field 1 comment 
    Field 2 name Field 2 type Field 2 comment 
    Field n name Field n type Field n comment
    ```

    Example:

    ```
    area_id string ‘Region ID’ 
    city_id string ‘City ID’ 
    order_amt double ‘Order amount’ 
    rank bigint ‘Rank’ 
    ```


## Component process bodies {#section_pnp_xkf_q2b .section}

The reference format of the parameters in a process body is as follows: @@\{parameter name\}

By compiling an abstract SQL working process, the process body controls the specified input tables based on the input parameters and generates output tables with business value.

Certain skills are required for the development of a component process. Input parameters and output parameters must be well used for the component process code so that different values of input parameters and output parameters can generate correct and runnable SQL code.

## Example of creating a component {#section_ayq_zkf_q2b .section}

You can create a component as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16311/15427069127944_en-US.png)

## Source table schema definition {#section_plv_mnf_q2b .section}

The source MySQL schema definition of the sales data is described in the following table:

|Field Name|Field type|Field description|
|:---------|:---------|:----------------|
|order\_id|varchar|Order ID|
|report\_date|datetime|Order date|
|customer\_name|varchar|Customer Name|
|order\_level|varchar|Order grade|
|order\_number|double|Order quantity|
|order\_amt|double|Order amount|
|back\_point|double|Discount|
|shipping\_type|varchar|Transportation mode|
|profit\_amt|double|Profit amount|
|price|double|Unit price|
|shipping\_cost|double |Transportation cost|
|area|varchar|Region|
|province|varchar|Province|
|city|varchar|City|
|product\_type|varchar|Product Type|
|product\_sub\_type|varchar|Product subtype|
|product\_name|varchar|Product Name|
|product\_box|varchar|Product packing box|
|shipping\_date|Datetime|Transportation date|

## Business implication of components {#section_fzc_t4f_q2b .section}

Component name: get\_top\_n

Component description:

In the component process, the specified sales data table is used as the input parameter \(table type\), the number of the top cities is used as the input parameter \(string type\), and the cities are ranked by sales amount. In this way, the component user can easily obtain the rank of the specified top N cities in each region.

## Definition of component parameters {#section_bc2_drf_q2b .section}

Input parameter 1:

Parameter name: myinputtable type: table

Input parameter 2:

Parameter name: topn type: string

Input parameter 3:

Parameter name: myoutput type: table

Parameter definition:

area\_id string

city\_id string

order\_amt double

rank bigint

Table creation statement: 

```
CREATE TABLE IF NOT EXISTS company_sales_top_n
( 
area STRING COMMENT 'Region', 
city STRING COMMENT 'City', 
sales_amount DOUBLE COMMENT 'Sales amount', 
rank BIGINT COMMENT 'Rank'
)
COMMENT 'Company sales ranking'
PARTITIONED BY (pt STRING COMMENT '')
LIFECYCLE 365;
```

## Definition of component process bodies {#section_ihl_hsf_q2b .section}

```
INSERT OVERWRITE TABLE @@{myoutput} PARTITION (pt='${bizdate}')
    SELECT r3.area_id,
    r3.city_id,
    r3.order_amt,
    r3.rank
from (
SELECT
    area_id,
    city_id,
    rank,
    order_amt_1505468133993_sum as order_amt ,
    order_number_1505468133991_sum,
    profit_amt_1505468134000_sum
FROM
    (SELECT
    area_id,
    city_id,
    ROW_NUMBER() OVER (PARTITION BY r1.area_id ORDER BY r1.order_amt_1505468133993_sum DESC) 
AS rank,
    order_amt_1505468133993_sum, 
    order_number_1505468133991_sum,
    profit_amt_1505468134000_sum
FROM     
    (SELECT area AS area_id,
     city AS city_id,
     SUM(order_amt) AS order_amt_1505468133993_sum,
     SUM(order_number) AS order_number_1505468133991_sum,
     SUM(profit_amt) AS profit_amt_1505468134000_sum
FROM
    @@{myinputtable}
WHERE
    SUBSTR(pt, 1, 8) IN ( '${bizdate}' )
GROUP BY 
    area,
    city )
    r1 ) r2
WHERE
    r2.rank >= 1 AND r2.rank <= @@{topn}
ORDER BY
    area_id, 
    rank limit 10000) r3;
```

## Sharing scope of components {#section_etw_pxf_q2b .section}

There are two sharing scopes: project component and public component.

After a component is published, it is visible to users within the project by default. The component developer can click the Publish Component icon to publish a universal global component to the entire tenant, allowing all users in the tenant to view and use the public component. Whether a component is public depends on whether the icon in the following figure is visible:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16311/15427069127945_en-US.png)

## Use of components {#section_iz3_vxf_q2b .section}

How can users use a developed component? For more information, see [Use components](reseller.en-US/User Guide/Data development/Component management/Use components.md#)

## Reference records of components {#section_d4g_wxf_q2b .section}

The component developer can click the **Reference Records** tab to view the reference record of a component.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16311/15427069127946_en-US.png)

