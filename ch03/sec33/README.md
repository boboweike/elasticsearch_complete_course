## 缺省date字段行为
```json
GET /_cat/indices?v

DELETE flights

PUT flights
{
  "mappings": {
    "properties": {
      "departure_date": {
        "type": "date"
      }
    }
  }
}

GET flights

PUT /flights/_doc/1
{
  "title": "Flight 1",
  "departure_date": "2021-05-01"
}

PUT /flights/_doc/2
{
  "title": "Flight 2",
  "departure_date": "2021-05-01T15:45:50"
}

PUT /flights/_doc/3
{
  "title": "Flight 3",
  "departure_date": 1619883950000
}

GET /flights/_search
{
  "query": {
    "range": {
      "departure_date": {
        "gte": "2021-05-01",
        "lte": "2021-05-02"
      }
    }
  }
}
```

## 自定义日期格式
```json
PUT flights
{
  "mappings": {
    "properties": {
      "departure_date": {
        "type": "date",
        "format": "dd-MM-yyyy||dd-MM-yy"
      }
    }
  }
}
```
