使用cURL导入数据
===

## 1. 下载数据文件

数据来源：https://raw.githubusercontent.com/madhusudhankonda/elasticsearch-in-action/main/datasets/movie_bulk_data.json
* 注意最后一个换行不能少 

## 2. cURL导入数据
```sh
curl -H "Content-Type: application/x-ndjson" -XPOST http://localhost:9200/_bulk --data-binary "@movie_bulk_data.json.txt"
```

## 3. Kibana Dev Console校验
```json
GET /_cat/shards?v

PUT /movies
{
  "settings": {
    "index": {
      "number_of_shards": 3,
      "number_of_replicas": 1
    }
  }
}

GET movies/_search

DELETE movies
```

