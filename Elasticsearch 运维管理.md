# Elasticsearch 运维管理

#### Elasticsearch 运维管理涉及日常运维、异常处理、运行监控等方面，本文针对日常管理命令，以及日常管理的基本概念进行说明，以方便大家能让ES稳定运行

```
查看已安装插件："GET /_cat/plugins"

验证分词是否已生效："POST _analyze  {"analyzer":"hanlp-index-yunyu","text":"aaaa多大图"}"

修改所有索引副本数："PUT _all/_settings { "index" : { "number_of_replicas" : 1 } }"

手动触发重试(默认重试5次失败后会停止自动分配，需手动触发重试)："POST _cluster/reroute?retry_failed=true"

查看阻塞的任务列表："GET _cat/pending_tasks"

查看正在运行的任务列表："1.获取任务列表：GET _tasks，2.查看某一任务ID详情：GET _tasks/task_id，取消某一任务：POST _tasks/task_id/_cancel"

查看实例默认参数："GET _cluster/settings?include_defaults=true"

修改索引副本数(默认：1)："PUT test-400-20260110/_settings {"index.number_of_replicas": 2}"

优雅下线节点(集群会自动迁移此节点上的索引至其它节点)："PUT _cluster/settings { "transient": { "cluster.routing.allocation.exclude._ip": "192.168.1.30" }}"  
  
修改集群重新分片索引任务数(默认：2)："PUT _cluster/settings { "transient": { "cluster.routing.allocation.cluster_concurrent_rebalance": 2}}"

修改节点并发恢复索引分片任务数(默认：2)："PUT _cluster/settings {"transient": { "cluster.routing.allocation.node_concurrent_recoveries": 2}}"

修改节点快照恢复速度(默认：40mb)："PUT _cluster/settings { "transient": { "indices.recovery.max_bytes_per_sec": "80mb" }}"

刷新事务日志："POST _flush"

清空节点堆内JVM缓存："POST _cache/clear"

设置总断路器大小(默认：堆内内存95%，断路器是防止节点内存OOM的保护机制，类似于电闸的保险丝)："PUT _cluster/settings { "transient": { "indices.breaker.total.limit": "95%" } }"
设置请求断路器大小(默认：堆内内存60%，请求断路器是指单个请求使用的最大堆内内存)："PUT _cluster/settings { "transient": { "indices.breaker.request.limit": "60%"} }"
设置Fielddata断路器大小(默认：堆内内存40%，Fielddata断路器是指对text文本字段进行聚合、排序的最大堆内内存)："PUT _cluster/settings { "transient": { "indices.breaker.fielddata.limit": "40%" } }"

索引数据迁移(内部实现：scroll+bulk)："POST _reindex {"source": {"index": "test-400"}, "dest": {"index": "test-400-new"} }"
```
