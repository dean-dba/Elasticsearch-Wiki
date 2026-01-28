# Elasticsearch esrally

#### Elasticsearch esrally是ES官方基于Python开发的基准压测工具，主要应用于基准测试、版本升版基准评估等场景


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
```


查看比赛结果
```
esrally list races
```

查看比赛间差异结果
```
compare --baseline 0993c565-533c-455c-98f2-bbf9b4cab498 --contender bc501396-e11c-4036-9d37-25428b70ac82
```
