# ES 查询的公共特性(Feature)

1. 分页
2. 高亮
3. Explanation
4. 排序
5. 响应结果处理

```json
# 1. 分页

# 查询固定数量的电影
GET movies/_search
{
  "size": 5,
  "query": {
    "match_all": {}
  }
}

# 使用size + from进行分页
GET movies/_search
{
  "size": 5,
  "from": 1,
  "query": {
    "match_all": {}
  }
}

# total_pages = ceil(total_hits / page_size)
# 14 = ceil(137 / 10)
# from = (page_size * (page_number - 1))
# 50 = (10 * (6 - 1))


# 2. 高亮

# 高亮匹配结果
GET movies/_search
{
  "_source": false,
  "query": {
    "term": {
      "title": {
        "value": "godfather"
      }
    }
  },
  "highlight": {
    "fields": {
      "title": {}
    }
  }
}

# 3. Explanation

# Final relevancy score = boost * idf * tf
# Idf = inverse document frequency
# tf = Term frequency
GET movies/_search
{
  "explain": true,
  "_source": false,
  "query": {
    "match": {
      "title": "Lord"
    }
  }
}

# 通过_explain端点查询和解释score
GET movies/_explain/14
{
  "query": {
    "match": {
      "title": "Lord"
    }
  }
}

GET movies/_explain/14
{
  "query": {
    "match": {
      "title": "Lords"
    }
  }
}

# 4. 排序(Sorting)

# 缺省根据打分(_score)排序
GET movies/_search
{
  "size": 5,
  "query": {
    "match": {
      "title": "Godfather"
    }
  }
}

# 等价于
GET movies/_search
{
  "size": 5,
  "query": {
    "match": {
      "title": "Godfather"
    }
  },
  "sort": [
    {
      "_score": {
        "order": "desc"
      }
    }
  ]
}

# 根据rating字段排序
GET movies/_search
{
  "size": 5,
  "query": {
    "match": {
      "genre": "crime"
    }
  },
  "sort": [
    {
      "rating": {
        "order": "desc"
      }
    }
  ]
}

# 根据rating字段排序，显示scoring
GET movies/_search
{
  "track_scores": true,
  "size": 5,
  "query": {
    "match": {
      "genre": "crime"
    }
  },
  "sort": [
    {
      "rating": {
        "order": "desc"
      }
    }
  ]
}

# 多字段排序
GET movies/_search
{
  "size": 5,
  "query": {
    "match": {
      "genre": "crime"
    }
  },
  "sort": [
    {
      "rating": {
        "order": "asc"
      }
    },
    {
      "release_date": {
        "order": "asc"
      }
    }
  ]
}

# 5. 响应结果处理

# 不返回source文档
GET movies/_search
{
  "_source": false,
  "query": {
    "match": {
      "certificate": "R"
    }
  }

}

# 返回指定字段
GET movies/_search
{
  "_source": false,
  "query": {
    "match": {
      "certificate": "R"
    }
  },
  "fields": [
    "title",
    "director"
  ]
}

# Source过滤
GET movies/_search
{
  "_source": ["title", "synopsis", "rating"],
  "query": {
    "match": {
      "certificate": "R"
    }
  }
}
```

# 下节课

# 1. term-level 查询

# 2. 全文本查询

# 3. 复合查询
