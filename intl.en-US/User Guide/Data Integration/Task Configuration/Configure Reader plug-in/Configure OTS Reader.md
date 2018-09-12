# Configure OTS Reader {#concept_jkn_jhq_p2b .concept}

The OTS Reader plug-in provides the ability to read data from OTS, which allows incremental data extraction within the specified data extraction range. Currently, the following three extraction methods are supported:

-   Full table extraction
-   Specified range extraction
-   Specified partition extraction

OTS is a NoSQL database service built upon Alibaba Cloud's Apsara distributed system, enabling you to store and access massive structured data in real time. OTS organizes data into instances and tables. Using data partition and server load balancing technology, it provides seamless scaling.

In short, OTS Reader connects to OTS server by using official OTS Java SDK, reads and transfers data to data synchronization field information according to official data synchronization protocol standard, and then transmits the information to downstream Writer side.

Based on OTS table range, OTS Reader divides the range into multiple tasks according to the number of data synchronization concurrencies. Each task is implemented with an OTS Reader thread.

Currently, OTS Reader supports all OTS types. The conversion of OTS types in the OTS Reader is as follows:

|Category|MySQL data type|
|:-------|:--------------|
|Integer|Integer|
|Float|Double|
|String type|String|
|Boolean|Boolean|
|Binary|Binary|

**Note:** OTS itself does not support "date" type. Long value is generally used as Unix TimeStamp at application layer when an error is reported.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|endpoint|The endpoint for the OTs server \(service address\), see [access control for details](https://help.aliyun.com/document_detail/27296.html).|Yes|N/A|
|accessId|The accessId of the OTS.|Yes|N/A|
|accessKey|The accesskey of the OTS.|Yes|N/A|
|Instance name|Description: The name of OTS instance. The instance is an entity for using and managing OTS service.After you enable the OTS service, you can create an instance in the Console to create and manage tables.

The instance is the basic unit for OTS resource management. All access control and resource measurement done by the OTS for applications are completed at the instance level.

|Yes|N/A|
|table.|Description: The name of the table to be extracted. Only one table can be filled in. Multi-table synchronization is not required for OTS.|Yes|N/A|
|column|Description: The column name set to be synchronized in the configured table. Field information is described with arrays in JSON. Because OTS itself is a NoSQL system, the corresponding field name must be specified when OTS Reader extracts data.-   Reading of ordinary columns is supported, for example, \{"name":"col1"\}.
-   Reading of partial columns is supported. OTS Reader does not read unconfigured columns.
-   Reading of constant columns is supported, for example, \{"type":"STRING", "value" : "DataX"\}. "type" is used to describe constant types. Currently supported types include STRING, INT, DOUBLE, BOOL, BINARY \(entered with a value encoded using Base64\), INF\_MIN \(minimum system limit value for OTS. You cannot enter the value attribute if this value is specified, otherwise an error may occur\), and INF\_MAX \(maximum system limit value for OTS. You cannot enter the value attribute if this value is specified, otherwise an error may occur\).
-   Function or custom expression is not supported. Because OTS itself does not provide function or expression similar to SQL, OTS Reader does not provide function or expression either.

|Yes|N/A|
|begin/end|Description: This configuration item that must be used in pairs allows data to be extracted from OTS table range. "begin/end" describes the distribution of OTS PrimaryKeys within the range which must cover all PrimaryKeys. The range of PrimaryKeys under the OTS table requires to be specified. For the range with infinite limit, use \{"type":"INF\_MIN"\} and \{"type":"INF\_MAX"\}. For example, if you want to extract data from an OTS table with the primary key of \[DeviceID, SellerID\], begin/end is configured as follows:```
"range":{
      "begin":[
        {"Type": "inf_min"}, // specify the minimum value of ergonomic ID
        
      ], 
      "end":[
        {"type": "INF_MAX"}, // specify the maximum value for ergonomic ID Extraction
        
      ]
    }
```

To extract data from the entire table, use the following configuration:

```
"range":{
      "begin":[
        {"type": "INF_MIN"}, // specify the minimum value of ergonomic ID
        
      ], 
      "end":[
        {"type": "INF_MAX"}, // specify the maximum value for ergonomic deviceID Extraction
          
      ]
    }
```

|Yes|Blank|
|split|Description: This is an advanced configuration item for the configuration of custom splitting, which is generally not recommended.Application scenario: The custom splitting rule is generally used when OTS Reader's auto splitting policy is invalid in the hotspot where OTS data is stored.

"split" specifies a splitting point within the range between Begin and End and only the information of splitting point for partitionKey, which means that only partitionKey is configured for split, but not all PrimaryKeys require to be specified.

If you want to extract data from an OTS table with the primary key of \[DeviceID, SellerID\], the configuration is as follows:

```
"range":{
      "begin":{
        {"type": "INF_MIN"}, //specify the minimum value of ergonomic DeviceID
        
      }, 
      "end":{
        {"type": "INF_MAX"}, //specify the maximum value for ergonomic DeviceID Extraction
        
      }，
       // The specified splitting point. If a splitting point is specified, Job splits Task according to begin, end, and split.
            // The column to be split is only Partition Key (first column of PrimaryKey).
            // INF_MIN, INF_MAX, STRING, INT are supported
            "split":[
                                {"type":"STRING", "value" : "1"},
                                {"type":"STRING", "value" : "2"},
                                {"type":"STRING", "value" : "3"},
                                {"type":"STRING", "value" : "4"},
                                
                    ]
    }
```

|No|N/A|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Development in wizard mode is unavailable currently.

## Development in script mode {#section_cp2_wsh_p2b .section}

Configure a job to extract data synchronously from the entire OTS table to local machine.

```
{
    "type": "job",
    "version": "2.0", //Indicates the version.
    "steps":[
        {
            "stepType": "ots", //plug-in name
            "parameter": {
                "datasource": "", //Data Source
                "column": [// Field
                    {
                        "name": "columnn1" // field name
                    },
                    {
                        "name": "column2"
                    },
                    {
                        "name": "column3"
                    },
                    {
                        "name": "column4"
                    },
                    {
                        "name": "column5"
                    }
                ],
                "range":{
                    "split":[
                        {
                            "type": "INF_MIN"
                        },
                        {
                            "type": "STRING",
                            "value": "splitPoint1"
                        },
                        {
                            "type": "STRING",
                            "value": "splitPoint2"
                        },
                        {
                            "type": "STRING",
                            "value": "splitPoint3"
                        },
                        {
                            "type": "INF_MAX"
                        }
                    ],
                    "end":[
                        {
                            "type": "INF_MAX"
                        },
                        {
                            "type": "INF_MAX"
                        },
                        {
                            "type": "STRING",
                            "value": "end1"
                        },
                        {
                            "type": "INT",
                            "Value":"100"
                        }
                    ],
                    "begin":[
                        {
                            "type": "INF_MIN"
                        },
                        {
                            "type": "INF_MIN"
                        },
                        {
                            "type": "STRING",
                            "value": "begin1"
                        },
                        {
                            "type": "INT",
                            "value": "0"
                        }
                    ]
                },
                "table": "// table name
            },
            "name": "Reader ",
            "category": "reader"
        },
        {//The following is a writer template. You can find the corresponding writer plug-in documentations.
            "stepType": "stream",
            "parameter":{},
            "name": "writer",
            "category": "writer"
        }
    ],
    "setting":{
        "errorLimit": {
            "record": "0"//Number of error records
        },
        "speed": {
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "concurrent": "1",//Number of concurrent tasks
            "dmu": 1 // DMU Value
        }
    },
    "order":{
        "hops":[
            {
                "from": "Reader ",
                "to": "Writer"
            }
        ]
    }
}
```

