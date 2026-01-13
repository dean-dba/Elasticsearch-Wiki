# Elasticsearch 逻辑备份恢复之elasticdump

#### elasticdump 是基于node js开发的一种第三方逻辑备份恢复工具，主要应用于索引数据量小、跨集群、跨环境之间迁移数据，可以对单个索引、多个索引同时迁移备份

#### elasticdump 迁移类型：index, settings, analyzer, data, mapping, policy, alias, template, component_template, index_template

#### elasticdump 单索引工具：elasticdump，elasticdump 多索引工具：multielasticdump

elasticdump 安装
```
1.下载node js："wget https://nodejs.org/dist/latest/node-v25.2.1-linux-x64.tar.gz"

2.解压node js："tar -xvf node-v25.2.1-linux-x64.tar.gz"

3.永久生效：
vim ~/.bash_profile 
export PATH=$PATH:/root/node-v25.2.1-linux-x64/bin
source ~/.bash_profile

4.安装elasticdump："npm install elasticdump -g"
```

elasticdump 迁移数据示例
```
跨集群迁移
elasticdump --input=http://192.168.1.30:9200/test-400-20260110 --output=http://192.168.1.30:9200/test-400-20260112 --type=data --limit=5000 --concurrency=3

导出索引数据至本地
elasticdump --input=http://192.168.1.30:9200/test --output=/data/test.json --type=data --limit=5000 --concurrency=3

迁移固定数量索引数据
elasticdump --input=http://192.168.1.30:9200/test --output=192.168.1.30:9200/test_new --type=data --size=1000

后台账号密码迁移
elasticdump --input=http://192.168.1.30:9200/test --output=http://root:123456@192.168.1.30:9200/test_new --type=data --limit=1000

批量导出数据：multielasticdump --direction=dump --match='^test.*' --input=http://192.168.1.30:9200 --output=/root/multielasticdump/ --includeType=data,mapping,settings,template,alias,analyzer --limit=10000
批量导入数据：multielasticdump --direction=load --input=/root/multielasticdump --output=http://192.168.1.30:9200 --includeType=data,mapping,settings,template,alias,analyzer --limit=10000
```

##### 至此，Elasticsearch 逻辑备份恢复之elasticdump 就介绍完成了，请开始你的表演吧！

&nbsp;

**有兴趣的小伙伴，可加联系方式：Telegram：@dean_code**  
