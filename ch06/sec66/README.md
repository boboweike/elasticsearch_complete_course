# Term-level search(6.1)

## 1. Term-level search 介绍

- Term-level 查询属于一种结构化数据查询，适用于关键字，数值、布尔、范围(range)，日期等数据类型。
- Term-level 查询用于精准匹配(存在或不存在)，而不关心结果的相关性。
- Term-level 查询类似于数据库查询中的`WHERE`语句 ～ 如果条件满足就返回结果，否则没有结果。
- Term-level 查询和全文本(full-text)查询的根本区别是：term-level 查询不经过文本分析。
- Term-level 查询包括 term, terms, IDs, exists, range, wildcard, prefix, fuzzy 等不同查询方式，后续依次讲解。

## 2. term & terms query 演示

### 2.1 Term Query 演示

查找具有某个 certificate 的电影

```json
# certificate是关键字字段，并不经过文本分析
GET movies/_search
{
  "query": {
    "term": {
      "certificate": "R"
    }
  }
}

# 值换成`r`则查不到
GET movies/_search
{
  "query": {
    "term": {
      "certificate": "r"
    }
  }
}

# 大小写不敏感查询(缺省大小写敏感)
GET movies/_search
{
  "query": {
    "term": {
      "certificate": {
        "value": "r",
        "case_insensitive": true
      }
    }
  }
}
```

### 2.2. Terms Query

```json
# 对单个字段进行多个term查询
GET movies/_search
{
  "query": {
    "terms": {
      "certificate": [
        "PG-13", "Approved"
      ]
    }
  }
}

```

### 2.3. 对文本字段执行 term query

```json
# "The Godfather" -> 文本分析 -> index("the", "godfather")
GET movies/_search
{
  "query": {
    "term": {
      "title": {
        "value": "The Godfather"
      }
    }
  }
}

GET movies/_search
{
  "query": {
    "term": {
      "title": {
        "value": "the godfather"
      }
    }
  }
}

GET movies/_search
{
  "query": {
    "term": {
      "title": {
        "value": "godfather"
      }
    }
  }
}

GET movies/_search
{
  "query": {
    "term": {
      "title": {
        "value": "the"
      }
    }
  }
}
```

## 3. 总结，使用 term query 注意点：

# ES 中最重要的查询方式之一

# term query 适用于非文本字段类型，比方说关键字，数值，日期等数据类型。

# term query 并不是适用于文本字段。虽然不推荐，你仍然可以对文本字段进行 term query，前提是你要理解文本分析规则。

# 缺省大小写敏感，v7.1.0 后支持`case_insensitive`

# 多个 term 查询使用 terms query

## 4. 下节课 -> IDs query
