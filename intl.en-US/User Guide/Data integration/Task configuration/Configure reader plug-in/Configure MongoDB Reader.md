# Configure MongoDB Reader {#concept_lsz_f2p_p2b .concept}

The MongoDB Reader plug-in uses Mongo Client, the Java client of MongoDB, to read data from MongoDB. In the latest version of Mongo, the granularity of the DB lock has been reduced from the DB level to the document level. Combined with the powerful indexing function of MongoDB, it allows a high-performance reading of MongoDB.

**Note:** 

-   If you are using ApsaraDB for MongoDB, a root account is provided by default. To ensure security, Data Integration only supports using the relevant account of MongoDB for connection. Avoid using the root account as access account when adding and using the MongoDB data source.
-   Query does not support the JS syntax.

MongoDB Reader reads data in parallel from MongoDB by means of Data Integration framework. Based on the specified rules, it partitions the data in MongoDB into multiple data fragments, reads them in parallel using the controlling Job program based on the specified rules, and then converts the data types supported by MongoDB to the ones supported by Data Integration individually.

## Type conversion list {#section_m2v_hxm_q2b .section}

MongoDB Reader supports most data types in MongoDB. Check whether your data type is supported before using it.

MongoDB Writer converts the MongoDB data types as follows:

|Type classification|MongoDB data type |
|:------------------|:-----------------|
|Integer|int and long|
|Floating point|double|
|String|string|
|Date and time|date|
|Boolean|bool|
|Binary|bytes|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description| Required|Default value|
|:--------|:----------|:--------|:------------|
|datasource|The data source name. It must be identical to the data source name added. Adding data source is supported in script mode.|Yes|N/A-|
|collectionName|The collection name of MongoDB.|Yes|N/A|
|column|An array of multiple column names of a document in MongoDB.-   name: Column name.
-   type: Column type.
-   splitter: MongoDB supports array, but the CDP framework does not. Therefore, the data items read from MongoDB in an array format are joined into a string using this delimiter.

|Yes|N/A|
|query|Used to define the range of returned MongoDB data. For example, if you set it to`"query":"{'operationTime':{'$gte':ISODate('${last_day}T00:00:00.424+0800')}}"`, only the data with an operationTime later than or equal to 00:00 of $\{last\_day\} is returned. $\{last\_day\} is DataWorks scheduling parameter of in the format of $\[yyyy-mm-dd\]. You can use conditional operators \($gt, $lt, $gte, $lte\), logical operators \(and, or\), and functions \(max, min, sum, avg, ISODat\) supported by MongoDB as needed. For details, see the query syntax of MongoDB.|No|N/A|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Currently, development in wizard mode is unavailable.

## Development in script mode {#section_cp2_wsh_p2b .section}

To configure a job to extract data locally from MongoDB, please refer to the above parameter descriptions for details.

```
{
    "type":"job",
    "version":"2.0",
    "steps":[
            "reader": {
            "plugin": "mongodb", 
            "parameter": {
                "datasource": "datasourceName", 
                "collectionName": "tag_data",
                "query":"",
                "column": [
                         {
                              "name": "unique_id", 
                              "type": "string" 
                          },
                          {
                              "name": "sid",
                              "type": "string"
                          },
                          {
                              "name": "user_id",
                              "type": "string"
                          },
                          {
                              "name": "auction_id",
                              "type": "string"
                          },
                          {
                              "name": "content_type",
                              "type": "string"
                          },
                          {
                              "name": "pool_type",
                              "type": "string"
                          },
                          {
                              "name": "frontcat_id",
                              "type": "Array",
                              "spliter": ""
                          },
                          {
                              "name": "categoryid",
                              "type": "Array",
                              "spliter": ""
                          },
                          {
                              "name": "gmt_create",
                              "type": "string"
                          },
                          {
                              "name": "taglist",
                              "type": "Array",
                              "spliter": " "
                          },
                          {
                              "name": "property",
                              "type": "string"
                          },
                          {
                              "name": "scorea",
                              "type": "int"
                          },
                          {
                              "name": "scoreb",
                              "type": "int"
                          },
                          {
                              "name": "scorec",
                              "type": "int"
                          },
			 {
                            "name": "a.b",
                            "type": "document.int"
                          },
                          {
                            "name": "a.b.c",
                            "type": "document.array",
                            "splitter": " "
                          }
                ]
            }
        },
        { 
            "stepType":"stream",
            "parameter":{},
            "name":"Writer",
            "category":"writer"
        }
    ],
    "setting":{
        "errorLimit":{
            "record":"0"
        },
        "speed":{
            "throttle":false,
            "concurrent":1,
            "dmu":1
        }
    },
    "order":{
        "hops":[
            {
                "from":"Reader",
                "to":"Writer"
            }
        ]
    }
}
```

