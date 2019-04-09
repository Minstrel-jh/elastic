```json
GET /slg4tg-000001/log/_search
{
  "size": 0,
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "resType": 7
          }
        },
        {
          "term": {
            "playerId": 498896801824773
          }
        },
        {
          "script": {
            "script": {
              "source": "doc[\"newNum\"].value > doc[\"oldNum\"].value",
              "lang": "painless"
            }
          }
        }
      ]
    }
  },
  "aggs": {
    "newSum": {
      "sum": {
        "script": "doc.newNum.value - doc.oldNum.value"
      }
    }
  }
}
```