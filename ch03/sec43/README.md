## 多字段 Mapping

```json
PUT /blogs
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text"
      },
      "tags": {
        "type": "text",
        "fields": {
          "keyword": {
            "type": "keyword"
          }
        }
      }
    }
  }
}

GET /blogs/_mapping

PUT /blogs/_doc/1
{
  "title": "A test blog",
  "tags": ["java", "programming", "API"]
}

GET /blogs/_search
{
  "query": {
    "match_all": {}
  }
}

GET /blogs/_search
{
  "query": {
    "match": {
      "tags": "java"
    }
  }
}

GET /blogs/_search
{
  "query": {
    "term": {
      "tags.keyword": "java"
    }
  }
}

```
