# Relationship {#concept_jb4_1cm_z2b .concept}

kinship relations show the relationships between the current node and other nodes. This relationship shows two parts: the dependency diagram and the internal relationship map.

## Dependency Graph {#section_flg_43p_1fb .section}

Depending on the dependency of the node, the dependency graph shows whether the dependency of the current node is what it expects, if not, you can return to the Schedule configuration interface to reset.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/19064/153899389711295_en-US.png)

## Internal relationship Map {#section_ucq_53p_1fb .section}

The internal relationship map is parsed Based on the node's code, for example:

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

According to this SQL, the following internal relationship map is resolved, parses an output table that will be used as a join mosaic to show the relationship relationship between the tables:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/19064/153899389811296_en-US.png)

