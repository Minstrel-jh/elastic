```json
# 使用kibana的Dev Tools里的Console来操作你的elas
GET /_cat/health
```

```json
GET _search
{
  "query": {
    "match_all": {}
  }
}
```

```json
# 插入数据
# 格式: 索引/类型/id
POST hello/doc/1
{
  "user": "jack",
  "uid": 1,
  "score": 70
}
```

```json
# 查看数据
GET hello/doc/1
```

```json
# 修改数据
PUT hello/doc/1
{
  "user": "jack",
  "uid": 1,
  "score": 75
}
```

```json
# 删除数据
DELETE hello/doc/1
```

```json
# 添加数据
# 不加id的话，会自动生成id
POST hello/doc/
{
  "user": "jack",
  "uid": 1,
  "score": 70
}
```

```json
# 检索
GET hello/_search
```

```json
# 删除索引
DELETE hello
```

```json
# 批量插入
POST _bulk
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "jack", "uid": 1, "score": 70}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "tom", "uid": 2, "score": 80}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "david", "uid": 3, "score": 90}
```

```json
# 查询
GET hello/_search
{
  "query": {
    "match": {
      "user": "david"
    }
  }
}
```

```json
# 查询 条件 and
GET hello/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {"user": "david"}},
        {"match": {"uid": 3}}
      ]
    }
  }
}
```

```json
# 查询 条件 not
GET hello/_search
{
  "query": {
    "bool": {
      "must_not": [
        {"match": {
          "user": "david"
        }}
      ]
    }
  }
}
```

```json
# 查询 条件 or
GET hello/_search
{
  "query": {
    "bool": {
      "should": [
        {"match": {
          "user": "jack"
        }},
        {"match": {
          "user": "tom"
        }}
      ]
    }
  }
}
```

```json
# 计数
GET hello/_count
{
  "query": {
    "bool": {
      "must": [
        {"match": {
          "user": "tom"
        }}
      ]
    }
  }
}
```

```json
# 查看mapping(index的结构)
GET hello/_mapping
```

```json
# 自定义一个索引
DELETE hello
PUT hello
{
  "settings": {"number_of_shards": 1}
}
```

```json
PUT hello/doc/_mapping
{
  "properties": {
    "user": {
      "type": "keyword"
    },
    "subject": {
      "type": "keyword"
    },
    "uid": {
      "type": "long"
    },
    "score": {
      "type": "long"
    }
  }
}
```

```json
POST _bulk
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "jack","subject": "Java","uid": 1,"score": 80}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "tom","subject": "Java", "uid": 2, "score": 70}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "david","subject": "Java", "uid": 3, "score": 90}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "nancy","subject": "Java","uid": 4,"score": 60}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "john","subject": "Java", "uid": 5, "score": 100}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "mike","subject": "Java", "uid": 6, "score": 95}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "jack","subject": "c","uid": 1,"score": 85}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "tom","subject": "c", "uid": 2, "score": 65}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "david","subject": "c", "uid": 3, "score": 70}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "nancy","subject": "c","uid": 4,"score": 50}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "john","subject": "c", "uid": 5, "score": 95}
{"index": {"_index": "hello", "_type": "doc"}}
{"user": "mike","subject": "c", "uid": 6, "score": 80}
```

```json
GET hello/_search
```

```json
# 范围 排序
GET hello/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {
          "subject": "Java"
        }},
        {"range": {
          "score": {
            "gte": 80,
            "lte": 100
          }
        }}
      ]
    }
  },
  "sort": [
    {
      "score": {
        "order": "desc"
      }
    }
  ]
}
```

```json
# range聚合
# to 为开区间
GET hello/_search
{
  "query": {
    "match": {
      "subject": "Java"
    }
  }, 
  "size": 20, 
  "aggs": {
    "by_score": {
      "range": {
        "field": "score",
        "ranges": [
          {
            "from": 0,
            "to": 60
          }, {
            "from": 60,
            "to": 80
          }, {
            "from": 80,
            "to": 90
          }, {
            "from": 90,
            "to": 101
          }
        ],
      }
    }
  }
}
```

```json
# terms聚合
GET hello/_search
{
  "size": 0, 
  "aggs": {
    "by_subject": {
      "terms": {
        "field": "user",
        "size": 6
      },
      "aggs": {
        "sum_score": {
          "sum": {
            "field": "score"
          }
        }
      }
    }
  }
}
```

```json
# COUNT(DISTINCT)
GET hello/_search
{
  "size": 0, 
  "aggs": {
    "car_user": {
      "cardinality": {
        "field": "user"
      }
    }
  }
}
```

```json
# sql
POST /_xpack/sql?format=txt
{
  "query": "SELECT * FROM hello"
}
```

```json
POST /_xpack/sql?format=txt
{
  "query": "SELECT count(DISTINCT user) FROM hello"
}
```

```json
# 将sql查询翻译成普通的查询
POST /_xpack/sql/translate
{
  "query": "SELECT count(DISTINCT user) FROM hello"
}
```
