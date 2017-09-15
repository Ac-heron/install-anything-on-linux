# 安装Elasticsearch

- 下载：https://www.elastic.co/downloads/elasticsearch
- 解压：unzip elasticsearch-5.6.0.zip
- 进入：cd elasticsearch-5.6.0
- 启动：./bin/elasticsearch
- 后台启动：./bin/elasticsearch -d
- 测试是否启动成功：浏览器访问：http://localhost:9200/
或者命令： curl -XGET 'localhost:9200/?pretty'
返回：
```
{
  "name" : "EISZGJl",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "ancyTHvYTXe1pKsoX08VPA",
  "version" : {
    "number" : "5.6.0",
    "build_hash" : "781a835",
    "build_date" : "2017-09-07T03:09:58.087Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.0"
  },
  "tagline" : "You Know, for Search"
}
```

- 配置见：config/elasticsearch.yml

参考：
https://www.elastic.co/guide/en/elasticsearch/reference/5.6/zip-targz.html
