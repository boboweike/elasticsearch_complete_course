# 通配符 Wildcard 查询

# `*` -> 表示通配 0 或者多个字符

# `?` -> 表示通配单个字符

```json
GET movies/_search
```

## 1 通配符查询演示

### 1.1 星号通配符查询演示

```json
GET movies/_search
{
  "query": {
    "wildcard": {
      "title": {
        "value": "god*"
      }
    }
  }
}

GET movies/_search
{
  "query": {
    "wildcard": {
      "title": {
        "value": "g*d"
      }
    }
  }
}
```

### 1.2 问号通配符查询演示

```json
GET movies/_search
{
  "query": {
    "wildcard": {
      "title": {
        "value": "f?ght"
      }
    }
  }
}
```

### 1.3 对关键字字段进行 wildcard 查询

```json
GET movies/_search
{
  "query": {
    "wildcard": {
      "title.original": {
        "value": "The God*"
      }
    }
  }
}
```

### 1.4 对关键字字段进行 wildcard 查询(大小写不敏感)

```json
GET movies/_search
{
  "query": {
    "wildcard": {
      "title.original": {
        "value": "the god*",
        "case_insensitive": true
      }
    }
  }
}
```

## 2. 禁用高开销查询

# 高开销查询：wildcard, range, prefix, regex, join, fuzzy

### 2.1 禁用高开销查询

```json
PUT _cluster/settings
{
  "transient": {
    "search.allow_expensive_queries": "true"
  }
}
```

### 2.2 演示禁用高开销查询以后的效果

## 3. 下一课 ～ 前缀 Prefix 查询
