# Posts

## [POST] Create Post

> https Request Example

```http
POST https://baseUrl/posts/
```

> Request Body: 아이템인 경우

```json
{
 "category":"item",
 "description": "빠른 판매 원합니다",
 "images": [
  {
   "display_order": 0,
   "original": "images/posts/post_rVvDZa9Mk28f58ch2AXuN0.jpeg"
  }
 ],
 "price": "10000",
 "server": "RaCmzUhXEfc3Sv6HnpuQQc",
 "title": "트럭스터 어새신 팬츠 공43%"
}
```

> Request Body: 게임머니인 경우

```json
{
  "coin_amount":15000,
  "category":"coin",
  "description":"빠른 판매 원합니다",
  "images":[
    {
      "url":"http://localhost:8000/media/images/posts/post_rVvDZa9Mk28f58ch2AXuN0.jpeg",
      "display_order":0
    }
  ],
  "price":10000,
  "server":"RaCmzUhXEfc3Sv6HnpuQQc",
  "title":"15000메소 팝니다.",
}
```

> Response: 201 Created - 아이템인 경우

```json
{
  "coin_amount":null,
  "category":"item",
  "description":"빠른 판매 원합니다",
  "images":[
    {
      "url":"http://localhost:8000/media/images/posts/post_rVvDZa9Mk28f58ch2AXuN0.jpeg",
      "display_order":0
    }
  ],
  "price":10000,
  "server":"RaCmzUhXEfc3Sv6HnpuQQc",
  "status":"available",
  "title":"트럭스터 어새신 팬츠 공43%",
  "uuid":"dmipiWmHULndGLD32T7GTf"
}
```

> Response: 201 Created - 게임머니인 경우

```json
{
  "coin_amount":15000,
  "category":"coin",
  "description":"빠른 판매 원합니다",
  "images":[
    {
      "url":"http://localhost:8000/media/images/posts/post_rVvDZa9Mk28f58ch2AXuN0.jpeg",
      "display_order":0
    }
  ],
  "price":10000,
  "server":"RaCmzUhXEfc3Sv6HnpuQQc",
  "status":"available",
  "title":"15000메소 팝니다.",
  "uuid":"ny5vJXe3DZNqQvQeQq7YMU"
}
```

> Response: 409 Conflict - 아이템에 coin_amount값이 request.body에 포함된 경우

```json
{
 "detail": "아이템은 게임머니 수량을 입력할 수 없습니다.",
 "code": "10002"
}
```

> Response: 406 Not Acceptable - 게임머니에 수량(coin_amount 값)이 request.body에 없는 경우

```json
{
 "detail": "게임머니는 수량을 반드시 입력해야 합니다.",
 "code": "10003"
}
```

### Request Parameters

| Parameters | Type   | Required | description                                                  |
| ---------- | ------ | -------- | ------------------------------------------------------------ |
| category   | string | false    | 값이 빠진 경우 default인 `item`이 자동으로 설정. `item`, `coin` 두 종류 옵션이 있음. |

### Response Parameters

| Parameters | Type   | description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| status     | string | `available(=판매중)`, `reserverd(=예약중)`, `sold(=거래완료)`, `hidden(=숨김)` |

## [GET] Post List

> https Request Example

```http
GET https://baseUrl/posts/
GET https://baseUrl/posts/?seller=NgUwhvm8xh3pVwzbJUy5qu&status=sold
```

> Request Body

```json
없음
```

> Response: 200 OK

```json
{
  "next":null,
  "previous":null,
  "results":[
    {
      "coin_amount":10,
      "category": "coin",
      "description":"매너 거래욤",
      "images":[],
      "price":34000,
      "title":"10억 메소 팝니다",
      "uuid":"gcWDK3d3EAQB7ecrZ33ejB",
      "coin_rate":3400,
      "humanized_created":"끌올 2분 전",
    },
    {
      "coin_amount":null,
      "category": "item",
      "description":"빠른 판매 원합니다",
      "images":[
        {
          "url":"http://localhost:8000/media/images/posts/post_rVvDZa9Mk28f58ch2AXuN0.jpeg",
          "display_order":0
        }
      ],
      "price":10000,
      "title":"트럭스터 어새신 팬츠 공43%",
      "uuid":"dmipiWmHULndGLD32T7GTf",
      "coin_rate":null,
      "humanized_created":"4시간 전",
    },
    ...
  ]
}
```

### Query Parameters

| Parameters | Type | Required | 선택가능한 상태 값과 설명                                    |
| ---------- | ---- | -------- | ------------------------------------------------------------ |
| category   | str  | false    | `item` or `coin`                                             |
| server     | uuid | false    | server API에서 받아온 uuid                                   |
| seller     | uuid | false    | 특정 판매자의 판매내역을 불러오고 싶을 때(ex. 나의 판매내역 또는 다른 사람의 판매내역) |
| status     | str  | false    | `available`, `reserved`, `sold`, `hidden`                    |

### Response Parameters

| Parameters        | Type   | description                                                  |
| ----------------- | ------ | ------------------------------------------------------------ |
| humanized_created | string | 정렬 기준이 되는 필드. 한번도 끌올한 적이 없으면 생성시간으로 표시되고, 한번이라도 끌올했다면 마지막 끌올 시간이 표시되고 앞에 `끌올`이라고 표기됨. |
| coin_rate         | int    | category가 `coin`인 경우만 표시됨. 1억당 메소로, 소수점은 반올림. |
| coin_amount       | int    | 메이플: 메소(억 단위)                                        |

## [PATCH] Update Post

> https Request Example

```http
PATCH https://baseUrl/posts/:uuid/
```

> Request Body (아이템)

```json
{
  "category":"item",
  "description":"(수정)빠른 판매 원합니다",
  "images":[
    {
      "display_order":0,
      "original":"images/posts/post_rVvDZa9Mk28f58ch2AXuN1.jpeg"
    },
    ...
  ],
  "price":60000,
  "status":"reserved",
  "title":"(수정)트럭스터 어새신 팬츠 공43%"
}
```

> Request Body (게임머니)

```json
{
  "category":"coin",
  "coin_amount":20,
  "description":"(수정)빠른 판매 원합니다",
  "images":[
    {
      "display_order":0,
      "original":"images/posts/post_rVvDZa9Mk28f58ch2AXuN1.jpeg"
    },
    ...
  ],
  "price":60000,
  "status":"hidden",
  "title":"(수정) 20억 메소 팝니다"
}
```

> Response: 200 OK (아이템)

```json
{
  "coin_amount":null,
  "category":"item",
  "description":"(수정)빠른 판매 원합니다",
  "images":[
    {
      "url":"http://localhost:8000/media/images/posts/post_rVvDZa9Mk28f58ch2AXuN1.jpeg",
      "display_order":0
    },
    ...
  ],
  "price":60000,
  "server":"RaCmzUhXEfc3Sv6HnpuQQc",
  "status":"reserved",
  "title":"(수정)트럭스터 어새신 팬츠 공43%",
  "uuid":"P7pZYBbrPMDJMkvBTncit7"
}
```

> Response: 200 OK (게임머니)

```json
{
  "coin_amount":20,
  "category":"coin",
  "description":"(수정)빠른 판매 원합니다",
  "images":[
    {
      "url":"http://localhost:8000/media/images/posts/post_rVvDZa9Mk28f58ch2AXuN1.jpeg",
      "display_order":0
    },
    ...
  ],
  "price":60000,
  "server":"RaCmzUhXEfc3Sv6HnpuQQc",
  "status":"hidden",
  "title":"(수정) 20억 메소 팝니다",
  "uuid":"gcWDK3d3EAQB7ecrZ33ejB"
}
```

주의: 게시글 끌어올리기는 별로 API(#patch-bump-post)를 사용합니다.

### Request Body

| Parameters | Type | Required | description                               |
| ---------- | ---- | -------- | ----------------------------------------- |
| status     | str  | false    | `available`, `reserved`, `sold`, `hidden` |

## [GET] Post Detail

> https Request Example

```http
GET https://baseUrl/posts/:uuid/
```

> Request Body

```json
없음
```

> Response: 200 OK (아이템)

```json
{
  "coin_amount":null,
  "category":"item",
  "description":"빠른 판매 원합니다",
  "images":[
    {
      "url":"http://localhost:8000/media/images/posts/post_rVvDZa9Mk28f58ch2AXuN0.jpeg",
      "display_order":0
    },
    ...
  ],
  "price":10000,
  "seller":{
    "uuid":"aVYk2fG5yWETFJwCiGbthF",
    "username":"asdf",
    "profile":null,
    "review_num":0
  },
  "title":"트럭스터 어새신 팬츠 공43%",
  "uuid":"P7pZYBbrPMDJMkvBTncit7",
  "coin_rate":null,
  "humanized_created":"끌올 어제"
}
```

> Response: 200 OK (게임머니)

```json
{
  "coin_amount":10,
  "category":"coin",
  "description":"빠른 판매 원합니다",
  "images":[
    {
      "url":"http://localhost:8000/media/images/posts/post_rVvDZa9Mk28f58ch2AXuN0.jpeg",
      "display_order":0
    },
    ...
  ],
  "price":34000,
  "seller":{
    "uuid":"aVYk2fG5yWETFJwCiGbthF",
    "username":"asdf",
    "profile":null,
    "review_num":0
  },
  "title":"트럭스터 어새신 팬츠 공43%",
  "uuid":"gcWDK3d3EAQB7ecrZ33ejB",
  "coin_rate":3400,
  "humanized_created":"끌올 어제"
}
```

### Response Parameters

### 판매자 관련

| Parameters | Type | Required | description                                                  |
| ---------- | ---- | -------- | ------------------------------------------------------------ |
| review_num | int  |          | 거래내역 횟수. 정확히는 `거래 내역`이라기보다, 판매자로서 또는 구매자로서 거래 내역을 남긴 횟수의 총합을 의미합니다. |

### 기타

| Parameters | Type | Required | description    |
| ---------- | ---- | -------- | -------------- |
| coin_rate  | int  |          | 억 메소당 원화 |

## [PATCH] Bump Post (끌어올리기)

> https Request Example

```http
POST https://baseUrl/posts/:post_uuid/bump/
```

> Request Body

```json
없음
```

> Response: 204 No Content (성공시)

```json
None
```

> Response: 412 Precondition Failed

```json
{
  "detail":"마지막 끌올 이후 36시간이 지난 후에만 다시 끌올을 할 수 있습니다.",
  "code":"10004"
}
```

> Response: 406 Not Acceptable

```json
{
  "detail":"같은 게시글의 최대 끌올 가능 횟수는 15회입니다.",
  "code":"10005"
}
```
