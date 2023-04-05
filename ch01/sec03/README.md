# 查看ES集群状态

`ES`暴露HTTP RESTful API，可以通过常用工具进行访问：
* Kibana Dev Console
* Postman
* cURL

Kibana Dev Console：
* 最简单的访问ES API的方法
* 对查询结果进行格式化展示
* 自动设置必要的HTTP headers(Content-Type etc)
* 请求自动提示/补全
* 本课主要使用该工具

## 使用Kibana Dev Console查询集群状态

1. 查询集群健康状态
```
GET /_cluster/health
```
* `_cluster`表示API
* `health`表示命令

2. 查询节点状态
```
GET /_cat/nodes?v
```
`cat`表示`Compact and aligned text`

3. 查看索引情况
```
GET /_cat/indices?v
```

## 使用cUrl查询集群状态

```
curl -X GET http://localhost:9200
curl -X GET http://localhost:9200/_cluster/health

npm install -g json

curl -X GET http://localhost:9200/_cluster/health | json
```






