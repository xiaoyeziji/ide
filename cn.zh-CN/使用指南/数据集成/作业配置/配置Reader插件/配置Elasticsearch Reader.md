# 配置Elasticsearch Reader {#concept_ihc_mmp_hhb .concept}

本文将为您介绍Elasticsearch Reader的工作原理、功能和参数。

## 工作原理 {#section_sbw_pmp_hhb .section}

-   使用Elasticsearch的\_search + scroll + slice （即游标+分片）方式实现，slice结合DataX job的task多线程分片机制。
-   根据Elasticsearch中的mapping配置进行数据类型转换。

更多详情请参见[Elasticsearch官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-scroll.html)。

## 基本配置 {#section_ysk_bnp_hhb .section}

```
"reader": {
    "name": "elasticsearchreader",
    "parameter": {
        "endpoint": "127.0.0.1:0000",  // 服务地址
        "username": "<yourUserName>", // 用户名
        "password": "<yourPassWord>", // 密码
        "index": "xxxx", // 索引
        "type": "default", // 索引类型
        "column": [       // 读取列
            "id",
            "name"
        ],
        "pageSize": 1,  // 分页大小
        "search": {       // 查询query参数, 与es的query内容相同，使用_search api,故意重命名为search
            "match": {
                "type": "<yourType>"
            }
        },
        "scroll": "1m", // scroll标志
        "sort": { "_id": "asc" }, // 排序 k：属性名  v：排序值 asc desc
        "retryCount": 3,
        "retrySleepTime": 1000,
        "connTimeOut": 1000,
        "readTimeOut": 5000
    }
}
```

## 高级功能 {#section_q1x_znp_hhb .section}

-   支持全量拉取

    支持将Elasticsearch一个文档的所有内容拉取为一个字段。

-   支持半结构化到结构化数据的提取
    -   产生背景

        Elasticsearch中的数据特征为字段不固定且有中文名、数据使用深层嵌套的形式，为更好地方便下游业务对数据的计算和存储需求，特推出从半结构化到结构化的转换解决方案。

    -   实现原理

        将Elasticsearch获取到的json数据，利用json工具的路径获取特性，将嵌套数据扁平化为一维结构的数据，然后将数据映射到结构化数据表中，将Elasticsearch复合结构数据拆分到多个结构化数据表。

    -   解决方案
        -   json有嵌套，通过path路径来解决。
            -   属性
            -   属性.子属性
            -   属性\[0\].子属性
        -   附属信息有一对多的情况，需要进行拆表拆行处理，进行遍历。

            属性\[\*\].子属性

        -   数组归并，一个字符串数组内容，归并为一个属性，并进行去重。

            属性\[\] 去重

        -   多属性合一，将多个属性合并为一个属性。

            属性1,属性2

        -   多属性选择处理

            属性1|属性2


## 参数说明 {#section_kx3_bf5_hhb .section}

|参数|描述|是否必选|默认值|
|--|--|----|---|
|endpoint|Elasticsearch的连接地址|是|无|
|username|http auth中的username|否|空|
|password|http auth中的password|否|空|
|index|Elasticsearch中的index名|是|无|
|type|Elasticsearch中index的type名|否|index名|
|pageSize|每次读取数据的条数|否|100|
|search|Elasticsearch的query参数|是|无|
|scroll|Elasticsearch的分页参数，设置游标存放时间|是|无|
|sort|返回结果的排序字段|否|无|
|retryCount|失败后重试的次数|否|300|
|connTimeOut|客户端连接超时时间|否|600000|
|readTimeOut|客户端读取超时时间|否|600000|
|multiThread|http请求，是否有多线程。|否|true|
|column|Elasticsearch所支持的字段类型，样例中包含了全部。|是|无|
|full|是否支持将Elasticsearch的数据拉取为一个字段|否|false|
|multi|是否支持将数组进行列拆多行的处理，需要辅助设置子属性。|否|false|

补充配置：

```
"full":false,
        "multi": {
          "multi": true,
          "key":"crn_list[*]"
        }
```

