# Incremental data synchronization {#concept_m1y_2y3_r2b .concept}

## The two data types for synchronization {#section_ofy_zz3_r2b .section}

Based on whether the data is edited after writing, the data for synchronization is classified as unedited data \(generally the log data\), and edited data, such as changes in the personnel status in the personnel table.

## Example {#section_pfy_zz3_r2b .section}

You must specify different synchronization policies for each data entry. The following example shows how to synchronize the RDS database data with MaxCompute, the method is applicable to other data sources.

According to idempotence policies, the same task operation performed multiple times will consistently generate the same result. In this way, the task supports re-running scheduling and can easily clear dirty data when an error occurs. The data is imported to a separate table or partition, or overwrites the historical data in the existing table or partition.

In the example, the task test date is 11/14/2016, and ull synchronization is performed on the same day. The historical data is synchronized to the partition ds=20161113. For the incremental synchronization scenario, the automatic scheduling is configured to synchronize the incremental data to the partition ds=20161114 created on November 15, 2016. The time field optime indicates the modified data time, which is used to determine whether the data is incremental or not.

## Incremental synchronization of unchanged data {#section_b2h_l1j_r2b .section}

This scenario allows you to easily partition the data generation pattern because the data remains unchanged after generation. Typically, you can partition by date, such as creating one partition each day.

Data preparation

```
drop table if exists oplog;
create table if not exists oplog(
optime DATETIME,
uname varchar(50),
action varchar(50),
status varchar(10)
 );
Insert into oplog values(str_to_date('2016-11-11','%Y-%m-%d'),'LiLei','SELECT','SUCCESS');
Insert into oplog values ("2016-11-12 ', '% Y-% m-% d''),' hanmm ', 'desc ', "success ');
```

The two data entries in the historical data are available. You must perform full data synchronization before synchronizing the historical data to the partition created from yesterday.

Procedure

1.  Create a MaxCompute table.

    ```
    Create a good maxcompute table and partition by day
    create table if not exists ods_oplog(
     optime datetime,
     uname string,
     action string,
     status string
    ) partitioned by (ds string);
    ```

2.  Configure a task to synchronize the historical data.

    Only one test is required because the task is performed one-time only. After the test is complete, change the task status to Pause \(in the far-right scheduling configuration\), submit, and release the task again in the “Data Development” module to prevent the task from being scheduled automatically.

3.  Write more data to the RDS source table as incremental data.

    ```
    Insert into oplog values (current_date, "Jim", "Update", "success ');
    insert into oplog values(CURRENT_DATE,'Kate','Delete','Failed'); 
    insert into oplog values(CURRENT_DATE,'Lily','Drop','Failed');
    ```

4.  Configure a task to synchronize the incremental data.

    **Note:** If you configure “Data Filtering”, all data added to the source table on November 14 is retrieved and synchronized to the incremental partition in the target table during synchronization the next day on November 15.

5.  View synchronization results.

    If you set the task scheduling cycle as daily scheduling, the task is scheduled automatically the next day after the task is submitted and released. The following are data changes in the MaxCompute target table, once the task runs successfully.


## Incremental synchronization of edited data {#section_p3y_32j_r2b .section}

For data in personnel or order tables that are subject to changes, the full data synchronization on a daily basis is recommended based on the time variant collection feature of the data warehouse. In other words, you store full data daily. In this way, both historical and current data can be retrieved easily.

In actual scenarios, daily incremental synchronization may be required. Because MaxCompute does not support editing data with the Update statement, you must implement synchronization with other measures. The following describes how to implement incremental and full synchronization.

Data preparation

```
drop table if exists user ;
create table if not exists user(
    uid int,
    uname varchar(50),
    deptno int,
    gender VARCHAR(1),
    optime DATETIME
    );
-- Historical data
insert into user values (1,'LiLei',100,'M',str_to_date('2016-11-13','%Y-%m-%d'));
insert into user values (2,'HanMM',null,'F',str_to_date('2016-11-13','%Y-%m-%d'));
insert into user values (3,'Jim',102,'M',str_to_date('2016-11-12','%Y-%m-%d'));
insert into user values (4,'Kate',103,'F',str_to_date('2016-11-12','%Y-%m-%d'));
insert into user values (5,'Lily',104,'F',str_to_date('2016-11-11','%Y-%m-%d'));
Incremental data
update user set deptno=101,optime=CURRENT_TIME where uid = 2; -- Change null to non-null
update user set deptno=104,optime=CURRENT_TIME where uid = 3; -- Change non-null to non-null
update user set deptno=104,optime=CURRENT_TIME where uid = 4; -- Change non-null to null
delete from user where uid = 5;
insert into user(uid,uname,deptno,gender,optime) values (6,'Lucy',105,'F',CURRENT_TIME);
```

Daily full synchronization

1.  Create a MaxCompute table

    ```
    Daily full synchronization is relatively simple.
    create table ods_user_full(
        uid bigint,
        uname string,
        deptno bigint,
        gender string,
        Optime datetime 
    ) partitioned by (ds string);ring);
    ```

2.  Configure full synchronization tasks.

    **Note:** Set the task scheduling cycle as the daily scheduling because daily full synchronization is required.

3.  Test the task and view the synchronized MaxCompute target table.

    When full synchronization is performed on a daily basis, no incremental synchronization is performed, you can view the following data results after the task is automatically scheduled the next day.

    To query data results, set `where ds = '20161114'` to retrieve the full data.


Daily incremental synchronization

This mode is not recommended except in specific scenarios. Because the delete statement is not supported in specific scenarios, deleted data cannot be retrieved by filtering SQL statement conditions. Generally, enterprise codes are deleted logically, in which the update statement is applied instead of the delete statement. In scenarios where this method is inapplicable, using this sync method may cause data inconsistency when a special condition is encountered. Another drawback is that you must merge new and historical data after the synchronization.

Data preparation

Create two tables, in which one is for writing the latest data and the other is for writing incremental data.

```
-- Result table
create table dw_user_inc(
    uid bigint,
    uname string,
    deptno bigint,
    gender string,
    optime DATETIME 
);
-- Incremental record
create table ods_user_inc(
    uid bigint,
    uname string,
    deptno bigint,
    gender string,
    optime DATETIME 
)
```

1.  Configure a task to write full data directly to the result table.

    **Note:** Run this task only once and set the task as Paused in the **Data Development** module after the task runs successfully.

2.  Configure a task to write incremental data to the incremental record.
3.  Merge the data.

    ```
    insert overwrite table dw_user_inc 
    select 
    case when b.uid is not null then b.uid else a.uid end as uid,
    Case when B. uid is not null then B. uname else A. uname end as uname,
    case when b.uid is not null then b.deptno else a.deptno end as deptno,
    case when b.uid is not null then b.gender else a.gender end as gender,
    case when b.uid is not null then b.optime else a.optime end as optime
    from 
    dw_user_inc a 
    full outer join ods_user_inc b
    on a.uid = b.uid ;
    ```


As you can see in the preceding figure, the deleted data entries cannot be synchronized.

The daily incremental synchronization is different from the daily full synchronization in that the daily incremental synchronization synchronizes only a small amount of incremental data, but with risk of data inconsistency , extra computing workload for data merging is required..

If it is unnecessary, change the data volume synchronized throughout the day. In addition, you can set a lifecycle for the historical data, which can be deleted automatically after a certain period.

