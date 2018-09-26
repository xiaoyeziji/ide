# Structure {#concept_arb_4kp_1fb .concept}

The structure is based on the current Code, which parses the process diagram that runs under SQL, help users quickly review the edited SQL situation, so that it can be easily modified and viewed.

## Structure {#section_d1y_qkp_1fb .section}

As shown in SQL:

```
INSERT OVERWRITE TABLE dw_user_info_all_d PARTITION (dt='${bdp.system.bizdate}')
SELECT COALESCE(a.uid, b.uid) AS uid
  , b.gender
  , b.age_range
  , B. flavdiac
  , a.region
  , a.device
  , a.identity
  , a.method
  , a.url
  , a.referer
  , a.time
FROM (
  VALUES
  From fig
  WHERE dt = ${bdp.system.bizdate}
) a
LEFT OUTER JOIN (
  VALUES
  FROM ods_user_info_d
  WHERE dt = ${bdp.system.bizdate}
) b
on a.uid = b.uid ;
```

According to this Code, the structure is parsed:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20204/153716685711297_en-US.png)

When the mouse is placed in a circle, the corresponding explanation appears:

1.  Source table: the target table for the SELECT query.
2.  Filter: filters the specific partitions in the table that you want to query.
3.  In the first part of the intermediate table \(query view\): place the results of the query data into a temporary table.
4.  Join: mosaic the results of the two-part query through join.
5.  In the second section, the intermediate table \(the query view\): summarize the results of the join into a temporary table, this temporary table exists for three days and is automatically cleared three days later.
6.  Target table \(insert\): inserts the data obtained in the second part into the table in insert override.

