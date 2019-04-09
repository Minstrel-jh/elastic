```json
GET slg4tg-000001/log/_search
{
  "query": {
    "bool": {
      "must": [{
        "term": {
          "actionType": 201
        }
      }]
    }
  },
  "aggs": {
    "by_localDt": {
      "date_range": {
        "field": "localDt",
        "format": "epoch_millis",
        "ranges": [{
          "from": 1554048000000,
          "to": 1554134400000
        },{
          "from": 1554134400000,
          "to": 1554220800000
        },{
          "from": 1554220800000,
          "to": 1554307200000
        },{
          "from": 1554307200000,
          "to": 1554393600000
        }]
      },
      "aggs": {
        "car_playerId": {
          "cardinality": {
            "field" : "playerId",
            "precision_threshold": 40000
          }
        }
      }
    }
  }
}
```

```json
GET slg4tg-000001/log/_search
{
  "query": {
    "bool": {
      "must": [{
        "term": {
          "actionType": 201
        }
      }]
    }
  },
  "aggs": {
    "by_localDt": {
      "date_range": {
        "field": "localDt",
        "format": "yyyy-MM-dd HH:mm:ssZ",
        "ranges": [{
          "from": "2019-04-01 00:00:00+0800",
          "to": "2019-04-02 00:00:00+0800"
        },{
          "from": "2019-04-02 00:00:00+0800",
          "to": "2019-04-03 00:00:00+0800"
        },{
          "from": "2019-04-03 00:00:00+0800",
          "to": "2019-04-04 00:00:00+0800"
        },{
          "from": "2019-04-04 00:00:00+0800",
          "to": "2019-04-05 00:00:00+0800"
        }]
      },
      "aggs": {
        "car_playerId": {
          "cardinality": {
            "field" : "playerId",
            "precision_threshold": 4000
          }
        }
      }
    }
  }
}
```