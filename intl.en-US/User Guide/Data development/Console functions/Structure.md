# Structure {#concept_arb_4kp_1fb .concept}

The structure is based on the current Code, which parses the process diagram that runs under SQL, helps users quickly review the edited SQL situation, so that it can be easily modified and viewed.

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

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20204/155193766211297_en-US.png)

When the mouse is placed in a circle, the corresponding explanation is displayed:

1.  Source table: The target table for the SELECT query.
2.  Filter: Filters the specific partitions in the table that you want to query.
3.  In the first part of the intermediate table \(query view\): Place the query data results into a temporary table.
4.  Join: The mosaic of the results in the two-part query through join.
5.  In the second section, the intermediate table \(the query view\): Summarizes the results of join in a temporary table. This temporary table exists for three days and is automatically cleared three days later.
6.  Target table \(insert\): Inserts data obtained in the second part of the table in insert override.

