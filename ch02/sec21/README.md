文档版本号
===

## 1介绍

* 和git版本控制完全不同，无法获取版本历史，只是一种简单的版本机制
* 每一个文档中有一个`_version`，存放当前最新版本号，每次文档变更(更新或删除)该版本号会增加1
  * 删除时版本号保持60秒(可配置)，60秒插入相同id文档，版本号继续递增，否则从1重新开始
* GET by ID查看版本号
* 查询时，缺省不返回版本号，需要添加参数才返回

```json
GET /books/_doc/20

PUT /books/_doc/20
{
    "price" : 99,
    "authtor" : "Marko Luksa",
    "inStock" : 158,
    "title" : "Kubernetes in Action"
}

DELETE /books/_doc/20

GET /books/_search
{
  "version": true
}

```

## 2.版本类型

* 内部提供版本号(缺省)
* 外部提供版本号，比方说外部数据库维护的版本号，必须是整数

外部版本号样例：
```json
PUT /books/_doc/123?version=3&version_type=external
{
    "price" : 99,
    "authtor" : "Marko Luksa",
    "inStock" : 158,
    "title" : "Kubernetes in Action"
}
```

## 用途

* 以前用于乐观并发控制，但是现在已经不用它做并发控制了(演示)
* 目前用途：可以查看文档变更的次数

```json
PUT /books/_doc/123?version=3
{
    "price" : 99,
    "authtor" : "Marko Luksa",
    "inStock" : 158,
    "title" : "Kubernetes in Action"
}
```