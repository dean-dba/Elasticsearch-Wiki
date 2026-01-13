# Elasticsearch 快照

#### ES 快照是ES内置的一种物理备份恢复的功能，快照采用数据段唯一存储和每个快照保留完整文件引用清单的方式，在存储和效率之间平衡，通过内置SLM(snapshot lifecycle management)快照生命周期管理对快照进行自动备份与快照保留周期管理

#### ES 备份恢复方法：物理快照、reindex(同集群、跨集群)、elasticdump(同集群、跨集群)、Logstash

#### ES 8.0默认情况下索引已存在时恢复会报错，受参数"action.destructive_requires_name": true影响

创建ES 快照步骤
```
1.配置快照共享存储库(oss、nfs)

2.在集群节点中配置快照存储库：elasticsearch.yml中添加：path.repo: ["/opt/elasticsearch-8.19.8/backup"]

3.创建快照存储库
PUT _snapshot/es-backup
{
  "type": "fs",
  "settings": {
    "location": "/opt/elasticsearch-8.19.8/backup"
  }
}

查看注册存储库："GET _snapshot"

4.创建快照："PUT _snapshot/es-backup/snapshot-20260112"

5.查看当前仓库正在运行的快照："GET _snapshot/es-backup/_current"
```

恢复ES 快照步骤
```
恢复全量快照："POST _snapshot/es-backup/snapshot-20260112/_restore"

恢复部分快照：
POST _snapshot/es-backup/snapshot-20260112/_restore
{
  "indices": "test*",
  "ignore_unavailable": true,
  "include_global_state": false,
  "rename_pattern": "test(.+)",
  "rename_replacement": "restored_test_$1"
}
```
```
删除快照："DELETE _snapshot/es-backup/snapshot-20260112"

查看某一快照状态："GET _snapshot/es-backup/snapshot-20260112/_status"
```
```
SLM(snapshot lifecycle management)快照生命周期管理：时间是UTC时间，如果想设置北京时间需要"-8"

创建SLM：
PUT _slm/policy/daily_snapshot_policy
{
  "name": "<snapshot-{now/d}>",
  "schedule": "0 30 17 * * ?",
  "repository": "es-backup",
  "config": {
    "ignore_unavailable": true,
    "include_global_state": true,
    "feature_states": []
  },
  "retention": {
    "expire_after": "7d",
    "min_count": 7,
    "max_count": 8
  }
}

手动执行SLM："POST _slm/policy/daily_snapshot_policy/_execute"

查看SLM设置："GET _slm/policy/daily_snapshot_policy"
```
