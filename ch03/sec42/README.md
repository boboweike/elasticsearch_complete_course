## 字段别名(alias)

```json

GET books/_mapping

PUT /books/_mapping
{
  "properties": {
    "book_name": {
      "type": "alias",
      "path": "title"
    }
  }
}

PUT /books/_doc/6
{
  "book_id": 106,
  "book_name": "Spring in Action, Sixth Edition",
  "rating": 4.3,
  "price": 75,
  "author": "Craig Walls",
  "pub_date": "2022-03-01",
  "publisher": "Manning"
}

GET books/_search
{
  "query": {
    "match": {
      "title": "effective"
    }
  }
}

GET books/_search
{
  "query": {
    "match": {
      "book_name": "effective"
    }
  }
}

POST books/_search
{
  "query": {
    "match_all": {}
  }
}
```
