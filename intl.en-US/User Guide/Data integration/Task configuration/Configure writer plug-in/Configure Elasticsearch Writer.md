# Configure Elasticsearch Writer {#concept_okj_c24_q2b .concept}

This topic describes the data types and parameters supported by Elasticsearch Writer and how to configure Writer in both Wizard and Script mode.

The Elasticsearch is a Lucene-based search and data analysis tool that provides distributed service. Elasticsearch is an open source product based on Apache's open source terms, and is currently a mainstream enterprise-class search engine. The Elasticsearch core concept that corresponds to core database concepts as follows.

```
Relational DB (Instance)-> databases (database)-> tables (table) -> rows (one row of data)-> Columns (one row of data)
Innissearch-> index-> types-> documents-> Fields
```

There can be multiple indexes \(INDEX\)/\(database\) in Elasticsearch, where each index can contain multiple types \(type\)/\(table\). Each type can contain multiple document rows, and each document can contain multiple fields \(columns\). The Elasticsearch Writer plug-in uses Elasticsearch REST API interface to write data that is read from the reader in bulk to Elasticsearch.

## Parameter description​ {#section_a3m_424_q2b .section}

|Parameter|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|endpoint|The Elasticsearch URL in the format of `http://xxxx.com:9999`.|No|None|
|accessId|The Elasticsearch user name, which is used for authorizing an Elasticsearch connection.|No|None|
|accessKey|The password of the Elasticsearch instance.|No|N/A|
|index|The index name in Elasticsearch.|No|None|
|indexType|The index type name in Elasticsearch.|No|Elasticsearch|
|cleanup|The parameter that determines if a data exists in an index or has been deleted. The method used to clean data is to delete and rebuild the corresponding index. By default, the value is False which means data in the existing index is retained.|No|False|
|batchSize|The number of data entries imported in bulk each time.|No|1,000|
|trySize|The number of retries after task failure.|No|30|
|timeout-|The client timeout.|No|600,000|
|discovery|When this Node Discovery parameter is enabled, the server list in the client is polled and regularly updated.|No|False|
|compression|The parameter that specifies whether compression is enabled for HTTP requests.|No|True|
|multiThread|The HTTP request that specifies if the request is multiple threads.|No|True|
|ignoreWriteError|This parameter ignores writing errors and writes without retries.|No|False|
|ignoreParseError|This parameter ignores parsing data format errors and continues writes.|No|True|
|alias|The Elasticsearch's alias is similar to the database view mechanism, and creates an alias name for the index my\_index. This operation is similar to the my\_index operation.Configuring the alias means that after completing the data import, an alias is created for the specified index.

|No|N/A|
|aliasMode|The modes for adding an alias after data is imported. The modes are append and exclusive.| No|No|
|settings|If you insert an array type target-side data column, use the specified delimiter \(-, -\) to separate the data source. For example:The source column data type is a string `a-,-b-,-c-,-d` that uses the delimiter \(-,-\). When the string is split into an array `["a", "b", "c", "d"]`, it is written into the Elasticsearch corresponding to the Filed column.

| No|-,-|
|column|The column used to configure multiple document fields. Each specific field item can configure basic configurations, such as name, type, and more. Available column extension configurations, include Analyzer, Format, and Array. The specific instructions are as follows:The field types supported by Elasticsearch are as follows.

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

If the column type is text, you can configure the analyzer, norms, and index\_options parameters as follows.```
{
        "name": "col_text ",
        "type": "text",
        "analyzer": "ik_max_word"
    }
```

If the column type is date, you can configure the Format and Timezone parameters. These represent a date serialization format and a time zone, respectively, as follows.

```
{
        "name": "col_date ",
        "type": "date",
        "format": "yyyy-MM-dd HH:mm:ss",
        "timezone": "UTC"
    }
```

You can configure the tree \(geohash or quadtree\) when the column type is ge\_shape\), precision properties, as follows.

```
{
        "name": "col_geo_shape",
        "type": "geo_shape ",
        "tree": "quadtree ",
        "precision": "10m"
    }
```

The array column is displayed, if you have configured the array property in the column filed, and the value is true. This plug-in uses the delimiter configured by the splitter, where only one split delimiter is supported per task. The corresponding source data is then split, and converted into a string array, and then written to the destination as follows.

```
{
        "name": "col_integer_array",
        "type": "integer ",
        "array": true
    }
```

|Yes|N/A|
|dynamic|This parameter uses automatic Elasticsearch mappings instead of DataX mappings.| No|False|

## Development in Script Mode {#section_pcz_fh4_q2b .section}

The following is an example of a script configuration. For details about the parameter configurations, see the preceding Parameter Description.

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
          "name": "Elasticsearchwriter",
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

**Note:** Currently, Elasticsearch for the VPC environment can only use custom scheduling resources. If you run the default Resource Group, the network connection will breakdown. For more information on how to add a custom resource group, see[Add task resources](reseller.en-US/User Guide/Data integration/Common configuration/Add task resources.md#).

