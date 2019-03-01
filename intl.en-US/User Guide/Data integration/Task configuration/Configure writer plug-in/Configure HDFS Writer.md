# Configure HDFS Writer {#concept_q3n_hdm_q2b .concept}

This topic describes the data types and parameters supported by HDFS Writer and how to configure the Writer in Script Mode.

The HDFS Writer is used to write TextFile, ORCFile, and ParquetFile to the specified path in HDFS. The files can be associated with Hive tables. You must configure the data source before configuring the HDFS Writer plug-in. For more information, see [Configure FTP data source](reseller.en-US/User Guide/Data integration/Data source configuration/Configure FTP data source.md#).

## How to implement HDFS Writer  {#section_dvy_43m_q2b .section}

The implementation process for HDFS Writer as follows:

1.  Create a temporary directory that does not exist in HDFS based on the specified path. 

    Naming rule: path\_random

2.  Write files that have been read to this temporary directory. 
3.  When all files are written to the temporary directory, move these files to the directory you specified. The file names should be unique. 
4.  Delete the temporary directory. If you are unable to connect to HDFS because of network interruptions or other reasons, delete the temporary directory and the files written to it manually.

**Note:** For data synchronization, admin account, and read/write permissions for the files are required.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16224/15514328257725_en-US.png)

As shown in the preceding figure:

-   Create an admin user and home directory, specify a user group and additional group, and grant the files permissions.

    ```
    useradd -m -G supergroup -g hadoop -p admin admin 
    ```

    -   `-G supergroup`: Specifies the additional group to which the user belongs.
    -   `-g hadoop`: Specifies the user group to which the user belongs.
    -   `-p admin admin`: Add a password to the admin user.
-   View the files contents in this directory.

    ```
    hadoop fs -ls /user/hive/warehouse/hive_p_partner_native
    ```

    When using Hadoop commands, the format is`hadoop fs -command`, where command represents the command.

-   Copy the file part-00000 to the local file system.

    ```
    hadoop fs -get /user/hive/warehouse/hive_p_partner_native/part-00000
    ```

-   Edit the copied file.

    ```
    vim part-00000
    ```

-   Exit the current user.

    ```
    exit
    ```

-   Connect to the host from the list and create an admin account for each attached host.

    ```
    pssh -h /home/hadoop/slave4pssh useradd -m -G supergroup -g hadoop -p admin admin
    ```

    -   `pssh -h /home/hadoop/slave4pssh`: Connect to the host from the manifest file.
    -   `useradd -m -G supergroup -g hadoop -p admin admin`: Create an admin account.

## Functional restrictions {#section_fbh_pqj_p2b .section}

-   HDFS Writer only supports TextFile, ORCFile, and ParquetFile formats. Content stored in the file must be a two-dimensional table in a logic sense.
-   HDFS is a file system with no schema. Therefore, it does not support writing columns partially.
-   Only the following Hive data types are supported:
    -   Numeric: TINYINT, SMALLINT, INT, BIGINT, FLOAT, and DOUBLE
    -   String: STRING, VARCHAR, and CHAR
    -   Boolean: BOOLEAN
    -   Time type: date, timestamp.
-   Currently, Hive data types such as Decimal, Binary, Arrays, Maps, Structs, and Union are not supported.
-   For Hive partition tables, the data can only be written to one partition at a time.
-   For the TextFile format, ensure delimiters in the files written to HDFS are identical to those used in the tables created in Hive, so the data written to HDFS is associated with the Hive table fields.
-   In the current plug-in, the Hive version is 1.1.1 and the Hadoop version is 2.7.1. Apache is compatible with JDK1.7. Data can be written normally in the testing environments of Hadoop 2.5.0, Hadoop 2.6.0, and Hive 1.2.0. For other versions, further tests are needed.

## Data type conversion {#section_acv_mlm_q2b .section}

Currently, HDFS Writer supports most Hive data types. Check whether the Hive type is supported.

HDFS Writer converts Hive data types as follows:

|Data Integration category|HDFS/Hive data type|
|:------------------------|:------------------|
|long|TINYINT,SMALLINT,INT,BIGINT|
|double|FLOAT,DOUBLE|
|string|STRING,VARCHAR,CHAR|
|boolean|BOOLEAN|
|date|DATE,TIMESTAMP|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|defaultFS|The namenode address in Hadoop HDFS, for example: `hdfs://127.0.0.1:9000`. The default resource group does not support the configuration of the advanced Hadoop parameter HA.|Yes|None|
|fileType|The file type. Currently, only text, orc, and parquet are supported.-   text: Indicates the TextFile.
-   orc: Indicates the ORCFile.
-   parquet: Indicates the ParquetFile.

|Yes|None|
|path| The path in which the files are written to Hadoop HDFS. The HDFS Writer writes multiple files under the path based on the concurrent writing configurations.

 To associate a Hive table, enter the path of the Hive table stored in HDFS. For example, if the path to the data warehouse set in Hive is `/user/hive/warehouse/`and the created database test table is named hello, the Hive table path is `/user/hive/warehouse/test.db/hello`.

 |Yes|None|
|FileName|The file name written by HDFS Writer. A random suffix is appended to the file name to form the actual file name written with each thread.|Yes|None|
|column|The fields of the written data. Some columns cannot be written.To associate a Hive table, you must specify all field names and table types, and specify name and type of the field name and type respectively.

You can configure the column field as follows:

```
"column": 
[
    {
        "name": "userName",
        "type": "string"
    },
    {
        "name": "age",
        "type": "long"
    }
]
```

|Yes. If the filetype is parquet, this entry is not required.|None|
|writeMode|The mode in which the HDFS Writer clears existing data before writing data:-   append: The file is not processed before writing, and the Data Integration HDFS Writer writes data directly using fileName without conflict in file names.
-   nonConflict: An error is reported, if a file prefixed by fileName exists under the path directory.

**Note:** Parquet files only support nonConflict mode, and does not support the Append mode.

|Yes|None|
|fieldDelimiter|The field delimiter used for the fields written by HDFS Writer. Ensure the field delimiter is identical to the one used in the Hive table created. Otherwise, you are unable to locate data in the Hive table.|Yes. If the filetype is parquet, it is optional.|None|
|compress|The compression type of HDFS files. By default, it is left empty , which means no compression is performed.Text files support gzip and bzip2 compression types. Orc files support SNAPPY compression and requires SnappyCodec.

|No|None|
|encoding|The encoding configuration for the Write File.|No|No compression|
|parquetSchema|This parameter is required for parquet format files and is used to specify the structure of the target file. This parameter takes effect only when the fileType is parquet. The format is as follows:```
message MessageType {
Required, data type, column name;
...................... ;
}
```

Parameters:

-   MessageType: Any supported value.
-   Required: Required or Optional. Optional is recommended.
-   Data Type: Parquet files support the following data types: BOOLEAN, Int32, Int64, Int96, FLOAT, DOUBLE, BINARY \(select binary if the data type is string\), and fixed\_len\_byte\_array.

**Note:** All configuration rows and columns, including the last one must end with a semicolon.

Example:

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

## Development in Wizard Mode {#section_bp2_wsh_p2b .section}

Currently, development in Wizard Mode is not supported.

## Development in Script Mode {#section_cp2_wsh_p2b .section}

The script configuration example is as follows, please refer to the above parameter descriptions for details.

```
{
    "type": "job",
    "version": 2.0 ", // version number
    "steps": [
        {//The following is a reader template. You can find the corresponding reader plug-in documentations.
            "stepType": "stream",
            "parameter": {},
            "name": "Reader",
            "category": "reader"
        },
        {
            "stepType": "hdfs", // plug-in name
            "parameter": {
                "path:"", // path information stored to hadoop HDFS File System
                "fileName:" ",/HDFS writer file name when writing
                "compress": "", // HDFS File compression type
                "datasource": "", //Name of the data source
                "column":[
                    {
                        "name": "col1", // field name
                        "type": "string" // Field Type
                    },
                    {
                        "name": "col2",
                        "type": "int"
                    },
                    {
                        "name": "col3",
                        "type": "double"
                    },
                    {
                        "name": "col4",
                        "type": "boolean"
                    },
                    {
                        "name": "col5",
                        "type": "date",
                    }
                ],
                "writeMode": "insert",//Write mode
                "fieldDelimiter": "," //Delimiter of each column
                "Encoding": "UTF-8", // encoding format
                "fileType": "text" // text type
            },
            "name": "Writer",
            "category": "writer"
        }
    ],
    "setting": {
        "errorLimit": {
            "record": "0"//Number of error records
        },
        "speed": {
            "concurrent": "3",//Number of concurrent tasks
            "throttle":false,//False indicates that the traffic is not throttled and the following throttling speed is invalid. True indicates that the traffic is throttled.
            "dmu": 1 // DMU Value
        }
    },
    "order": {
        "hops": [
            {
                "from": "Reader",
                "to": "Writer"
            }
        ]
    }
}
```

