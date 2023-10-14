# 生命周期策略

## 1. 索引的生命周期阶段

> `HOT`: 可操作模式，读写都可以
> `WARM`: 只读，可以频繁查询
> `COLD`: 只读，不频繁查询，查询性能一般
> `FROZEN`: 只读，极少查询，查询性能慢
> `DELETE`: 最终阶段，索引被永久删除

## 2. 手动管理生命周期演示

### 第一步，创建一个生命周期策略

```json
PUT _ilm/policy/hot_delete_policy
{
  "policy": {
    "phases": {
      "hot": {
        "min_age": "1d",
        "actions": {
          "set_priority": {
            "priority": 250
          }
        }
      },
      "delete": {
        "actions": {
          "delete": {}
        }
      }
    }
  }
}
```

### 第二步，将索引和策略相关联

```json
PUT hdp_index
{
  "settings": {
    "index.lifecycle.name": "hot_delete_policy"
  }
}

GET hdp_index
```

## 3. 生命周期+Rollover

### 第一步，创建生命周期策略

```json
PUT _ilm/policy/hot_rollover_policy
{
  "policy": {
    "phases": {
      "hot": {
        "min_age": "0ms",
        "actions": {
          "rollover": {
            "max_age": "1d",
            "max_docs": 20000,
            "max_size": "5gb"
          }
        }
      }
    }
  }
}
```

### 第二步，将索引模版和策略相关联

```json
PUT _index_template/nginx_logs_template
{
  "index_patterns": ["nginx-"],
  "template": {
    "settings": {
      "index.lifecycle.name": "hot_rollover_policy",
      "index.lifecycle.rollover_alias": "nginx-logs-alias"
    }
  }
}
```

### 第三步，创建和模版相匹配的索引

```json
PUT nginx-index-000001
{
  "aliases": {
    "nginx-logs-alias": {
      "is_write_index": true
    }
  }
}

GET nginx-index-000001
```

## 4. 缺省扫描间隔 = 10 分钟

```json
GET _cluster/settings

PUT _cluster/settings
{
  "persistent": {
    "indices.lifecycle.poll_interval":"10ms"
  }
}
```

## 5. 高级案例

Hot 阶段 30 天 => Warm 阶段 30 天 => Cold 阶段 1 年 => 删除索引

## 6. 下个视频

第四章 -> `索引操作`讲完，第五章 -> `查询（Search）`
