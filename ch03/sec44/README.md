# 索引模版(Index Template)

- 定义 settings + mappings
- 创建索引时，如果索引的名称满足模版中定义的索引名模式[app-logs-*]，那么这个模版就会应用到这个索引上
- 索引模版只适用于新创建的索引

按月创建新索引

- http-logs-2023-01
- http-logs-2023-02
- http-logs-2023-03
- http-logs-2023-04
- ...

按日创建新索引

- http-logs-2023-01-01
- http-logs-2023-01-02
- http-logs-2023-01-03
- http-logs-2023-01-04
- ...

```json
PUT /_template/http-logs
{
  "index_patterns": ["http-logs-*"],
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 2
  },
  "mappings": {
    "properties": {
      "@timestamp": {
        "type": "date"
      },
      "request.path": {
        "type": "keyword"
      },
      "request.method": {
        "type": "keyword"
      },
      "request.body": {
        "type": "text"
      },
      "response.status": {
        "type": "integer"
      },
      "response.body": {
        "type": "text"
      }
    }
  }
}

GET /_template/http-logs

PUT /http-logs-2023-02-10

GET /http-logs-2023-02-10


DELETE /_template/http-logs
```
