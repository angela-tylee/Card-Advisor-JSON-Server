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
          "reward": "3.3%",
          "store": [             
              {
                  "消費店家(String)": {
                      "type": "消費類型(String)",
                      "limit":"回饋上限(String)",        
                      "condition": "回饋條件任務(String)",    
                      "announcement_period":"信用卡公告時間(String)"
                  }
              },
              {
                  "消費店家(String)": {
                      "type": "消費類型(String)",
                      "limit":"回饋上限(String)",
                      "condition":"回饋條件任務(String)",
                      "announcement_period":"信用卡公告時間(String)"
                  }
              }
          ] 
        },
        "welcom_bonus": "新戶禮/首刷禮(String)", 
        "annual_fee": "年費(String)",  
        "high_reward_type": ["高回饋類別(Array)“]  
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
        "card_name":"國泰世華Cube卡",
        "img": "cathaybk_cube.png",
        "url": "https://www.cathaybk.com.tw/cathaybk/personal/product/credit-card/cards/cube/", 
        "reward_type":"小樹點",
        "announcement_period": "2023.07.01 - 2024.06.30",
        "general_payment": {     
          "reward": "0.3%",
          "limit": "",
          "condition": ""
        },
        "special_payment": {     
          "reward": "3.3%",
          "store": [             
              {
                  "Line Pay": {
                      "type": "行動支付",
                      "limit":"",        
                      "condition": "",    
                      "announcement_period":""
                  }
              },
              {
                  "UberEats": {
                      "type": "美食餐飲",
                      "limit":"",
                      "condition":"",
                      "announcement_period":""
                  }
              }
          ] 
        },
        "welcom_bonus": "none", 
        "annual_fee": "none",  
        "high_reward_type": ["行動支付", "網購"]  
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
