# Configure HDFS Writer {#concept_q3n_hdm_q2b .concept}

HDFS Writer is used to write TextFile, ORCFile, and ParquetFile to the specified path to HDFS. The files can be associated with Hive tables.  You must configure the data source before configuring the HDFS Writer plug-in. For more information, see [Configure the FTP data source](intl.en-US/User Guide/Data Integration/Data source configuration/Configure the FTP data source.md#)Configure the HDFS data source.

## How to implement HDFS Writer  {#section_dvy_43m_q2b .section}

The implementation process for HDFS writer is shown below.

1.  Create a temporary directory that does not exist in HDFS based on the path you specified. 

    Naming rule: path\_random

2.  Write the files that have been read to this temporary directory. 
3.  When all the files are written to the temporary directory, move these files to the directory you specified. The file names should be unique. 
4.  Delete the temporary directory.  If you are unable to connect to HDFS for reasons such as network interruption during the process, delete the temporary directory and the files written to it manually.

**Note:** For data synchronization, admin account and read/write permissions for the files are required.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16224/15367224447725_en-US.png)

as shown in the following figure:

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

    When using hadoop commands, the format is`hadoop fs -command`, where command represents the command.

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

## Functional restrictions {#section_fbh_pqj_p2b .section}

-   It only supports TextFile, ORCFile, and ParquetFile formats, and what is stored in the file must be a two-dimensional table in a logic sense.
-   HDFS is a file system and has no schema. Therefore, it does not support writing columns partially.
-   Only the following Hive data types are supported:
    -   Numeric: TINYINT, SMALLINT, INT, BIGINT, FLOAT, and DOUBLE
    -   String: STRING, VARCHAR, and CHAR
    -   Boolean: BOOLEAN
    -   Time type: date, timestamp.
-   Hive data types such as decimal, binary, arrays, maps, ovens, and union are not currently supported.
-   For Hive partition tables, the data can only be written to one partition at a time.
-   For the TextFile format, ensure the delimiters in the files written to HDFS are identical to the ones used in the tables created in Hive, so that the data written to HDFS is associated with the Hive table fields.
-   In the current plug-in, the Hive version is 1.1.1, and the Hadoop version is 2.7.1. Apache is compatible with JDK1.7.  Data can be written normally in the testing environments of Hadoop 2.5.0, Hadoop 2.6.0, and Hive 1.2.0.For other versions, further test is needed.

## Data type conversion {#section_acv_mlm_q2b .section}

Currently, HDFS Writer supports most data types in Hive. Check whether the Hive type you are using is supported.

HDFS Writer converts the data types in Hive as follows:

|Data Integration category|HDFS/Hive data type|
|:------------------------|:------------------|
|long|TINYINT,SMALLINT,INT,BIGINT|
|double|FLOAT,DOUBLE|
|string| STRING,VARCHAR,CHAR|
|boolean| BOOLEAN|
|date|DATE,TIMESTAMP|

## Parameter description​ {#section_jn2_gqh_p2b .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|defaultFS|Description: The namenode address in Hadoop HDFS, for example, `hdfs://127.0.0.1:9000`. The default resource group does not support the configuration of the advanced Hadoop parameter HA.|Yes|None|
|fileType|Description: File type. Currently, only text, orc, and parquet are supported.-   text: Indicates TextFile.
-   orc: Indicates ORCFile.
-   parquet: Indicates ParquetFile.

|Yes|None|
|path| Description: The path under which the files are written to Hadoop HDFS. HDFS Writer writes multiple files under the path based on the concurrent writing configurations.

 For association with a Hive table, enter the path under the Hive table stored in HDFS.  For example, if the path to the data warehouse set in Hive is `/user/hive/warehouse/`and you have created the database test table named hello, the path of the Hive table is `/user/hive/warehouse/test.db/hello` .

 |Yes|None|
|FileName|Description: Name of the file written by HDFS Writer. A random suffix is appended to the file name to form the actual name of the file written using each thread.|Yes|None|
|column|Description: Fields of the written data. Some columns cannot be written.For association with a Hive table, you must specify all the field names and types in the table, with name and type specifying the field name and field type respectively.

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

|Yes \(if filetype is parquet, this entry is not required\)|None|
|writeMode|Description: The mode in which HDFS Writer clears the existing data before data writing:-   append: The file is not processed before writing, and Data Integration HDFS Writer writes data directly using fileName without conflict of file names.
-   nonConflict: An error is reported if a file prefixed by fileName exists under the path directory.

**Note:** NOTE: Parquet files only support the nonConflict mode, and does not support the Append mode.

|Yes|None|
|fieldDelimiter|Description: The field delimiter used for the fields written by HDFS Writer. Ensure the field delimiter is identical to the one used in the Hive table created. Otherwise, you are unable to locate the data in the Hive table.|Yes. If the filetype is parquet, it is optional.|None|
|compress|Description: Compression type of HDFS files. It is left empty by default, which means no compression is performed.Text files support gzip and bzip2 compression types. Orc files support SNAPPY compression. SnappyCodec is needed.

|No|None|
|encoding|The encoding configuration for the Write File.|No|No compression|
|parquetSchema|Description: Required when the file is in parquet format. It is used to specify the structure of the target file, and takes effect only when the fileType is parquet.  The format is as follows:```
message MessageType {
Required, data type, column name;
...................... ;
}
```

Parameters:

-   MessageType: Any supported value
-   Required: Required or Optional. Optional is recommended.
-   Data Type: Parquet files support the following data types: boolean, int32, int64, int96, float, double, binary \(select binary if the data type is string\), and fixed\_len\_byte\_array.

**Note:** Each configuration row and column, including the last one, must end with a semicolon.

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

|否|N/A|

## Development in wizard mode {#section_bp2_wsh_p2b .section}

Development in wizard mode is not supported currently.

## Development in script mode {#section_cp2_wsh_p2b .section}

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

