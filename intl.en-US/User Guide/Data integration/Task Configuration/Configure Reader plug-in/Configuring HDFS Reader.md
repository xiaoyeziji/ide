# Configuring HDFS Reader {#concept_azr_4qj_p2b .concept}

HDFS Reader provides the ability to read the data stored by distributed file systems. At the underlying implementation level, HDFS Reader retrieves the file data on the distributed file system, coverts the data into Data Integration transport protocol, and transfers it to the Writer.

HDFS Reader provides the ability to read file data from the Hadoop distributed file system \(HDFS\) and coverts the data into Data Integration transport protocol.

For example:

TextFile is the default storage format for creating Hive tables without data compression. Essentially, TextFile stores data in HDFS as text, and the implementation of HDFS Reader is quite similar to that of OSS Reader for Data Integration. ORCFile refers to Optimized Row Columnar File, which is the optimized RCFile. This file format provides an efficient method for storing Hive data. HDFS Reader utilizes the OrcSerde class provided by Hive to read and parse the data of ORCFile files.

**Note:** For data synchronization, admin account and read/write permissions for the files are required,

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16224/15408689027725_en-US.png)

Usage:

-   Create an admin user and home directory, specify a user group and additional group, and grant the permissions for the files.

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

    When using hadoop commands, the format is `hadoop fs -command`, where command represents the command.

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

-   Connect to the host from the list and create an admin account on each attached host.

    ```
    pssh -h /home/hadoop/slave4pssh useradd -m -G supergroup -g hadoop -p admin admin
    ```

    -   `pssh -h /home/hadoop/slave4pssh`: connect to the host from the manifest file.
    -   `useradd -m -G supergroup -g hadoop -p admin admin`: Create admin account.

## Supported functions {#section_fbh_pqj_p2b .section}

Currently, HDFS Reader supports the following features:

-   Supports TextFile, ORCFile, rcfile, sequence file, csv, and parquet file formats, and what is stored in the file must be a two-dimensional table in a logic sense.
-   Supports reading multiple types of data \(represented by String\) and supports column pruning and column constants.
-   Supports recursive reading and regular expressions "\*" and "?".
-   Supports ORCFile data compression, and currently supports the SNAPPY and ZLIB compression modes.
-   Supports data compression for sequence files, and currently supports the lzo compression mode.
-   Supports concurrent reading of multiple files.
-   Supports the following compression formats for the csv type: gzip, bz2, zip, lzo, lzo\_deflate, and snappy.
-   In the current plugin, the Hive version is 1.1.1, and the Hadoop version is 2.7.1 \(Apache \[is compatible with JDK 1.6\]\). Data can be written normally in the testing environments of Hadoop 2.5.0, Hadoop 2.6.0, and Hive 1.2.0.

**Note:** Temporarily, HDFS Reader does not support the multi-thread concurrent reading of a single file, which involves the internal splitting algorithm of the single file.

## Supported data types {#section_mtj_dck_p2b .section}

**RCfile**

If the file type of the HDFS file being synchronized is rcfile, you must specify the data type of the column in the Hive table in "column type" because the data storage mode varies with the data type during rcfile underlying storage and the HDFS Reader does not support accessing and querying Hive metadata databases. If the column type is bigint, double, or float, enter bigint, double, or float accordingly as the data type. If the column type is varchar or char, enter string for the same purpose.

RCFile data types are converted into the internal types supported by Data Integration by default, as shown in the following comparison table:

|Type Classification|HDFS Data Type|
|:------------------|:-------------|
|Integer|Tinyint, smallint, Int, and bigint|
|Float|Float, double, decimal|
|String type|String, Char, and varchar|
|Date and time type|Date and timestamp|
|Boolean class|Boolean|
|Binary class|BINARY|

**Parquetfile**

ParquetFile data types are converted into the internal types supported by Data Integration by default, as shown in the following comparison table:

|Type Classification|HDFS Data Type|
|:------------------|:-------------|
|Integer|Int32, int64, and int96|
|Floating point|Float and double|
|String type|FIXED\_LEN\_BYTE\_ARRAY|
|Date and time type|Date and timestamp|
|Boolean|Boolean|
|Binary|BINARY|

**TextFile, ORCfile, and SequenceFile**

Given that the metadata of TextFile and ORCFile file tables is maintained by and stored in the database maintained by Hive itself \(such as MySQL\), HDFS Reader currently does not support the access and query to the Hive metadata database, so you must specify a data type for type conversion.

TextFile, ORCFile, and SequenceFile data types are converted into the internal types supported by Data Integration by default, as shown in the following comparison table:

|Category|HDFS Data Type|
|:-------|:-------------|
|Integer|Tinyint, smallint, Int, and bigint|
|Floating point|float and double|
|String type|String, Char, varchar, struct, MAP, array, union, binary|
|Date and time|date and timestamp|
|Boolean|Boolean|

Notes:

-   LONG: Represents with an integer string in the HDFS file, such as 123456789.
-   DOUBLE: Represents with a double string in the HDFS file, such as 3.1415.
-   BOOLEAN: Represents with a boolean string in the HDFS file, such as true or false \(case-insensitive\).
-   DATE: Represents with a date and time string in the HDFS file, such as 2014-12-31 00:00:00.

**Note:** The Timestamp data type supported by Hive can be accurate to nanoseconds, so the data content of Timestamp stored in TextFile and ORCFile can be in the format like "2015-08-21 22:40:47.397898389". If the converted data type is set as Date for Data Integration, the nanosecond part is truncated after conversion. If you want to retain the nanosecond part, set the converted data type as String for Data Integration.

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|path|Description: It refers to the file path to be read. If you want to read multiple files, use a regular expression to match all of them, such as /hadoop/data\_201704\*.-   If a single HDFS file is specified, HDFS Reader only supports single-threaded data extraction.
-   If multiple HDFS files are specified, HDFS Reader supports multiple-threaded data extraction, and the number of concurrent threads is determined by the job speed \(mbps\). The actual number of initiated concurrent threads is the smaller of the number of HDFS files to be read and the set job speed.

**Note:** The actual number of initiated concurrent threads is the smaller of the number of HDFS files to be read and the set job speed.

-   When the wildcard is specified, HDFS Reader attempts to traverse multiple files. For example: When "/" is specified, HDFS Reader reads all the files under the "/" directory. When "/bazhen/" is specified, HDFS Reader reads all the files under the bazhen directory. Currently, HDFS Reader only supports "\*" and "?"  as file wildcards, and the syntax is similar to that of the file wildcards of common Linux command lines.

**Note:** 

-   Data Integration regards all the files to be read in the same synchronization job as one data table.  For this reason, you must ensure that all those files adapt to the same schema information and grant the read permission to Data Integration.
-   **Note on reading partitions:** During Hive table creation, you can specify partitions. For example, after creating the partition\(day="20150820",hour="09"\), two directories with the name of /20150820 and /09 respectively are created in the table catalog of the HDFS file system and /20150820 is the parent directory of /09.

Given that partitions are organized in a directory structure, we can set the value of path in JSON when reading all the data of a table by partitions. For example, if you want to read all the data of the table named mytable01 on the partition day of 20150820, set as follows:

    ```
"path": "/user/hive/warehouse/mytable01/20150820/*"
    ```


|Yes|N/A|
|defaultFS|- Description: The namenode node address in HDFS.By default, the resource group does not support the configuration of HA, an advanced parameter of Hadoop.|Yes|N/A|
|fileType|- Description: File type. Currently, only text, orc, rc, seq, csv, and parquet are supported. HDFS reader automatically recognizes the type of file, and uses the corresponding file type read policy. Before synchronizing data, HDFS Reader checks whether the types of all the target files under the specified path are consistent with fileType. If not, the synchronization task fails.The list of parameter values that can be configured by fileType is as follows:

-   text: The format of TextFile.
-   orc: The format of ORCFile.
-   rc: The format of RCFile
-   seq: The format of sequence file
-   csv: The format of common HDFS file \(logical two-dimensional table\).
-   parquet: The format of common parquet file.

**Note:** Because TextFile and ORCFile are totally different file formats, HDFS Reader parses the two file types in different ways. For this reason, the formats of the converted results varies when complex compound types supported by Hive \(such as map, array, struct, and union\) are converted to the String type supported by Data Integration. The following takes use the map type as an example:

-   After being parsed and converted to the String type supported by Data Integration, the ORCFile map type is \{job=80, team=60, person=70\}.
-   After being parsed and converted to the String type supported by Data Integration, the TextFile map type is job:80,team:60,person:70.

From the preceding results, the data itself remains unchanged but the representation formats differ slightly. For this reason, if the fields to be synchronized under the file path configured are compound in Hive, we recommended that you set a unified format for the files.

**Recommended best practices:**

-   To unify the file types parsed from compound types, we recommended that you export TextFile tables as ORCFile tables on the Hive client.
-   If the file type is Parquet, the parquetSchema is required, which is used to describe the format of the Parquet file to be read.

For the specified column information, you must enter type and choose one from index/value.

|Yes|N/A|
|column|Description: It refers to the list of fields read, where type indicates the type of the source data, index indicates the number of the column in the text \(starts from 0\), and value indicates that the current type is constant, which means the corresponding column is automatically generated according to the value instead of the data read from the source file. By default, you can all read the data according to the string type, configured as `"column": ["*"]`.You also can configure the column field as follows:

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
|fieldDelimiter| Description: It refers to the field delimiter read. When HDFS Reader reads the TextFile data, a file delimiter is required, which defaults to ',' if no delimiter is specified. When HDFS Reader reads the ORCFile data, no field delimiter is required. The default delimiter of Hive is \\u0001. -   To use each row as a column of the target, use characters that are not included in the content of rows as the delimiter, such as the invisible characters \\u0001.
-   Additionally, \\n cannot be used as the delimiter.

|No|,|
|encoding|Description: Encoding of the read files.|No|UTF-8|
|nullFormat|Description: Defining null \(null pointer\) with a standard string is not allowed in text files. Data Integration provides nullFormat to define which strings can be expressed as null.For example, when nullFormat: "null" is configured, if the source data is "null", it is considered as a null field in Data Integration.

|No|N/A|
|compress|Description: It refers to the possible file compression formats when the fileType is csv, which currently support gzip, bz2, zip, lzo, lzo\_deflate, hadoop-snappy, and framing-snappy.**Note:** 

-   Two lzo compression formats are available: lzo and lzo\_deflate. In actual configuration scenarios, make sure to select the appropriate one.
-   Given that no unified stream format is now available to snappy, Data Integration currently only supports the most popular two compression formats: hadoop-snappy \(the snappy stream format in Hadoop\) and framing-snappy \(the snappy stream format recommended by Google\).
-   rc is the format of rcfile.
-   No entry is required for the orc file type.

|No|N/A|
|parquetSchema|Description: Required when the file is in parquet format. It is used to specify the structure of the target file, and takes effect only when the fileType is parquet. The format is as follows:```
message MessageType {
Required, data type, column name;
...................... ;
}
```

Notes:

-   MessageType: Any supported value
-   Required: Required or Optional. Optional is recommended. Optional is recommended.
-   Data Type: Parquet files support the following data types: boolean, int32, int64, int96, float, double, binary \(select binary if the data type is string\), and fixed\_len\_byte\_array.

**Note:** Note that each configuration row and column, including the last one, must end with a semicolon.

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
|csvReaderConfig|Description: Reads the parameter configurations of CSV files. It is the Map type. This reading is performed by the CsvReader for reading CSV files and involves many configuration items, whose defaults are used if they are not configured.Common configuration:

```
csvReaderConfig
  "safetySwitch": false,
  "skipEmptyRecords": false,
  "useTextQualifier": false
}
```

For all the configuration items and default values, you must configure the map of csvReaderConfig strictly in accordance with the following field names:

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
|hadoopConfig|Description: Some Hadoop-related advanced parameters can be configured in hadoopConfig, such as the configuration of HA.```
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

Development in wizard mode is not supported currently.

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

