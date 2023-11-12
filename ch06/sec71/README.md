# 前缀 Prefix 查询

```json
GET movies/_mapping
```

## 1，通过前缀查询演员

```json
GET movies/_search
{
  "query": {
    "prefix": {
      "actors.original": {
        "value": "Mi"
      }
    }
  }
}
```

### 简写

```json
GET movies/_search
{
  "query": {
    "prefix": {
      "actors.original": "Mi"
    }
  }
}
```

### 对文本进行前缀查询(不推荐)

```json
GET movies/_search
{
  "query": {
    "prefix": {
      "actors": "mi"
    }
  }
}
```

## 2. 前缀查询+高亮

```json
GET movies/_search
{
  "_source": false,
  "query": {
    "prefix": {
      "actors.original": "Ja"
    }
  },
  "highlight": {
    "fields": {
      "actors.original": {}
    }
  }
}
```

## 3. 加快前缀查询

### 缺省 2~5 个字符

```json
PUT speed_up_prefix_movies
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "index_prefixes": {}
      }
    }
  }
}

GET speed_up_prefix_movies/_mapping
```

### 自定义前缀长度

```json
PUT speed_up_prefix_movies_custom_prefix_sizes
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "index_prefixes": {
          "min_chars": 3,
          "max_chars": 8
        }
      }
    }
  }
}

GET speed_up_prefix_movies_custom_prefix_sizes/_mapping
```

## 4. 下一课 ～ 正则查询
