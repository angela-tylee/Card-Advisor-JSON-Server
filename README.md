# Getting Started

## Deployment

Render: https://card-advisor-json-server.onrender.com

- 運行限制：每月 750 hrs（單獨帳號計算）
- 待命期間：閒置 15m 會開始進入休眠
- 喚醒時間：最久 30s
- 其他：免費帳號若執行太久，會不定期被重開機

## API Document
https://card-advisor-json-server.onrender.com/cards

```
{
  "cards": [
      {
        "card_name":"信用卡名稱 (String)",
        "img": "信用卡圖片(String)",
        "url": "信用卡官方公告頁面URL(String)", 
        "reward_type":"回饋方式(String)",
        "announcement_period": "信用卡公告時間(String)",
        "general_payment": {     
          "reward": "消費回饋％(Number)",
          "limit": "回饋上限(String)",
        },
        "special_payment": {     
          "store": [             
              {
                  "消費店家(String)": {
                      "type": "消費類型(String)",
                      "reward": "消費回饋％(Number)",
                      "limit":"回饋上限(String)",        
                      "condition": "回饋條件任務(String)",    
                      "announcement_period":"信用卡公告時間(String)"
                  }
              },
              {
                  "消費店家(String)": {
                      "type": "消費類型(String)",
                      "reward": "消費回饋％(Number)",
                      "limit":"回饋上限(String)",
                      "condition":"回饋條件任務(String)",
                      "announcement_period":"信用卡公告時間(String)"
                  }
              }
          ] 
        },
        "welcom_bonus": {
            "reward": "新戶禮/首刷禮(String)",
            "condition": "回饋條件任務(String)"
        },
        "annual_fee": "年費(String)",  
        "high_reward_type": ["高回饋類別(Array)"]  
      }
    ]
}
```
`general_payment` 一般消費
`special_payment` 指定通路消費

### Example
```
{
  "cards": [
    {
      "card_name": "中國信託 Line Pay 卡",
      "img": "ctbcbank_linepay.png",
      "url": "https://www.ctbcbank.com/content/dam/minisite/long/creditcard/LINEPay/notice.html",
      "reward_type": "LINE Points",
      "announcement_period": "2023/1/1 - 2023/12/31",
      "general_payment": {
          "reward": "1%",
          "limit": "none",
          "condition": "none"
      },
      "special_payment": {
        "store": [
          {
            "海外消費": {
                "type": "overseas",
                "reward":"2.8%",
                "limit": "none",
                "condition": "",
                "announcement_period": "2023/1/1 - 2023/12/31"
            }
          },
          {
              "Steam": {
                  "type": "entertainment",
                  "reward":"10%",
                  "limit": "",
                  "condition": "以LINE Pay VISA卡別支付(限量需登錄)",
                  "announcement_period": "2023/1/1 - 2023/12/31"
              }
          }
        ]
    },
    "welcom_bonus": {
        "reward":"好禮三選一、新卡禮刷卡金 NT100元",
        "condition":"核卡日起30日內消費滿NT888元送好禮，好禮三選一：LINE FRIENDS 風趣系列旅行組（乙組）、LINE FRIENDS 攜帶式卡式爐（乙組）、刷卡金 NT500元"
    },
    "annual_fee": "成功綁定LINE Pay或申請本行電子帳單期間，享終身免年費禮遇",
    "referral": "推薦人 100 LINE Points、被推薦人 50 LINE Points",
    "high_reward_type": [
        "mobile_payment"
    ]
  }
  ]
}
```


## Routes

`posts`, `comments` are default data. It can be replaced with new data e.g. `cards`

```
GET    /posts
POST   /posts
PUT    /posts
PATCH  /posts
DELETE /posts
```


### Filter

Use . to access deep properties
```
GET /posts?title=json-server&author=typicode
GET /posts?id=1&id=2
GET /comments?author.name=typicode
```

### Paginate

Use `_page` and optionally `_limit` to paginate returned data.

In the Link header you'll get first, prev, next and last links.
```
GET /posts?_page=7
GET /posts?_page=7&_limit=20
10 items are returned by default
```

### Sort

Add `_sort` and `_order` (ascending order by default)
```
GET /posts?_sort=views&_order=asc
GET /posts/1/comments?_sort=votes&_order=asc
```
For multiple fields, use the following format:
```
GET /posts?_sort=user,views&_order=desc,asc
```

### Slice

Add `_start` and `_end` or `_limit` (an X-Total-Count header is included in the response)
```
GET /posts?_start=20&_end=30
GET /posts/1/comments?_start=20&_end=30
GET /posts/1/comments?_start=20&_limit=10
```
Works exactly as Array.slice (i.e. _start is inclusive and _end exclusive)

### Operators

Add `_gte` or `_lte` for getting a range

```
GET /posts?views_gte=10&views_lte=20
```
Add `_ne` to exclude a value
```
GET /posts?id_ne=1
```
Add `_like` to filter (RegExp supported)
```
GET /posts?title_like=server
```
### Full-text search

Add `q`
```
GET /posts?q=internet
```
### Relationships

To include children resources, add `_embed`
```
GET /posts?_embed=comments
GET /posts/1?_embed=comments
```
To include parent resource, add `_expand`
```
GET /comments?_expand=post
GET /comments/1?_expand=post
```
To get or create nested resources (by default one level, add custom routes for more)
```
GET  /posts/1/comments
POST /posts/1/comments
```

### Database
```
GET /db
```

### Homepage

Returns default index file or serves `./public` directory
```
GET /
```

## Reference
[JSON Server](https://github.com/typicode/json-server)
[render](https://render.com/docs/free)
[render deployment tutorial](https://hackmd.io/@NoName21/deploy-to-render-2022)
