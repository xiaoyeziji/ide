# Configuring HDFS Reader {#concept_azr_4qj_p2b .concept}

This topic describes how to configure the HDFS Reader. HDFS Reader provides the ability to read data stored by the distributed file systems. At the underlying implementation level, HDFS Reader retrieves data on the distributed file system, and converts data into a Data Integration transport protocol and transfers it to the Writer.

HDFS Reader provides the ability to read file data from the Hadoop distributed file system HDFS and converts data into a Data Integration transport protocol.

For example:

By default, the TextFile is the storage format for creating Hive tables without data compression. Essentially, the TextFile stores data in HDFS as text, and the HDFS Reader implementation is similar to that of an OSS Reader for Data Integration. ORCFile is the acronym for Optimized Row Columnar File, which is the optimized RCFile. This file format provides an efficient method for storing Hive data. HDFS Reader utilizes the OrcSerde class provided by Hive to read and parse ORCFile data.

**Note:** Data synchronization requires an admin account and files read/write permissions.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16224/15514313147725_en-US.png)

Usage:

-   Create an admin user and home directory to specify a user group and additional group, and for granting file permissions.

    ```
    useradd -m -G supergroup -g hadoop -p admin admin 
    ```

    -   `-G supergroup`: Specifies the additional group to which the user belongs.
    -   `-g hadoop`: Specifies the user group to which the user belongs.
    -   `-p admin admin`: Add a password to the admin user.
-   View the contents of the files in this directory.

    ```
    hadoop fs -ls /user/hive/warehouse/hive_p_partner_native
    ```

    When using Hadoop commands, the format is `hadoop fs -command`. The command means command.

-   Copies the file part-00000 to the local file system.

    ```
    hadoop fs -get /user/hive/warehouse/hive_p_partner_native/part-00000
    ```

-   Edit the file you just copied.

    ```
    vim part-00000
    ```

-   Exits the current user.

    ```
    exit
    ```

-   Connects the host from the list and create an admin account on each attached host.

    ```
    pssh -h /home/hadoop/slave4pssh useradd -m -G supergroup -g hadoop -p admin admin
    ```

    -   `pssh -h /home/hadoop/slave4pssh`: Connect to the host from the manifest file.
    -   `useradd -m -G supergroup -g hadoop -p admin admin`: Create an admin account.

## Supported functions {#section_fbh_pqj_p2b .section}

Currently, HDFS Reader supports the following features:

-   Supports TextFile, ORCFile, rcfile, sequence file, csv, and parquet file formats. The file logically has a two-dimensional table.
-   Supports reading multiple data types represented by Strings and supports column pruning and column constants.
-   Supports recursive reading and regular expressions "\*" and "?".
-   Supports ORCFile data compression, and currently supports the SNAPPY and ZLIB compression modes.
-   Supports data compression for sequence files, and currently supports the lzo compression mode.
-   Supports concurrent reading of multiple files.
-   Supports the following compression formats for the csv type: gzip, bz2, zip, lzo, lzo\_deflate, and snappy.
-   In the current plug in, the Hive version is 1.1.1, and the Hadoop version is 2.7.1 \(Apache \[is compatible with JDK 1.6\]\). Data can be written normally in the testing environments of Hadoop 2.5.0, Hadoop 2.6.0, and Hive 1.2.0. For other versions, further tests are required.

**Note:** Currently, HDFS Reader does not support multi-thread concurrent reading of a single file, which requires internal splitting algorithm of the file.

## Supported data types {#section_mtj_dck_p2b .section}

**RCfile**

If the synchronized HDFS file type is a RCfile, you must specify the column data type in the Hive table under “column type” because the data storage mode varies with the data type during the RCfile underlying storage. The HDFS Reader does not support accessing and querying Hive metadata databases. If the column type is BIGINT, DOUBLE, or FLOAT, enter respectively BIGINT, DOUBLE, or FLOAT. If the column type is varchar or char, enter the string for the same purpose.

RCFile data types are converted into default internal types supported by Data Integration, as shown in the following comparison table.

|Type classification|HDFS data type|
|:------------------|:-------------|
|Integer|Tinyint, smallint, int, and bigint|
|Float|Float, Double, decimal|
|String type|String, Char, and Varchar|
|Date and time type|Date and timestamp|
|Boolean class|Boolean|
|Binary class|BINARY|

**Parquetfile**

By default, the ParquetFile data types are converted into internal types supported by Data Integration, as shown in the following comparison table.

|Type classification|HDFS data type|
|:------------------|:-------------|
|Integer|Int32, int64, and int96|
|Floating point|Float and double|
|String type|FIXED\_LEN\_BYTE\_ARRAY|
|Date and time type|Date and timestamp|
|Boolean|Boolean|
|Binary|BINARY|

**TextFile, ORCfile, and SequenceFile**

Given that the metadata of TextFile and ORCFile file tables is maintained and stored in the database maintained by Hive, such as MySQL. Currently, HDFS Reader does not support Hive metadata database access and query, so you must specify a data type for conversion.

By default, the TextFile, ORCFile, and SequenceFile data types are converted into internal types supported by Data Integration, as shown in the following comparison table.

|Category|HDFS data type|
|:-------|:-------------|
|Integer|Tinyint, smallint, int, and bigint|
|Floating point|FLOAT and DOUBLE|
|String type|String, Char, VARCHAR, Struct, MAP, Array, Union, BINARY|
|Date and time|Date and timestamp|
|Boolean|Boolean|

Notes:

-   LONG: Represents an INTEGER string in the HDFS file, such as 123456789.
-   DOUBLE: Represents a DOUBLE string in the HDFS file, such as 3.1415.
-   BOOLEAN: Represents a BOOLEAN string in the HDFS file, such as true or false and is case-insensitive.
-   DATE: Represents a date and time string in the HDFS file, such as 2014-12-31 00:00:00.

**Note:** The TIMESTAMP data type supported by Hive can be accurate to the nanosecond, so the TIMESTAMP data content stored in TextFile and ORCFile can be in the format like “2015-08-21 22:40:47.397898389”. If the converted data type is set as the Date for Data Integration, the nanosecond part is truncated after conversion. If you want to retain this part, set the converted data type as the String for Data Integration.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|path|It refers to the read file path. If you want to read multiple files, use a regular expression to match all of them, such as /hadoop/data\_201704\*.-   If a single HDFS file is specified, the HDFS Reader only supports single-threaded data extraction.
-   If multiple HDFS files are specified, the HDFS Reader supports multiple-threaded data extraction, and the number of concurrent threads is determined by the task speed \(mbps\). The actual number of initiated concurrent threads is the smaller of the number of HDFS files to be read and set task speed.

**Note:** The actual number of initiated concurrent threads is the smallest number of HDFS files read and set job speed.

-   When the wildcard is specified, the HDFS Reader attempts to traverse multiple files. For example: When the path "/" is specified, the HDFS Reader reads all files under the "/" directory. When "/bazhen/" is specified, the HDFS Reader reads all files under the bazhen directory. Currently, the HDFS Reader only supports wildcards that are asterisks \(\*\) and question marks\(?\), and the syntax is similar to that of common Linux command wildcards.

**Note:** 

-   Data Integration considers all files to be read in the same synchronization job as one data table. For this reason, you must ensure all those files adapt the same schema information and grant read permission to Data Integration.
-   **Note on reading partitions:** During Hive table creation, you can specify partitions. For example, after creating the partition\(day="20150820",hour="09"\), two directories with the name of /20150820 and /09 respectively are created in the table catalog of the HDFS file system and /20150820 is the parent directory of /09.

Because partitions are organized in a directory structure, we can set the path value in JSON when reading all data of a table by partitions. For example, if you want to read all data of the table named mytable01 on the partition day of 20150820, set as follows:

    ```
"path": "/user/hive/warehouse/mytable01/20150820/*"
    ```


|Yes|N/A|
|defaultFS|The namenode node address in HDFS. By default, the resource group does not support the configuration of HA, an advanced parameter of Hadoop.|Yes|N/A|
|fileType|The file type. Currently, only text, orc, rc, seq, csv, or parquet are supported. HDFS Reader can automatically identify files that are ORCFile, RCFile, Sequence File, TextFile, and csv types. Use the appropriate reading policy for the corresponding file type. Before data synchronization, the HDFS Reader checks whether all synchronized file types under the specified path are consistent with the fileType. The synchronization task fails if the synchronized file types are inconsistent to the fileType.The parameter values list that can be configured by fileType is as follows.

-   text: The TextFile format.
-   orc: The ORCFile format.
-   rc: The RCFile format.
-   seq: The sequence file format.
-   csv: The common HDFS file \(logical two-dimensional table\) format.
-   parquet: The common parquet file format.

**Note:** Because TextFile and ORCFile are different file formats, the HDFS Reader parses these two file types differently. For this reason, the converted format results varies when converting complex compound types supported by Hive, such as map, array, struct, and union to the String type supported by Data Integration. The following uses map type as an example.

-   After being parsed and converted to the String type supported by Data Integration, the ORCFile map type is \{job=80, team=60, person=70\}.
-   After being parsed and converted to the String type supported by Data Integration, the TextFile map type is job:80, team:60, person:70.

From the preceding results, the data remains unchanged but the representation formats are slightly different. For this reason, if the fields synchronized under the configured file path are compound in Hive, we recommend you set a unified file format.

**Recommended best practices:**

-   To unify file types parsed from compound types, we recommend you export TextFile tables as ORCFile tables on the Hive client.
-   If the file type is Parquet, the parquetSchema is required to describe the read Parquet file format.

For specified column information, you must enter type and select one from index and value.

|Yes|N/A|
|column|The list of fields read, when the type is the source data. The index indicates the column in which the current column location \(starts from 0\), and the value indicates the current type is constant and data is not read from the source file, but the corresponding column is automatically generated based on the value. By default, you can read data by taking the String as the only type. The configuration is as: `"column": ["*"]`.The column field can also be configured as follows:

```
{
  "type": "long",
  "index": 0 // Retrieves the int field from the first column of the local file text
},
{
  "type": "string",
  "value": "alibaba" // HDFS Reader internally generates the alibaba string field as the current field
}
```

|Yes|N/A|
|fieldDelimiter|It refers to the read field delimiter. The file delimiter is required when the HDFS Reader reads the TextFile data, and by default the delimiter is a comma \(,\). Field delimiters are not required if none are specified when the HDFS Reader reads the ORCFile data. The Hive default delimiter is \\u0001.-   To use each row as the target, use characters excluded from the row content as the delimiter, such as the invisible characters \\u0001.
-   Additionally, \\n cannot be used as the delimiter.

|No|,|
|encoding|Encoding the read files.|No|UTF-8|
|nullFormat|Text files do not allow defining null \(null pointer\) with a standard string. Data Integration provides nullFormat to define which strings can be expressed as null.For example, when nullFormat: "null" is configured. If the source data is "null", it is considered a null field in Data Integration.

|No|N/A|
|compress|It refers to fileType csv file compression formats, which currently supports gzip, bz2, zip, lzo, lzo\_deflate, hadoop-snappy, and framing-snappy.**Note:** 

-   Two lzo compression formats are available: lzo and lzo\_deflate. Select the corresponding configuration scenario.
-   Given that no unified stream format is now available for snappy, Data Integration currently only supports the most common two compression formats provided by Hadoop \(hadoop-snappy\) and Google recommended format \(snappy-framed \).
-   rc is the format of rcfile.
-   No entry is required for the orc file type.

|No|N/A|
|parquetSchema|This parameter is required for parquet format files. It is used to specify the target file structure, and takes effect only when the fileType is parquet. The format is as follows:```
message MessageType {
Required, data type, column name;
...................... ;
}
```

Notes:

-   MessageType: Any supported value.
-   Required: Required or Optional. We recommend you use Optional.
-   Data Type: Parquet files support the following data types: boolean, int32, int64, int96, float, double, binary select binary if the data type is string, and fixed\_len\_byte\_array.

**Note:** Note each configuration row and column, including the last one must end with a semicolon.

Configuration example:

```
message m {
optional int64 id;
optional int64 date_id;
optional binary datetimestring;
optional int32 dspId;
optional int32 advertiserId;
optional int32 status;
optional int64 bidding_req_num;
optional int64 imp;
optional int64 click_num;
}
```

|No|N/A|
|csvReaderConfig|Reads the CSV file parameter configurations. It is the Map type. This reading is performed by the CsvReader for reading CSV files and involves many configurations. If there are no configurations, the default values are used.Common configuration:

```
csvReaderConfig
  "safetySwitch": false,
  "skipEmptyRecords": false,
  "useTextQualifier": false
}
```

For all configuration items and default values, you must configure the csvReaderConfig map in accordance with the following field names.

```
boolean caseSensitive = true;
char textQualifier = 34;
boolean trimWhitespace = true;
boolean useTextQualifier = true;//Whether to use csv escape characters
char delimiter = 44;//Delimiter
Char fig = 0;
char comment = 35;
boolean useComments = false;
int escapeMode = 1;
boolean safetySwitch = true;// Whether the length of each column is limited to 100,000 characters
boolean skipEmptyRecords = true;// Whether to skip empty rows
boolean captureRawRecord = true;
```

|No|N/A|
|hadoopConfig|Certain Hadoop-related advanced parameters can be configured in hadoopConfig, such as the configuration of HA.```
hadoopConfig
"dfs.nameservices": "testDfs",
"dfs.ha.namenodes.testDfs": "namenode1,namenode2",
"dfs.namenode.rpc-address.youkuDfs.namenode1": "",
"dfs.namenode.rpc-address.youkuDfs.namenode2": "",
"dfs.client.failover.proxy.provider.testDfs":"org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider"
}
```

|No|N/A|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Currently, does not support development in wizard mode.

## Development in script mode {#section_cp2_wsh_p2b .section}

A script template can be imported for development. The following is a script configuration sample. For relevant parameters, see Parameter Description.

```
{
    "type": "job",
    "version": "2.0",
    "steps": [
        {
            "stepType": "hdfs", // plug-in name
            "parameter": {
                "path": ", // file path to read
                "datasource": "", //Name of the data source
                "column": [
                    {
                        "index": 0, //serial number
                        "type": "string" //Field Type
                    },
                    {
                        "index": 1,
                        "type": "long"
                    },
                    {
                        "index": 2,
                        "type": "double",
                    },
                    {
                        "index": 3,
                        "type": "boolean"
                    },
                    {
                        "format":"yyyy-MM-dd HH:mm:ss", // time format
                        "index": 4,
                        "type": "date",
                    }
                ],
                "fieldDelimiter": "," //Delimiter of each column
                "Encoding": "UTF-8", // encoding format
                "fileType": "// text type
            },
            "name": "Reader",
            "category": "reader"
        },
        {//The following is a writer template. You can find the corresponding writer plug-in documentations.
            "stepType": "stream",
            "parameter": {},
            "name": "Writer",
            "Category": "Writer"
        }
    ],
    "setting": {
        "errorLimit": {
            "record": ""//Number of error records
        },
        "speed": {
            "concurrent": "3",//Number of concurrent tasks
            "throttle": false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "dmu": 1 // DMU Value
        }
    },
    "order":{
        "hops":[
            {
                "from": "Reader",
                "to": "Writer"
            }
        ]
    }
}
```

