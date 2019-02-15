# Configure ElasticSearch Writer {#concept_okj_c24_q2b .concept}

This topic describes the data types and parameters supported by ElasticSearch Writer and how to configure Writer in both wizard and script mode.

The ElasticSearch is a Lucene-based search and data analysis tool that provides distributed service. ElasticSearch is an open source product based on Apache's open source terms, and is currently a mainstream enterprise-class search engine. The ElasticSearch core concept that corresponds to the database core concepts as follows.

```
Relational DB (Instance)-> databases (database)-> tables (table) -> rows (one row of data)-> Columns (one row of data)
Innissearch-> index-> types-> documents-> Fields
```

There can be multiple indexes \(INDEX\)/\(database\) in ElasticSearch, where each index can contain multiple types \(type\)/\(table \). Each type can contain multiple document rows, each document can then contain multiple fields \(columns\). The ElasticSearch Writer plug-in uses ElasticSearch REST API interface to write data that is read from the reader in bulk to ElasticSearch.

## Parameter description​ {#section_a3m_424_q2b .section}

|Attribute|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|endpoint|The ElasticSearch URL in the format of `http://xxxx.com:9999`.|No|None|
|accessId|The user name of ElasticSearch, which is used for authorization when a connection with the ElasticSearch is established.|No|None|
|accessKey|The password of the ElasticSearch instance.|No|N/A|
|index|The index name in ElasticSearch.|No|None|
|indexType|The index type name in ElasticSearch.|No|ElasticSearch|
|cleanup|The parameter that determines if a data exists in an index or has been deleted. The method used to clean data is to delete and rebuild the corresponding index. The default value of False indicates the data in the existing index is retained.|No|False|
|batchSize|The number of data entries imported in the batch each time.|No|1,000|
|trySize|The number of retries after failure.|No|30|
|timeout-|The client timeout.|No|600,000|
|discovery|When this Node Discovery parameter is enabled, the server list in the client is polled and regularly updated.|No|False|
|compression|The parameter that specifies whether compression is enabled for HTTP requests.|No|True|
|multiThread|The HTTP request that specifies if the request is multiple threads or not.|No|True|
|ignoreWriteError|Ignores writing errors and writes without retries.|No|False|
|ignoreParseError|Ignores parsing data format errors and continues writes.|No|True|
|alias|The ElasticSearch's alias is similar to the database view mechanism, and creates an alias name for the index my\_index. This operation is similar to the my\_index operation.Configuring the alias means that after completing the data import, an alias is created for the specified index.

|No|N/A|
|aliasMode|The modes for adding an alias after data is imported. The modes are append and exclusive.| No|No|
|settings|If you insert an array type target-side data column, use the specified separator \(-, -\) to split the source data. For example:The source column is string type data `a-,-b-,-c-,-d` that uses the delimiter \(-,-\). After the split is the array `["a", "b", "c", "d"]`, eventually written into the ElasticSearch corresponding to the Filed column.

| No|-,-|
|column|The column used to configure multiple document fields, each specific field item can be configured with a base configuration, such as name, type, and more. Available column extension configurations, include analyzer, format, and array. Specific instructions are as follows:The field types supported by ElasticSearch are as follows.

```
- id
- string
- text
- keyword
- long
- integer
- short
- byte
- double
- float
- date
- boolean
- binary
- integer_range
- float_range
- long_range
- double_range
- date_range
- geo_point
- geo_shape
- ip
- completion
- token_count
- array
-Object
- nested
```

If the column type is text, you can configure the analyzer, norms, and index\_options parameters as in the following example.```
{
        "name": "col_text ",
        "type": "text",
        "analyzer": "ik_max_word"
    }
```

If the column type is date, you can configure the Format and Timezone parameters. These represent a date serialization format and a time zone, respectively, as in the following example.

```
{
        "name": "col_date ",
        "type": "date",
        "format": "yyyy-MM-dd HH:mm:ss",
        "timezone": "UTC"
    }
```

You can configure the tree \(geohash or quadtree\) when the column type is ge\_shape\), precision properties, as in the following example.

```
{
        "name": "col_geo_shape",
        "type": "geo_shape ",
        "tree": "quadtree ",
        "precision": "10m"
    }
```

The array column is displayed, if you have configured the array property in the column filed, and the value is true. This plug-in uses the delimiter configured by the splitter, where only one split delimiter is supported per task. The corresponding source data is then split, and converted into a string arrays, and written to the destination as in the following example.

```
{
        "name": "col_integer_array",
        "type": "integer ",
        "array": true
    }
```

|Yes|N/A|
|dynamic|Uses automatic ElasticSearch mappings instead of DataX mappings.| No|False|

## Development in script mode {#section_pcz_fh4_q2b .section}

The following is an example of a script configuration. For details about the parameters, see the preceding Parameter Description.

```
{
  "job": {
    "setting": {
      ...
    },
    "content": [
      {
        "reader": {
          ...
        },
        "writer": {
          "name": "ElasticSearchwriter",
          "parameter": {
            "endpoint:" "http://xxxx.com: 9999 "",
            "accessId": "xxxx",
            "accessKey": "yyyy",
            "index": "test-1",
            "type": "default",
            "cleanup": true,
            "settings": {"index" :{"number_of_shards": 1, "number_of_replicas": 0}},
            "discovery": false,
            "batchSize": 1000,
            "splitter": ",",
            "column": [
              {"name": "pk", "type": "id"},
              { "name": "col_ip","type": "ip" },
              { "name": "col_double","type": "double" },
              { "name": "col_long","type": "long" },
              { "name": "col_integer","type": "integer" },
              { "name": "col_keyword", "type": "keyword" },
              { "name": "col_text", "type": "text", "analyzer": "ik_max_word"},
              { "name": "col_geo_point", "type": "geo_point" },
              { "name": "col_date", "type": "date", "format": "yyyy-MM-dd HH:mm:ss"},
              { "name": "col_nested1", "type": "nested" },
              { "name": "col_nested2", "type": "nested" },
              { "name": "col_object1", "type": "object" },
              { "name": "col_object2", "type": "object" },
              { "name": "col_integer_array", "type":"integer", "array":true},
              
            ]
          }
        }
      }
    ]
  }
}
```

**Note:** Currently, the ElasticSearch for the VPC environment uses only custom scheduling resources. If you run in the default Resource Group, the network connection will breakdown. To add a Custom Resource Group, see[Add task resources](reseller.en-US/User Guide/Data integration/Common Configuration/Add task resources.md#).

