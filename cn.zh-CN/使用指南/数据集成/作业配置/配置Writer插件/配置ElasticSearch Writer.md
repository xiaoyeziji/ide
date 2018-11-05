# 配置ElasticSearch Writer {#concept_okj_c24_q2b .concept}

本文为您介绍ElasticSearch Writer支持的数据类型、写入方式、字段映射和数据源等参数及配置举例。

Elasticsearch是一个基于Lucene的搜索和数据分析工具，它提供了一个分布式服务。Elasticsearch是遵从Apache开源条款的一款开源产品，是当前主流的企业级搜索引擎。Elasticsearch核心概念同数据库核心概念对应关系如下所示。

```
Relational DB(实例) -> Databases(数据库) -> Tables(表) -> Rows(一行数据) -> Columns(一行数据的一列)
Elasticsearch       -> Index             -> Types      -> Documents    -> Fields
```

Elasticsearch中可以有多个索引（index）/（数据库），每个索引可以包含多个类型（type）/（表），每个类型可以包含多个文档（document）行，然后每个文档可以包含多个字段（Field）（列）。ElasticSearch Writer插件使用Elasticsearch的Rest API接口，批量把从Reader读入的数据写入Elasticsearch中。

## 参数说明 {#section_a3m_424_q2b .section}

|参数|描述|是否必选|默认值|
|:-|:-|:---|:--|
|endpoint|ElasticSearch的连接地址，一般格式为`http://xxxx.com:9999`。|否|无|
|accessId|ElasticSearch的username，用于与ElasticSearch建立连接时的鉴权。|否|无|
|accessKey|ElasticSearch的password。|否|无|
|index|ElasticSearch中的index名。|否|无|
|indexType|ElasticSearch中index的type名。|否|elasticsearch|
|cleanup|是否删除所配索引index中已有数据，清理数据的方法为删除并重建对应索引，默认值为false表示保留已有索引中的数据。|否|false|
|batchSize|每次批量数据的条数。|否|1000|
|trySize|失败后重试的次数。|否|30|
|timeout|客户端超时时间。|否|600000|
|discovery|启用节点发现将轮询并定期更新客户机中的服务器列表。|否|false|
|compression|http请求，开启压缩。|否|true|
|multiThread|http请求，是否有多线程。|否|true|
|ignoreWriteError|忽略写入错误，不重试，继续写入。|否|false|
|ignoreParseError|忽略解析数据格式错误，继续写入。|否|true|
|alias|Elasticsearch的别名类似于数据库的视图机制，为索引my\_index创建一个别名my\_index\_alias，这样对my\_index\_alias的操作就像对my\_index的操作一样。配置alias表示在数据导入完成后，为指定的索引创建别名。

|否|无|
|aliasMode|数据导入完成后增加别名的模式，append（增加模式），exclusive（只留这一个）。|否|append|
|settings|如果待插入目标端数据列类型是array数组类型，就使用指定分隔符（-,-）将源头数据进行拆分写出。示例如下：源头列是字符串类型数据`a-,-b-,-c-,-d`，使用分隔符-,-拆分后是数组`["a", "b", "c", "d"]`，最终写出到ElasticSearch对应Filed列中。

|否|-,-|
|column|column用来配置文档的多个字段Filed信息，具体每个字段项可以配置name（名称）、type（类型）等基础配置，以及Analyzer、Format和Array等扩展配置。具体说明如下。Elasticsearch所支持的字段类型如下所示。

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
- object
- nested
```

在列类型为text类型时，可以配置analyzer（分词器）、norms、index\_options参数，示例如下。```
{
        "name": "col_text",
        "type": "text",
        "analyzer": "ik_max_word"
    }
```

在列类型为日期Date类型时，可以配置Format、Timezone参数，分别表示日期序列化格式和时区，示例如下。

```
{
        "name": "col_date",
        "type": "date",
        "format": "yyyy-MM-dd HH:mm:ss",
        "timezone": "UTC"
    }
```

在列类型为地理形状geo\_shape时，可以配置tree（geohash或quadtree）、precision（精度）属性，示例如下。

```
{
        "name": "col_geo_shape",
        "type": "geo_shape",
        "tree": "quadtree",
        "precision": "10m"
    }
```

如果您在列Filed中配置了array属性，且值为true时，则表示数组列。本插件会使用splitter配置的分隔符（一个任务仅支持配置一种切分分隔符）将对应源端数据进行拆分，转换成字符串数组形式最终写出到目的端，示例如下。

```
{
        "name": "col_integer_array",
        "type": "integer",
        "array": true
    }
```

|是|无|
|dynamic|如果为true，则不使用datax的mappings，使用ES自己的自动mappings。|否|false|

## 脚本开发介绍 {#section_pcz_fh4_q2b .section}

脚本配置示例如下，具体参数请参见上文的参数说明。

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
          "name": "elasticsearchwriter",
          "parameter": {
            "endpoint": ""http://xxxx.com:9999"",
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
              { "name": "col_geo_shape", "type":"geo_shape", "tree": "quadtree", "precision": "10m"}
            ]
          }
        }
      }
    ]
  }
}
```

**说明：** VPC环境的ElasticSearch，目前只能使用自定义调度资源，运行在默认资源组会存在网络不通的情况。添加自定义资源组具体的步骤请参见[新增调度资源](intl.zh-CN/使用指南/数据集成/常见配置/新增调度资源.md#)

