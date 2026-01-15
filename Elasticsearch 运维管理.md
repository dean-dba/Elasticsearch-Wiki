# Elasticsearch 运维管理

#### Elasticsearch 运维管理涉及日常运维、异常处理、运行监控等方面，本文针对日常管理命令，以及日常管理的基本概念进行说明，以方便大家能让ES稳定运行


查看已安装插件
```
GET /_cat/plugins
```

验证分词是否已生效
```
POST _analyze  {"analyzer":"hanlp-index-yunyu","text":"aaaa多大图"}
```

修改所有索引副本数
```
PUT _all/_settings { "index" : { "number_of_replicas" : 1 } }
```

手动触发重试(默认重试5次失败后会停止自动分配，需手动触发重试)
```
POST _cluster/reroute?retry_failed=true
```

查看阻塞的任务列表
```
GET _cat/pending_tasks
```

查看正在运行的任务列表：
```
"1.获取任务列表：GET _tasks
2.查看某一任务ID详情：GET _tasks/task_id
3.取消某一任务：POST _tasks/task_id/_cancel
```

查看实例默认参数
```
GET _cluster/settings?include_defaults=true
```

修改索引副本数(默认：1)
```
PUT test-400-20260110/_settings {"index.number_of_replicas": 2}
```

优雅下线节点(集群会自动迁移此节点上的索引至其它节点)
```
PUT _cluster/settings { "transient": { "cluster.routing.allocation.exclude._ip": "192.168.1.30" }}
```  
  
修改集群重新分片索引任务数(默认：2)
```
PUT _cluster/settings { "transient": { "cluster.routing.allocation.cluster_concurrent_rebalance": 2}}
```

修改节点并发恢复索引分片任务数(默认：2)
```
PUT _cluster/settings {"transient": { "cluster.routing.allocation.node_concurrent_recoveries": 2}}
```

修改节点快照恢复速度(默认：40mb)
```
PUT _cluster/settings { "transient": { "indices.recovery.max_bytes_per_sec": "80mb" }}
```

刷新事务日志
```
POST _flush
```

清空节点堆内JVM缓存
```
POST _cache/clear
```

断路器
```
设置总断路器大小(默认：堆内内存95%，断路器是防止节点内存OOM的保护机制，类似于电闸的保险丝)："PUT _cluster/settings { "transient": { "indices.breaker.total.limit": "95%" } }"
设置请求断路器大小(默认：堆内内存60%，请求断路器是指单个请求使用的最大堆内内存)："PUT _cluster/settings { "transient": { "indices.breaker.request.limit": "60%"} }"
设置Fielddata断路器大小(默认：堆内内存40%，Fielddata断路器是指对text文本字段进行聚合、排序的最大堆内内存)："PUT _cluster/settings { "transient": { "indices.breaker.fielddata.limit": "40%" } }"
```

索引数据迁移(内部实现：scroll+bulk)
```
POST _reindex {"source": {"index": "test-400"}, "dest": {"index": "test-400-new"} }
```

#### 主节点：负责管理集群状态、索引及分片分配，数据节点：负责数据存储、CRUD操作

#### 请求过程：查询分发阶段、取数据阶段

#### 检索重要指标：QPS、TPS、查询时间、写入时间

#### 更新文档内部实现流程：先逻辑删除(后台进行合并)、再过入

#### 磁盘内部警戒线：low(禁止分配新分片)：85%，high(开始迁移分片)：90%，flood_stage(洪水泛滥，实例只读)：95%

#### 集群状态：绿色(正常)、黄色(副本分片异常)、红色(主分片异常)

异常索引排查过程
```
1.查看异常索引列表："GET _cat/shards?v&h=node,index,shard,prirep,state,store,unassigned.reason,unassigned.details&s=sto,index"
2.查看异常索引具体报错："GET _cluster/allocation/explain {"index":"索引名称","shard":分片编号,"primary":是否是主分片}"
```

#### ES日志组件：Log4j2，文件名：log4j2.properties，日志级别(从低到高)：fatal->error->warn->info->debug->trace，默认级别：info

ES全局模板
```
PUT _template/global_template
{
	"index_patterns": "*",
	"order": 0,
	"settings": {
			"index.search.slowlog.threshold.query.warn":"300ms",
			"index.search.slowlog.threshold.query.info":"200ms",
			"index.search.slowlog.threshold.query.debug":"100ms",
			"index.search.slowlog.threshold.query.trace":"50ms",
			"index.search.slowlog.threshold.fetch.warn":"300ms",
			"index.search.slowlog.threshold.fetch.info":"200ms",
			"index.search.slowlog.threshold.fetch.debug":"100ms",
			"index.search.slowlog.threshold.fetch.trace":"50ms",
			"index.search.slowlog.level":"trace",
			"index.indexing.slowlog.threshold.index.warn":"300ms",
			"index.indexing.slowlog.threshold.index.info":"200ms",
			"index.indexing.slowlog.threshold.index.debug":"100ms",
			"index.indexing.slowlog.threshold.index.trace":"50ms",
			"index.indexing.slowlog.level":"trace",
			"index.indexing.slowlog.source":"1000",			
			"index.number_of_shards": "3",
			"index.number_of_replicas": "1",
			"index.refresh_interval": "30s",
			"index.translog.durability": "async",
			"index.translog.sync_interval": "30s",
			"index.translog.flush_threshold_size": "1gb",
			"index.merge.policy.segments_per_tier": 20,
			"index.merge.policy.max_merged_segment": "10gb",
            "index.routing.allocation.total_shards_per_node": 2
		}
}
```

