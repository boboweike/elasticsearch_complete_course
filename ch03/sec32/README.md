## keyword演示
```json
POST _analyze
{
  "text": "Getting started with ElasticSearch",
  "analyzer": "keyword"
}
```

## constant_keyword演示
```json
GET /_cat/indices?v

PUT census
{
  "mappings": {
    "properties": {
      "city": {
        "type": "constant_keyword",
        "value": "Shanghai"
      }
    }
  }
}

GET census/_mapping

PUT census/_doc/1
{
  "name": "Bobo"
}

GET census/_search
{
  "query": {
    "term": {
      "city": {
        "value": "Shanghai"
      }
    }
  }
}

```

## wildcard演示
```json
PUT errors
{
  "mappings": {
    "properties": {
      "description": {
        "type": "wildcard"
      }
    }
  }
}

PUT errors/_doc/1
{
  "name": "NullPointerException",
  "description": "Null Pointer exception as object is null"
}

GET errors/_search
{
  "query": {
    "wildcard": {
      "description": {
        "value": "*obj*"
      }
    }
  }
}

GET errors/_mapping
```