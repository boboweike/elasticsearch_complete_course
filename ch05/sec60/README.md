# Query vs Filter Context

```json
GET /_cat/indices?

DELETE /movies
```

## 1. 导入 movies 数据集

### 1.1 添加 movies 索引

```json
PUT movies
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "fields": {
          "original": {
            "type": "keyword"
          }
        }
      },
      "synopsis": {
        "type": "text"
      },
      "actors": {
        "type": "text"
      },
      "director": {
        "type": "text"
      },
      "rating": {
        "type": "half_float"
      },
      "release_date": {
        "type": "date",
        "format": "dd-MM-yyyy"
      },
      "certificate": {
        "type": "keyword"
      },
      "genre": {
        "type": "text"
      }
    }
  }
}

```

### 1.2 下载 movie_bulk_data.json 到本地

https://github.com/boboweike/elasticsearch_complete_course/blob/main/ch05/sec60/movie_bulk_data.json
**注意最后有一个换行**

### 1.3 命令行执行导入

`curl -H "Content-Type: application/x-ndjson" -XPOST http://localhost:9200/_bulk --data-binary "@movie_bulk_data.json"`

### 1.4 校验

```json
GET /movies/_search
```

## 2. Query vs Filter Context

- ES 在执行查询时内部会使用一个执行上下文(Execution Context)
- Query Context 会计算查询结果的匹配度(具有 score)
- Filter Context 只检查是否匹配，不计算匹配度(score)
- Filter Context 具有更好的性能
- 所有的 ES 查询都属于两个 Context 之一
- ES 会根据你的查询自动判断 Query or Filter Context

### 2.1 Query Context 例子

```json
GET movies/_search
{
  "query": {
    "match": {
      "title": "Godfather"
    }
  }
}
```

### 2.2 Filter Context 例子

```json
GET movies/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "match": {
            "title": "Godfather"
          }
        }
      ]
    }
  }
}
```

## 3. 下节课

剖析查询的 Request/Response
