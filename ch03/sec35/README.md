ES如何处理数组(array)?
===

```json
POST /books/_doc
{
  "tags": "java"
}

POST /books/_doc
{
  "tags": ["java", "programming"]
}

{
  "books" : {
    "mappings" : {
      "properties" : {
        "tags" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    }
  }
}
```

## analyze
```json
POST /_analyze
{
  "text": ["How is array", "handled in ES"]
}
```

## 数组中的值应当是相同类型的
```json
// Good
["java", "programming", "classic"]
[1, 2, 3]
[true, false, false]
[{"key1": "Value 1"}, {"key2": "Value 2"}, {"key3": "Value 3"}]

// Ok，自动转型
["java", "programming", 100]
[1, 2, "3"]
[true, "false", true]

// Fail，无法自动转型
[{"key1": "Value 1"}, {"key2": "Value 2"}, true]
```



