显式映射(Explicit Mapping)
===

```json

DELETE employees

PUT employees
{
  "mappings": {
    "properties": {
      "name": {"type": "text"},
      "gender": {"type": "keyword"},
      "email": {"type": "keyword"},
      "birth_date": { "type": "date"}, 
      "address": {
        "properties": {
          "street": {"type": "text"},
          "country": {"type": "text"}
        }
      }
    }
  }
}

PUT employees/_doc/1
{
  "name": "bobo",
  "gender": "male",
  "email": "bobo@boboweike.cn",
  "birth_date": "1980-10-10",
  "address": {
    "street": "Shanghai, Pudong",
    "country": "China"
  }
}

GET employees
```