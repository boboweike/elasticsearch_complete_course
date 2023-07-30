## 关于 ES Mapping 的补充内容

## 获取 Mapping

```json
GET /employees/_mapping

GET /employees/_mapping/field/name
GET /employees/_mapping/field/address.country
```

## 使用点标记创建 Mapping

```json
PUT employees
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "gender": {
        "type": "keyword"
      },
      "email": {
        "type": "keyword"
      },
      "birth_date": {
        "type": "date"
      },
      "address": {
        "properties": {
          "street": {
            "type": "text"
          },
          "country": {
            "type": "text"
          }
        }
      }
    }
  }
}

PUT employees_dot_notation
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "gender": {
        "type": "keyword"
      },
      "email": {
        "type": "keyword"
      },
      "birth_date": {
        "type": "date"
      },
      "address.street": {
        "type": "text"
      },
      "address.country": {
        "type": "text"
      }
    }
  }
}

GET /employees_dot_notation/_mapping

```

## 添加 Mapping

```json
PUT employees/_mapping
{
  "properties": {
    "join_date": {"type": "date"},
    "phone_number": {"type": "keyword"}
  }
}

GET /employees/_mapping

PUT employees/_doc/2
{
  "name": "william",
  "gender": "male",
  "email": "william@boboweike.cn",
  "birth_date": "1990-03-05",
  "join_date": "2020-07-20",
  "phone_number": "1800112233",
  "address": {
    "street": "Shanghai, Pudong",
    "country": "China"
  }
}

```

## ES 如何处理缺失字段

```json
PUT employees/_doc/3
{
  "name": "mary",
  "gender": "female",
  "email": "mary@boboweike.cn",
  "phone_number": "1800332211",
  "address": {
    "country": "China"
  }
}


GET /employees/_search

```
