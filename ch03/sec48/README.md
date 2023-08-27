## 一些常用和高级的 Mapping 参数

### 1. format 参数

- 自定义 date 字段的格式
- 常用内置格式
  - 例子："epoch_second"
  - https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html
- 缺省格式
  - "strict_date_optional_time||epoch_millis"
- 基于 Java 的 DateFormatter 语法
  - "MM-dd-yyyy"
  - https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html

```json
PUT /books
{
  "mappings": {
    "properties": {
      "pub_date": {
        "type": "date",
        "format": "MM-dd-yyyy"
      }
    }
  }
}

PUT /books
{
  "mappings": {
    "properties": {
      "pub_date": {
        "type": "date",
        "format": "epoch_second"
      }
    }
  }
}
```

### 2. coerce 参数

- 是否启用强制类型转换
- 缺省启用

```json
PUT /books
{
  "mappings": {
    "properties": {
      "rating": {
        "type": "float",
        "coerce": false
      }
    }
  }
}

PUT /books
{
  "settings": {
    "index.mapping.coerce": false
  }
  "mappings": {
    "properties": {
      "rating": {
        "type": "float",
        "coerce": true
      }
    }
  }
}
```

## 3. index 参数

- 字段是否启用索引(indexing)
- 如果禁用
  - 该字段无法用于查询(search)
  - 仍然可用于聚合
  - 原始值仍然存在`_source`中
- 适用于不用于查询的数据，例如：时间序列(time series)数据，不查询/过滤，主要用于聚合
  - 节省磁盘空间
  - 提升文档插入速度

```json
PUT /books
{
  "mappings": {
    "properties": {
      "in_stock": {
        "type": "integer",
        "index": false
      }
    }
  }
}
```

## 4. doc_values 参数

- 正向(uninverted)索引(doc -> terms)
- 正向索引适用于排序/聚合/通过脚本(script)访问字段值, 反向索引适用于全文本查询
- 额外的数据结构，并不取代普通索引
- ES 会根据查询的不同而选择最佳数据结构
- 如果字段不用于排序/聚合/脚本操作，可禁用 doc_values，主要针对大索引
  - 减少磁盘存储
  - 提升索引性能
- 对于现有索引，需要 reindex

#### Doc-value-only fields

```json
PUT /books
{
  "mappings": {
    "properties": {
      "publisher": {
        "type":  "keyword",
      },
      "isbn": {
        "type":  "keyword",
        "index": false
      }
    }
  }
}
```

#### Disabling doc values

```json
PUT /books
{
  "mappings": {
    "properties": {
      "publisher": {
        "type":  "keyword",
      },
      "isbn": {
        "type":  "keyword",
        "doc_values": false
      }
    }
  }
}
```

## 5. norms 参数

- 用于相关性打分的 normalization factors
- 用于对查询结果进行打分
- 可以禁用该参数，节省磁盘空间
  - 如果字段不需要相关性打分
  - 字段仍然可以用于查询/过滤和聚合

```json
PUT /books
{
  "mappings": {
    "properties": {
      "tags": {
        "type": "text",
        "norms": false
      }
    }
  }
}
```

## 6. copy_to 参数

- 将多个字段值拷贝到一个组合字段
- first_name + last_name -> full_name
- 组合字段独立建索引
- 组合字段并不在`_source`中

```json
PUT /books
{
  "mappings": {
    "properties": {
      "first_name": {
        "type": "text",
        "copy_to": "full_name"
      },
      "last_name": {
        "type": "text",
        "copy_to": "last_name"
      },
      "full_name": {
        "type": "text"
      }
    }
  }
}

PUT /books/_doc/1
{
  "first_name": "Jack",
  "last_name": "Liu"
}
```

## 7. null_value 参数

https://www.elastic.co/guide/en/elasticsearch/reference/current/null-value.html
