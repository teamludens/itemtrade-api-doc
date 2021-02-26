# Posts

## [POST] Create Post

> https Request Example

```http
GET https://baseUrl/api/posts/
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
