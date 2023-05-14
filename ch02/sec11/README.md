## refresh参数演示

设置`refresh_interval`:
```
GET /books/_settings

PUT /books/_settings
{
  "index": {
    "refresh_interval": "15s"
  }
}
```

```
PUT books/_doc/1
{
  "title": "Effective Java",
  "Author": "Joshua Bloch",
  "Rating": 4.7,
  "Price": 60,
  "inStock": 300
}

GET books/_search

PUT books/_doc/2?refresh
{
  "title": "Head First Java",
  "Author": "Kathy Sierra",
  "Rating": 4.5,
  "Price": 80,
  "inStock": 1000
}

PUT books/_doc/3?refresh=wait_for
{
  "title": "Thinking in Java",
  "Author": "Bruce Eckel",
  "Rating": 4.5,
  "Price": 60,
  "inStock": 200
}
```
