# Places

## [GET] List Place

> https Request Example

```http
GET https://baseUrl/api/places/
GET https://baseUrl/api/places/?search_text=역삼
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
			"uuid":"dT4WejtBjvSFXZch3TKGuD",
			"sido":"서울특별시",
			"sigugun":"강남구",
			"dongmyun":"",
			"ri":"",
			"road_address":"강남구"
		},
		{
			"uuid":"gie9HRAkAJGRuwTQVE4gNi",
			"sido":"서울특별시",
			"sigugun":"강남구",
			"dongmyun":"대치동",
			"ri":"",
			"road_address":"강남구 대치동"
		},
		{
			"uuid":"dQToePdYPGgyfuBj4re3J7",
			"sido":"서울특별시",
			"sigugun":"강남구",
			"dongmyun":"삼성동",
			"ri":"",
			"road_address":"강남구 삼성동"
		},
    ...
	]
}
```

* Pagination은 1000개 까지 한번에 오게 됩니다.
* 정렬 순서는 한글 가나다 순입니다. 구만 담긴 주소(예. 서울시 강남구)가 먼저 오고, 그 뒤에 해당 구의 동까지 포함한 세부 주소(예. 서울시 강남구 대치동) 이 오게 됩니다.

### Query Parameters

| Parameters  | Type | Required | 필터방법 | 선택가능한 상태 값과 설명                                    |
| ----------- | ---- | -------- | -------- | ------------------------------------------------------------ |
| search_text | str  | false    | contains | 해당 한글 값을 포함한 주소를 반환합니다. db에 검색을 위해 필드가 추가되어 있으며(API에 노출X), 공백는 모두 trimming되어 있습니다.<br /> 예. "서울시강남구", "서울시강남구대치동" |

### Response Parameters

| Parameters   | Type | Required | Descriptions                                                 |
| ------------ | ---- | -------- | ------------------------------------------------------------ |
| road_address | str  |          | sigugun만 있는 경우는 알아서 trimming됩니다.<br />예. 서울특별시 강남구만 있는 경우 ->  `강남구` , 서울특별시 강남구 대치동까지 있는 경우 -> `강남구 대치동` |

