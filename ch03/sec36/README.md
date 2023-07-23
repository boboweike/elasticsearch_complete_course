## 自动转型(type coercion)

```json
PUT /employee/_doc/1
{
  "age": 20
}

PUT /employee/_doc/2
{
  "age": "22"
}

PUT /employee/_doc/3
{
  "age": "35y"
}

GET /employee/_search

GET /employee
```