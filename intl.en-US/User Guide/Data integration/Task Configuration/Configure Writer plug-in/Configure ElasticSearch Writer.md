# Configure ElasticSearch Writer {#concept_okj_c24_q2b .concept}

In this article we will show you the data types and parameters supported by ElasticSearch Writer and how to configure Writer in both wizard mode and script mode.

ElasticSearch is a Lucene-based search and data analysis tool that provides a distributed service. ElasticSearch is an open source product that follows Apache's open source terms and is currently the mainstream enterprise-class search engine. The ElasticSearch core concept corresponds to the core concepts of the database as follows.

```
Relational DB (Instance)-> databases (database)-> tables (table) -> rows (one row of data)-> Columns (one row of data)
Innissearch-> index-> types-> documents-> Fields
```

There can be multiple indexes \(INDEX\)/\(database\) in essbase search \), each index can contain multiple types \(type\)/\(table \), each type can contain multiple document rows, each document can then contain multiple fields \(columns \). The ElasticSearch writer plug-in uses the rest API interface of ElasticSearch, write the data that is read from the reader in bulk to ElasticSearch.

## Parameter description​ {#section_a3m_424_q2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|endpoint|Description: ElasticSearch URL, in the format of `http://xxxx.com:9999`.|No|None|
|accessId|The username of ElasticSearch, which is used for authorization when a connection with ElasticSearch is established.|No|None|
|accessKey|Description: Password of the ElasticSearch instance.|No|N/A|
|index|Description: index name in ElasticSearch.|No|None|
|indexType|Description: type name of index in ElasticSearch.|No|ElasticSearch|
|cleanup|Whether the data already exists in the index is deleted or not, the method used to clean the data is to delete and rebuild the corresponding index, the default value of false indicates that the data in the existing index is retained.|No|false|
|batchSize|Description: Number of data entries imported in batch each time.|No|1,000|
|trySize|Description: Number of retries after failure.|No|30|
|timeout-|Client timeout.|No|600,000|
|discovery|Description: When Node Discovery is enabled, server list in the client is polled and regularly updated.|No|false|
|compression|Description: Specifies whether compression is enabled for HTTP requests.|No|true|
|multiThread|Description: http request, multiple threads or not.|No|true|
|ignoreWriteError|Description: Ignore writing error and keep writing without retries.|No|false|
|ignoreParseError|Description: Ignore format error of parsing data and keep writing.|No|true|
|alias|ElasticSearch's alias is similar to the database's view mechanism, creating an alias name for the index my\_index, this is like the operation of my\_index with respect to maid.Configuring alias means that after the data import is complete, an alias is created for the specified index.

|No|N/A|
|aliasMode|Description: Modes of adding an alias after the data is imported: append and exclusive.| No|append|
|Settings|If you are inserting a target-side data column type that is an array type, use the specified separator \(-, -\) split the source data. Example:Source column is string type data `a-,-b-,-c-,-d`, using the separator-,-after the split is the array `["a", "b", "c", "d"]`, eventually written into the ElasticSearch corresponding filed column.

| No|-,-|
|column|Column is used to configure multiple fields of the document, filed information, each specific field item can be configured with a base configuration such as name, type, and so on, and extension configurations such as analyzer, format, and array. Specific instructions are as follows:The Field Types supported by essbase search are as follows.

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

You can configure analyzer when the column type is text type\) the, norms, index\_options parameters are as follows.```
{
        "name": "col_text ",
        "type": "text",
        "analyzer": "ik_max_word"
    }
```

When the column type is a date type, you can configure the format, timezone parameters, represents a date serialization format and a time zone, respectively, as an example.

```
{
        "name": "col_date ",
        "type": "date",
        "format": "yyyy-MM-dd HH:mm:ss",
        "timezone": "UTC"
    }
```

You can configure tree \(geohash or quadtree\) when the column type is ge\_shape\), precision properties, as in the following example.

```
{
        "name": "col_geo_shape",
        "type": "geo_shape ",
        "tree": "quadtree ",
        "precision": "10m"
    }
```

If you have configured the array property in the column filed, and the value is true, the array column is represented. This plug-in uses the separator configured by the splitter \(only one split separator is supported for one task\) the corresponding source data is split, converted into an array of strings and eventually written to the destination, the example is below.

```
{
        "name": "col_integer_array",
        "type": "integer ",
        "array": true
    }
```

|Yes|N/A|
|dynamic|Description: Use automatic mappings of ElasticSearch instead of DataX mappings.| No|false|

## Development in script mode {#section_pcz_fh4_q2b .section}

The following is a script configuration sample. For details about parameters, see the preceding Parameter Description.

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

**Note:** ElasticSearch for the VPC environment, currently using only custom scheduling resources, if you run in the default Resource Group, there will be a network breakdown. To add a Custom Resource Group, see[Add scheduling resources](reseller.en-US/User Guide/Data integration/Common configuration/Add scheduling resources.md#)

