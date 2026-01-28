# Elasticsearch 压测工具之esrally

#### Elasticsearch 压测工具之esrally是ES官方基于Python开发的基准压测工具，主要应用于基准测试、版本升版基准评估等场景


安装esrally
```
环境要求：python3.10+、git 1.9+

安装esrally：pip3.11 install esrally
```

esrally基准压测
```
查看赛道类型与比赛类型："esrally list tracks"

输出格式：markdown(默认)、csv

#### 可以手动调整压测参数 ####

选择赛道类型：nested，比赛类型：nested-search-challenge，并输出至markdown
esrally race --pipeline=benchmark-only --target-hosts=192.168.1.30:9200 --track=nested --challenge=nested-search-challenge --client-options="basic_auth_user:'elastic',basic_auth_password:'123456'" --report-file=/root/esbench_result.md

跳过创建索引，只压测某一环节
esrally race --pipeline=benchmark-only --target-hosts=192.168.1.30:9200 --track=nested --challenge=nested-search-challenge --client-options="basic_auth_user:'elastic',basic_auth_password:'123456'" --include-tasks="randomized-nested-queries" --report-file=/root/esbench_result.md

跳过创建索引，只压测某一环节，并打上标签
esrally race --pipeline=benchmark-only --target-hosts=192.168.1.30:9200 --track=nested --challenge=nested-search-challenge --client-options="basic_auth_user:'elastic',basic_auth_password:'123456'" --include-tasks="randomized-nested-queries" --report-file=/root/esbench_result.md --user-tag="版本:8.19.8"

离线模式，跳过创建索引，只压测某一环节，并打上标签
esrally race --pipeline=benchmark-only --target-hosts=192.168.1.30:9200 --track=nested --challenge=nested-search-challenge --client-options="basic_auth_user:'elastic',basic_auth_password:'123456'" --include-tasks="randomized-nested-queries" --report-file=/root/esbench_result.md --user-tag="版本:8.19.8" --offline

比赛结果示例
    ____        ____
   / __ \____ _/ / /_  __
  / /_/ / __ `/ / / / / /
 / _, _/ /_/ / / / /_/ /
/_/ |_|\__,_/_/_/\__, /
                /____/

[INFO] Race id is [ab149902-156b-482b-a4d0-3c82056783ef]
[INFO] Racing on track [nested], challenge [nested-search-challenge] and car ['external'] with version [8.19.8].

Running randomized-nested-queries                                              [100% done]

------------------------------------------------------
    _______             __   _____
   / ____(_)___  ____ _/ /  / ___/_________  ________
  / /_  / / __ \/ __ `/ /   \__ \/ ___/ __ \/ ___/ _ \
 / __/ / / / / / /_/ / /   ___/ / /__/ /_/ / /  /  __/
/_/   /_/_/ /_/\__,_/_/   /____/\___/\____/_/   \___/
------------------------------------------------------
            
|                                                         Metric |                      Task |         Value |   Unit |
|---------------------------------------------------------------:|--------------------------:|--------------:|-------:|
|                     Cumulative indexing time of primary shards |                           |   0           |    min |
|             Min cumulative indexing time across primary shards |                           |   0           |    min |
|          Median cumulative indexing time across primary shards |                           |   0           |    min |
|             Max cumulative indexing time across primary shards |                           |   0           |    min |
|            Cumulative indexing throttle time of primary shards |                           |   0           |    min |
|    Min cumulative indexing throttle time across primary shards |                           |   0           |    min |
| Median cumulative indexing throttle time across primary shards |                           |   0           |    min |
|    Max cumulative indexing throttle time across primary shards |                           |   0           |    min |
|                        Cumulative merge time of primary shards |                           |   0           |    min |
|                       Cumulative merge count of primary shards |                           |   0           |        |
|                Min cumulative merge time across primary shards |                           |   0           |    min |
|             Median cumulative merge time across primary shards |                           |   0           |    min |
|                Max cumulative merge time across primary shards |                           |   0           |    min |
|               Cumulative merge throttle time of primary shards |                           |   0           |    min |
|       Min cumulative merge throttle time across primary shards |                           |   0           |    min |
|    Median cumulative merge throttle time across primary shards |                           |   0           |    min |
|       Max cumulative merge throttle time across primary shards |                           |   0           |    min |
|                      Cumulative refresh time of primary shards |                           |   0.0001      |    min |
|                     Cumulative refresh count of primary shards |                           | 148           |        |
|              Min cumulative refresh time across primary shards |                           |   0           |    min |
|           Median cumulative refresh time across primary shards |                           |   0           |    min |
|              Max cumulative refresh time across primary shards |                           |   5e-05       |    min |
|                        Cumulative flush time of primary shards |                           |   0.0158333   |    min |
|                       Cumulative flush count of primary shards |                           |  37           |        |
|                Min cumulative flush time across primary shards |                           |   0.000183333 |    min |
|             Median cumulative flush time across primary shards |                           |   0.00025     |    min |
|                Max cumulative flush time across primary shards |                           |   0.00355     |    min |
|                                        Total Young Gen GC time |                           |   0.17        |      s |
|                                       Total Young Gen GC count |                           |   3           |        |
|                                          Total Old Gen GC time |                           |   0           |      s |
|                                         Total Old Gen GC count |                           |   0           |        |
|                                                   Dataset size |                           |   1.01188     |     GB |
|                                                     Store size |                           |   1.01188     |     GB |
|                                                  Translog size |                           |   1.89524e-06 |     GB |
|                                         Heap used for segments |                           |   0           |     MB |
|                                       Heap used for doc values |                           |   0           |     MB |
|                                            Heap used for terms |                           |   0           |     MB |
|                                            Heap used for norms |                           |   0           |     MB |
|                                           Heap used for points |                           |   0           |     MB |
|                                    Heap used for stored fields |                           |   0           |     MB |
|                                                  Segment count |                           |  82           |        |
|                                    Total Ingest Pipeline count |                           |   0           |        |
|                                     Total Ingest Pipeline time |                           |   0           |      s |
|                                   Total Ingest Pipeline failed |                           |   0           |        |
|                                                 Min Throughput | randomized-nested-queries |  19.99        |  ops/s |
|                                                Mean Throughput | randomized-nested-queries |  20           |  ops/s |
|                                              Median Throughput | randomized-nested-queries |  20           |  ops/s |
|                                                 Max Throughput | randomized-nested-queries |  20.01        |  ops/s |
|                                        50th percentile latency | randomized-nested-queries |  18.4394      |     ms |
|                                        90th percentile latency | randomized-nested-queries |  35.9938      |     ms |
|                                        99th percentile latency | randomized-nested-queries | 115.486       |     ms |
|                                      99.9th percentile latency | randomized-nested-queries | 173.506       |     ms |
|                                       100th percentile latency | randomized-nested-queries | 204.004       |     ms |
|                                   50th percentile service time | randomized-nested-queries |  16.173       |     ms |
|                                   90th percentile service time | randomized-nested-queries |  32.5001      |     ms |
|                                   99th percentile service time | randomized-nested-queries |  86.1085      |     ms |
|                                 99.9th percentile service time | randomized-nested-queries | 121.043       |     ms |
|                                  100th percentile service time | randomized-nested-queries | 149.289       |     ms |
|                                                     error rate | randomized-nested-queries |   0           |      % |


---------------------------------
[INFO] SUCCESS (took 172 seconds)
---------------------------------
```

查看比赛记录
```
esrally list races
```

查看比赛间差异结果
```
esrally compare --baseline 0993c565-533c-455c-98f2-bbf9b4cab498 --contender bc501396-e11c-4036-9d37-25428b70ac82
```

查看某一赛道的比赛信息
```
[root@es-1 nested]# esrally info --track=nested --challenge=nested-search-challenge

    ____        ____
   / __ \____ _/ / /_  __
  / /_/ / __ `/ / / / / /
 / _, _/ /_/ / / / /_/ /
/_/ |_|\__,_/_/_/\__, /
                /____/

Showing details for track [nested]:

* Description: StackOverflow Q&A stored as nested docs
* Documents: 11,203,029
* Compressed Size: 663.3 MB
* Uncompressed Size: 3.4 GB

====================================================
Challenge [nested-search-challenge] (run by default)
====================================================

Indexes the document corpus for an hour using Elasticsearch default settings. After that randomized nested queries are run.

Schedule:
----------

1. delete-index
2. create-index
3. check-cluster-health
4. index-append (4 clients)
5. refresh-after-index
6. force-merge
7. refresh-after-force-merge
8. wait-until-merges-finish
9. randomized-nested-queries (2 clients)
10. randomized-term-queries (2 clients)
11. randomized-sorted-term-queries (2 clients)
12. match-all (2 clients)
13. nested-date-histo (2 clients)
14. randomized-nested-queries-with-inner-hits_default (2 clients)
15. randomized-nested-queries-with-inner-hits_default_big_size (2 clients)

-------------------------------
[INFO] SUCCESS (took 1 seconds)
-------------------------------
```

##### 至此，Elasticsearch 压测工具之esrally就介绍完成了，请开始你的表演吧！

&nbsp;

**有兴趣的小伙伴，可加联系方式：Telegram：@dean_code** 
