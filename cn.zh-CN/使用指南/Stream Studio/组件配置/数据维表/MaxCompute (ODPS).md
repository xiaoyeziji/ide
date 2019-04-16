# MaxCompute \(ODPS\) {#concept_pfl_fzf_xgb .concept}

更多信息请参考[创建MaxCompute（ODPS）维表](../../../../../cn.zh-CN/使用指南/Flink SQL/DDL语句/创建数据维表/创建MaxCompute（ODPS）维表.md#)。

## 配置面板说明 {#section_pvn_gzf_xgb .section}

|参数|注释说明|备注|
|--|----|--|
|血缘表名\(任务唯一\)|任务中表的唯一标志，不能与任务中其他表血缘表名重名|无|
|表名|表名|无|
|project|项目|无|
|accessId|用户访问ID|无|
|accessKey|用户访问KEY|无|
|和元数据中心同步|从元数据中心读取指定表名的元数据帮助填充输出字段和其他元信息表单项|无|
|选择输出字段|从维表中读取输出到下游组件的字段列表|无|
|分区名|分区名|无|
|加载最大表限制|加载最大表限制|加载odps表最大记录数|
|缓存策略|缓存策略|可选None、LRU或ALL|
|缓存大小|缓存大小|当选择`LRU`缓存策略后，可以设置缓存大小，ODPS默认缓存值为100000行。|
|缓存超时时间|缓存超时时间，单位毫秒|当选择`LRU`缓存策略后，可以设置缓存失效的超时时间，默认为不过期。当选择 `ALL`策略，则为缓存reload的间隔时间，默认为不重新加载。|
|更新时间黑名单|ALL Cache时启用，更新时间黑名单，防止在此时间内做cache更新|可选，默认为空，格式为```
2017-10-24 14:00 -> 2017-10-24 15:00, 2017-11-10 23:30 -> 2017-11-11 08:00
```

。用逗号`,`来分隔多个黑名单，用箭头`->`来分割黑名单的起始结束时间。|
|cacheScanLimit|ALL Cache 时启用，load全量HBase数据，服务端一次RPC返回给客户端的行数|无|
|primaryKey|指定输出字段中作为主键的字段|无|

## 类型映射 {#section_h3d_3zf_xgb .section}

|MaxCompute|Blink|
|----------|-----|
|TINYINT|TINYINT|
|SMALLINT|SMALLINT|
|INT|INT|
|BIGINT|BIGINT|
|FLOAT|FLOAT|
|DOUBLE|DOUBLE|
|BOOLEAN|BOOLEAN|
|DATETIME|TIMESTAMP|
|TIMESTAMP|TIMESTAMP|
|VARCHAR|VARCHAR|
|STRING|STRING|
|DECIMAL|DECIMAL|
|BINARY|VARBINARY|

## METRIC {#section_pjb_jzf_xgb .section}

维表JOIN可以查看关联率，缓存命中率等METRIC信息。可以通过kmonitor查看。

|查询语句|说明|
|----|--|
|fetch qps|查询维表总qps，包括命中和不命中。对应的Metric Name：`blink.projectName.jobName.dimJoin.fetchQPS`。|
|fetchHitQPS|维表命中qps，包括换成命中和查询物理维表命中，对应Metric Name：`blink.projectName.jobName.dimJoin.fetchHitQPS`。|
|cacheHitQPS|维表缓存命中qps，对应Metric Name：`blink.projectName.jobName.dimJoin.cacheHitQPS`|
|dimJoin.fetchHit|维表关联率，对应Metric Name：`blink.projectName.jobName.dimJoin.fetchHit`。|
|dimJoin.cacheHit|维表缓存关联率，对应Metric Name：`blink.projectName.jobName.dimJoin.cacheHit`。|

## 说明 {#section_fyw_lzf_xgb .section}

-   推荐实时计算2.1.1及以上版本使用。
-   使用MaxCompute表作为维表，要先赋权读权限给MaxCompute账号。
-   声明维表时，必须要指名主键，维表JOIN的时候，ON的条件必须包含所有主键的等值条件。
-   MaxCompute维表主键必须有唯一性，否则会被去重。
-   如果是分区表，目前不支持将分区列写入到schema定义中。
-   使用ALL Cache的时候，由于会进行异步Reload，需要将维表JOIN的节点增加内存，增加的内存大小为远程表两倍的数据量。
-   任务如果出现

    ```
    RejectedExecutionException: Task
              java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTas
    ```

    的failover，通常是由于实时计算1.x版本中维表JOIN存在一定的问题，推荐升级到2.1.1及以上实时计算版本。如果需要继续使用原有版本，需要对作业进行暂停恢复操作，根据failover history中第一次出现failover的具体报错信息进行排查。


