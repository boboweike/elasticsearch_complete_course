# Query DSL

- DSL = domain specific language
- 基于 JSON 的查询语法
- 表达能力更强，支持更复杂的查询(嵌套/复合)
- 支持聚合分析

```json
# 1. 简单查询

GET movies/_search
{
  "query": {
    "match": {
      "title": "Lord"
    }
  }
}

# 2. Copy to CURL

# 3. 聚合查询

GET movies/_search
{
  "size": 0,
  "aggs": {
    "average_movie_rating": {
      "avg": {
        "field": "rating"
      }
    }
  }
}

# 4. 叶子和复合查询

# 叶子查询
GET movies/_search
{
  "query": {
    "match_phrase": {
      "synopsis": "Gandalf and Aragorn lead the World of Men against Saurons's army"
    }
  }
}

# 复合查询
GET movies/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": "Godfather"
          }
        }
      ],
      "must_not": [
        {
          "range": {
            "rating": {
              "lt": 9
            }
          }
        }
      ],
      "filter": [
        {
          "match": {
            "actors": "Brando"
          }
        }
      ]
    }
  }
}
```
