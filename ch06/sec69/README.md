# 范围 Range 查询

## 1. 重新导入 movies 数据集

```json
DELETE movies
```

### 1.1 添加 movies 索引

```json
# 显式创建movies索引的mapping
PUT movies
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "fields": {
          "original": {
            "type": "keyword"
          }
        }
      },
      "synopsis": {
        "type": "text",
        "fields": {
          "original": {
            "type": "keyword"
          }
        }
      },
      "actors": {
        "type": "text",
        "fields": {
          "original": {
            "type": "keyword"
          }
        }
      },
      "director": {
        "type": "text",
        "fields": {
          "original": {
            "type": "keyword"
          }
        }
      },
      "rating": {
        "type": "half_float"
      },
      "release_date": {
        "type": "date",
        "format": "dd-MM-yyyy"
      },
      "certificate": {
        "type": "keyword",
        "fields": {
          "original": {
            "type": "keyword"
          }
        }
      },
      "genre": {
        "type": "text",
        "index_prefixes":{},
        "fields": {
          "original": {
            "type": "keyword"
          }
        }
      }
    }
  }
}

```

### 1.2 下载 movie_bulk_data.json 到本地

https://github.com/boboweike/elasticsearch_complete_course/blob/main/ch06/sec69/movie_bulk_data.json
**注意最后有一个换行**

### 1.3 命令行执行导入

`curl -H "Content-Type: application/x-ndjson" -XPOST http://localhost:9200/_bulk --data-binary "@movie_bulk_data.json"`

### 1.4 校验

```json
GET /movies/_search
```

## 2. 范围 Range 查询

### 2.1 查询某个 rating 范围内的电影

```json
# gt ~ 大于
# gte - 大于等于
# lt - 小于
# lte - 小于等于

GET movies/_search
{
  "query": {
    "range": {
      "rating": {
        "gte": 9.0,
        "lte": 9.5
      }
    }
  }
}
```

### 2.2 查询 2010 年后的电影

```json
GET movies/_search
{
  "query": {
    "range": {
      "release_date": {
        "gte": "01-01-2010"
      }
    }
  },
  "sort": [
    {
      "release_date": {
        "order": "asc"
      }
    }
  ]
}
```

## 3. 日期计算

### 3.1 查询 x 天前发布的所有电影

```json
GET movies/_search
{
  "query": {
    "range": {
      "release_date": {
        "lte": "05-11-2023||-5d"
      }
    }
  }
}
```

### 3.2 查询 x 年前发布的所有电影

```json
GET movies/_search
{
  "query": {
    "range": {
      "release_date": {
        "lte": "now-4y"
      }
    }
  }
}
```

# y -> 年，M -> 月, w -> 周, d -> 天, h -> 小时, m -> 分, s -> 秒

## 4. 下一课 ～ 通配符 wildcard 查询
