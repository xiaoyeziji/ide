# Configure HBase11xsql Writer {#concept_kfw_ryl_q2b .concept}

In this article we will show you the data types and parameters supported by HBase11xsql Writer and how to configure Writer in script mode.

HBase11xsql Writer provides the function to import data in batch to an SQL table \(Phoenix\) in HBase.  The rowkey has been encoded by Phoenix. Therefore, you need to manually convert the data when you directly use HBase APIs for data writing, which is troublesome and error-prone. This plug-in provides a method for you to import data to a single SQL table.

At the underlying implementation level, the JDBC drive of Phoenix executes the UPSERT statement to write data to HBase.

## Supported functions {#section_k4h_tyl_q2b .section}

The writer supports importing data from an indexed table and simultaneously updating all indexed tables.

## Limits {#section_tmw_tyl_q2b .section}

The limitations of the glaswriter plug-in are shown below.

-   Only HBases of the 1.x version are supported.
-   Only tables created by Phoenix are supported. Native HBase tables are not supported.
-   Data with a timestamp cannot be imported.

## Implementation principles {#section_n4h_tyl_q2b .section}

The JDBC drive of Phoenix executes the UPSERT statement to write data in batch to a table.  Because an upper-layer API is used, the indexed tables can be updated simultaneously.

## Parameter description​ { .section}

|Attribute|Description|Required|Default Value|
|:--------|:----------|:-------|:------------|
|plugin:|Name of the plug-in, which must be hbase11xsql|Yes|None|
|table|Name of the table to be imported. The name is case sensitive and the name of Phoenix tables is generally in upper case.|Yes|None|
|column|Name of the column. The name is case sensitive  and the name of Phoenix tables is generally in upper case.**Note:** 

-   The column sequence must exactly correspond to the sequence of columns output by the reader.
-   The data type does not need to be entered, and the column metadata is automatically retrieved from Phoenix.

|Yes|None|
|  hbaseConfig|The address of the HBase cluster, in the format of ip1,ip2,ip3. The zk is required.**Note:** 

-   NOTE: Separate multiple IP addresses by commas \(,\).
-   znode is optional and the default value is /hbase.

|Yes|None|
|batchSize|Maximum number of rows written in bulk.|No|256|
|nullMode|Specifies the processing mode when the column value read is null. There are currently two methods:-   - skip: Skip this column. That is, this column is not inserted. If this column of the row already exists, the column is deleted.
-   - empty: Insert a null value. 0 is inserted for value of the numeric type and a null string is inserted for a varchar value.

 |No|skip|

## Development in script mode {#section_nlj_scm_q2b .section}

The script configuration example is as follows.

```
{
  "type": "job",
  "version": "1.0",
  "configuration": {
    "setting": {
      "errorLimit": {
        "record": "0"
      },
      "speed": {
        "mbps": "1",
        "concurrent": "1"
      }
    },
    "reader": {
      "plugin": "odps",
      "parameter": {
        "datasource": "",
        "table": "",
        "Column ":[
        "partition": ""
      }
    },
    "plugin": "hbase11xsql",
    "parameter": {
      "table": "Name of the target HBase table, which is case sensitive",
      "hbaseConfig": {
        "hbase.zookeeper.quorum": "Address of the ZK server of the target HBase cluster. Ask PE for the address",
        "zookeeper.znode.parent": "znode of the ZK server of the target HBase cluster. Ask PE for the znode"
      },
      "column": [
        "columnName"
      ],
      "batchSize": 256,
      "nullMode": "skip"
    }
  }
}
```

## Limits {#section_iqp_xcm_q2b .section}

The column sequence in the Writer must match that in the Reader. The column sequence in the Reader defines the organizational sequence of columns in each row.  The column sequence in the Writer defines the column sequence of the received data that is expected by the Writer. Example:

If the column sequence in the Reader is c1, c2, c3, c4,and the column sequence in the Writer is x1, x2, x3, x4, the column c1 output by the Reader is the column x1 in the Writer. If the column sequence in the Writer is x1, x2, x4, x3, c1 is assigned to x4 and c4 is assigned to x3.

## FAQ {#section_jqp_xcm_q2b .section}

**Q: How many concurrent settings are appropriate? Can I increase the concurrency to accelerate the import speed?**

A: The size of the default JVM stack for the data import process is 2 GB, and the concurrency \(number of channels\) is realized by multiple threads. Too many threads sometimes cannot accelerate the import speed but may result in performance deterioration due to frequent GC. A recommended concurrency \(number of channels\) is 5 to 10.

**Q: What should the batchSize value be?**

A: The default value is 256. You should set an appropriate batchSize according to the data volume in each row. Generally, the data volume at one operation is about 2 MB to 4 MB. You should divide this value by the data volume in the row and set the batchSize accordingly.

