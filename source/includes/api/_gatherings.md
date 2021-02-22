# Gatherings

### Gathering & Participation 관련 유사한 API 의미 정리

1. `api/gatherings/:uuid/`: gathering detail. 응답값 중 `participants` 필드가 `승인완료된 모임참여자 명단`
2. `api/gatherings/:uuid/participation_requests/`: 특정 모임에 참여신청한 사람들의 리스트(status 상관없음). request.user가 gathering의 host인 경우만 볼 수 있음.
3. `api/gatherings/i_participated/?fields=uuid,images,interests,title,humanized_date,place`: request.user가 과거부터 지금까지 참여했던 모임의 리스트(일종의 user history, request.user가 참여한 모임 중, 방장이 아니였던 모임 리스트)
4. `api/gatherings/?fields=uuid,images,interests,title,humanized_date,place&?host=user_uuid`: query_param에 request.user의 uuid를 보내면, request user가 방장으로 개설한 모임의 리스트를 받아봅니다.

## [POST] Create Gathering

> https Request Example

```http
GET https://baseUrl/api/gatherings/
```

> Request Body: 오프라인인 경우

```json
{
 "date": "2021-3-15",
 "description": "안녕하세요 골프를 좋아하는 사람을 모집합니다.",
 "images": [
  {
   "display_order": 0,
   "original": "images/gatherings/gathering_P6kjacMDyoBQZXHVy3d6Q8.jpeg"
  },
  ...
 ],
 "interests": [
  "fQW6UJWc5eNzvTNxSHK001",
  ...
 ],
 "participants_limit": 4,
 "place": "DEQ2oUXWkEdH8aKVQuEkZ6",
 "title": "스크 골프 모임 모집"
}
```

> Request Body: 오프라인인 경우

```json
{
 "date": "2021-3-15",
 "description": "안녕하세요 골프를 좋아하는 사람을 모집합니다.",
 "images": [
  {
   "display_order": 0,
   "original": "images/gatherings/gathering_P6kjacMDyoBQZXHVy3d6Q8.jpeg"
  },
  ...
 ],
 "interests": [
  "fQW6UJWc5eNzvTNxSHK001",
  ...
 ],
 "participants_limit": 4,
 "is_online": true,
 "title": "스크 골프 모임 모집"
}
```

> Response: 200 OK

```json
{
	"uuid":"CgFKwB94x4LM4DZLAUNRHU",
	"place":"DEQ2oUXWkEdH8aKVQuEkZ6",
	"images":[
		{
			"url":"http://localhost:8000/media/images/gatherings/gathering_P6kjacMDyoBQZXHVy3d6Q8.jpeg",
			"height":1600,
			"width":1600,
			"display_order":0
		},
    ...
	],
	"is_online":false,
	"participants_limit":4,
	"interests":[
		"fQW6UJWc5eNzvTNxSHK003",
    ...
	],
	"title":"스크 골프 모임 모집",
	"description":"안녕하세요 골프를 좋아하는 사람을 모집합니다.",
	"date":"2021-03-15",
	"created":"2021-02-07T15:18:39.079946+09:00"
}
```

> Response: 400 Bad Request - 이미 지난 시간에 모임 생성 할 경우

```json
{
	"date":[
		"Ensure this value is greater than or equal to 2021-01-13."
	]
}
```

주의: 날짜, 장소, 인원제한 미정시 null이 아니라 key-value값을 json payload에서 빼주세요 -> 그냥 보내지 말 것.

### 장소 처리 방법

* `is_online`: default 는 false이기 때문에 is_online을 안보내주면 자동으로 false 처리됨.
* 장소 지정(오프라인): `is_online` 는 False, `place`는 선택

* 미정인 경우: `is_online` 는 False, `place`는 없음
* 온라인 경우: `is_online` 는 True, `place`는 없음

### Request Parameters

| Parameters         | Type    | Required | description                                                  |
| ------------------ | ------- | -------- | ------------------------------------------------------------ |
| date               | string  | false    | 날짜 미정인 경우: null                                       |
| place              | string  | false    | 장소 미정인 경우: null<br>온라인인 경우: place 값을 빼고(null), is_online=true |
| participants_limit | integer | false    | 인원제한 없음: null<br>현재 100명이 최대임.                  |
| is_online          | bool    | false    | 온라인인 경우 true로 지정, 값이 없으면 default로 false로 알아서 설정됨. |

### Response Parameters

| Parameters         | Type     | description                                                  |
| ------------------ | -------  | ------------------------------------------------------------ |
| date               | string      | 날짜 미정인 경우: null                                       |
| place              | string      | 장소 미정인 경우: null |
| participants_limit | integer     | 미정일 경우: null                                           |

## [PATCH] Update Gathering

> https Request Example

```http
PATCH https://baseUrl/api/gatherings/:uuid/
```

> Request Body

```json
{
	"date":"2021-3-15",
	"description":"안녕하세요 골프를 좋아하는 사람을 모집합니다.",
	"images":[
		{
			"display_order":0,
			"original":"images/gatherings/gathering_P6kjacMDyoBQZXHVy3d6Q8.jpeg"
		},
    ...
	],
	"interests":[
		"fQW6UJWc5eNzvTNxSHK001",
    ...
	],
	"is_open":false,
	"participants_limit":4,
	"place":"DEQ2oUXWkEdH8aKVQuEkZ6",
	"title":"스크 골프 모임 모집"
}
```

> Response: 200 OK

```json
{
 "date": "2021-3-15",
 "description": "안녕하세요 골프를 좋아하는 사람을 모집합니다.",
 "images": [
  {
   "display_order": 0,
   "original": "images/gatherings/gathering_P6kjacMDyoBQZXHVy3d6Q8.jpeg"
  },
  ..
 ],
 "interests": [
  "fQW6UJWc5eNzvTNxSHK001",
  "fQW6UJWc5eNzvTNxSHK003"
 ],
 "is_open": false,
 "participants_limit": 4,
 "place": "DEQ2oUXWkEdH8aKVQuEkZ6",
 "title": "스크 골프 모임 모집"
}
```

### Request Body

| Parameters | Type   | Required | description                         |
| ---------- | ------ | -------- | ----------------------------------- |
| is_open    | string | false    | 방장이 신청을 마감하고 싶을 때 true |

## [GET] Gathering Detail

> https Request Example

```http
GET https://baseUrl/api/gatherings/:uuid/
```

> Request Body

```json
없음
```

> Response: 200 OK

```json
{
    "uuid": "8VTrapmbNJSNksiU98qWwB",
    "host": {
        "uuid": "NgUwhvm8xh3pVwzbJUy5qu",
        "username": "도도한도도새",
        "profile": "http://localhost:8000/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
        "gender": "male",
        "birth": "1980-01-10",
        "profession": {
            "uuid": "6jPaGgDuJeeVffM7SXTBGx",
            "name": "의사",
            "emoji": "👨‍⚕️"
        },
        "status": "accepted",
        "age": 40
    },
    "place": {
        "uuid": "DEQ2oUXWkEdH8aKVQuEkZ6",
        "sido": "서울특별시",
        "sigugun": "강남구",
        "dongmyun": "역삼동",
        "ri": "",
        "road_address": "강남구 역삼동"
    },
    "images": [
        {
            "url": "http://localhost:8000/media/images/gatherings/gathering_P6kjacMDyoBQZXHVy3d6Q8.jpeg",
            "height": 1600,
            "width": 1600,
            "display_order": 0
        },
        ...
    ],
    "is_online": false,
    "participants_limit": null,
    "interests": [
        {
            "uuid": "fQW6UJWc5eNzvTNxSHK001",
            "name": "액티비티",
            "emoji": "🪂",
            "image": "http://localhost:8000/media/images/interests/interest_P6kjacMDyoBQZXHVy3d6Q8.jpeg"
        },
        ...
    ],
    "title": "스크 골프 모임 모집",
    "description": "안녕하세요 골프를 좋아하는 사람을 모집합니다.",
    "date": "2022-01-15",
    "created": "2021-02-07T18:19:41.482130+09:00",
    "participants": [
        {
            "uuid": "4rXVs57UG3RUKXHJiPV004",
            "username": "pirwin",
            "profile": "http://localhost:8000/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
            "gender": "male",
            "birth": "1990-01-10",
            "profession": {
                "uuid": "6jPaGgDuJeeVffM7SXTBGx",
                "name": "의사",
                "emoji": "👨‍⚕️"
            },
            "interests": [],
            "status": "pending",
            "age": 30
        },
        ...
    ],
    "my_participation_request": [
      {
        "uuid": "caFjN4EVZU6vD4tPBQVdgT",
        "status": "accepted"
      }
    ]
    "humanized_date": "11개월 후",
    "humanized_created": "2분 전",
    "status": "closed",
    "display_date": "1월 15일 토요일"
}
```

주의: host 필드에는 interests 필드가 없습니다. (쿼리 최적화 위해)

### Response Parameters

### 시간 관련

| Parameters        | Type | Required | description                                                  |
| ----------------- | ---- | -------- | ------------------------------------------------------------ |
| created           | str  |          | API 요청시간에서 모임이 생성된 시간을 표시합니다.            |
| humanized_created | str  |          | API 요청시간에서 모임이 생성된 시간 차이를 읽기 좋은 시간으로 표시합니다. 초, 분, 시간, 일, 년 순으로 표시 됩니다. ex) 1분 전, 2일 전 |
| date              | str  |          | 모임 약속 시간을 표시합니다. ex) 2021-01-15 (update 화면에서 필요함) |
| display_date      | str  |          | 모임 약속 시간을 표시합니다. ex) 1월 2일 금요일              |
| humanized_date    | str  |          | humanized 된 모임 약속 시간을 표시합니다. ex) 한달 후        |

### 기타

| Parameters               | Type       | Required | description                                                  |
| ------------------------ | ---------- | -------- | ------------------------------------------------------------ |
| my_participation_request | list[dict] |          | request_user의 gathering에 대한 참여 상태를 표시합니다. (null: 미참여, pending:참여대기, accepted: 승인, withdrawn: 승인 취소, decline: 거절, decllined_confirmed: 거절 확인) |
| participants             | dict       |          | 모임참여신청이 승인된 사람들의 리스트                        |
| status                   | str        |          | `open`, `closed` : 방장이 is_open 필드를 false로 처리했거나, 모임의 날짜가 지났을 경우 자동으로 close됨 |

## [GET] Gathering 통계정보

> https Request Example

```http
GET https://baseUrl/api/gatherings/:uuid/stats/
```

> Request Body

```json
없음
```

> Response: 200 OK

```json
{
  "male": 2,
  "female": 0,
  "average_age": 31.0,
  "max_age": 31,
  "min_age": 31,
  "professions": [
    {
      "name": "의료인",
      "emoji": "👨‍⚕️",
      "count": 2
    }
  ]
}
```

## [GET] Gathering List

> https Request Example

```http
GET https://baseUrl/api/gatherings/
GET https://baseUrl/api/gatherings/?fields=uuid,images,interests,title,humanized_date,place&host=NgUwhvm8xh3pVwzbJUy5qu
```

> Request Body

```json
없음
```

> Response: 200 OK

```json
{
  "next": "http://localhost:8000/api/gatherings/?cursor=cD0yMDIxLTAyLTA0KzA4JTNBMzAlM0EzMi44NTE1NzIlMkIwMCUzQTAw&fields=uuid%2Ctitle%2Cinterests%2Cdescription%2Chumanized_created%2Cimages%2Cis_open%2Chumanized_date%2Cdate",
  "previous": null,
  "results": [
    {
      "uuid": "NfBdJ5kZyngHXFbmCScWee",
      "images": [
        {
          "url": "http://localhost:8000/media/images/gatherings/gathering_P6kjacMDyoBQZXHVy3d6Q8.jpeg",
          "height": 166,
          "width": 304,
          "display_order": 0
        },
        ...
      ],
      "interests": [
        {
          "uuid": "fQW6UJWc5eNzvTNxSHK003",
          "name": "경제",
          "emoji": "💰",
          "image": "http://localhost:8000/media/images/interests/interest_P6kjacMDyoBQZXHVy3d6Q8.jpeg"
        },
        ...
      ],
      "title": "스크 골프 모임 모집",
      "date": "2021-03-15",
      "humanized_date": "한달 후",
      "place": {
        "uuid": "DEQ2oUXWkEdH8aKVQuEkZ6",
        "sido": "서울특별시",
        "sigugun": "강남구",
        "dongmyun": "역삼동",
        "ri": "",
        "road_address": "강남구 역삼동"
      },
    },
    ...
  ]
}
```

### Query Parameters

| Parameters | Type | Required | 선택가능한 상태 값과 설명                                    |
| ---------- | ---- | -------- | ------------------------------------------------------------ |
| fields     | str  | false    | graphql과 비슷한 형태로, 응답값에 포함할 필드명을 `,`(공백없음)로 분리해서 지정해줍니다.<br><br />예) ?fields=uuid,images,interests,title,humanized_date,place |
| host       | uuid | false    | 내가 만든 모임 리스트를 조회하고 싶을 때 사용합니다.         |

## [GET] 내가 참여한 모임 리스트

> https Request Example

```http
GET https://baseUrl/api/gatherings/i_participated/?fields=uuid,images,interests,title,humanized_date,place
```

> Request Body

```json
없음
```

> Response: 200 OK

```json
{
 "next": null,
 "previous": null,
 "results": [
  {
   "uuid": "8VTrapmbNJSNksiU98qWwB",
   "images": [
    {
     "display_order": 0,
     "height": 1600,
     "url": "http://localhost:8000/media/images/gatherings/gathering_P6kjacMDyoBQZXHVy3d6Q8.jpeg",
     "width": 1600
    },
    ...
   ],
   "interests": [
    {
     "emoji": "",
     "name": "액티비티",
     "uuid": "fQW6UJWc5eNzvTNxSHK001"
    },
    ...
   ],
   "title": "스크 골프 모임 모집",
   "humanized_date": "내일",
   "place": {
    "uuid":"FGNj3LhyuQt9umUTNiD38a",
    "sido":"서울특별시",
    "sigugun":"강남구",
    "dongmyun":"논현동",
    "ri":"",
    "road_address":"강남구 논현동"
   }
  }
 ]
}
```

주의: 내가 방장인 경우 자동으로 참여자 목록에 추가되지만, 본 API에서는 `내가 참여했지만, 방장이 아닌 모임 리스트`를 가져옵니다.

### Query Parameters

| Parameters | Type | Required | 선택가능한 상태 값과 설명                                    |
| ---------- | ---- | -------- | ------------------------------------------------------------ |
| fields     | str  | false    | graphql과 비슷한 형태로, 응답값에 포함할 필드명을 `,`(공백없음)로 분리해서 지정해줍니다.<br><br />예) ?fields=uuid,images,interests,title,humanized_date,place |

## [GET]모임에 참여신청한 유저 리스트

> https Request Example

```http
GET https://baseUrl/api/gatherings/:gathering_uuid/participation_requests/
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
			"uuid":"S5pw2YsFUBRhNVVX56uL4u",
			"gathering":"8VTrapmbNJSNksiU98qWwB",
			"participant":{
				"uuid":"TMuFZ8aJBvt3RyWzz4gEsq",
				"username":"edward29",
				"profile":"http://localhost:8000/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
				"gender":"male",
				"birth":"1990-01-10",
				"profession":{
					"uuid":"6jPaGgDuJeeVffM7SXTBGx",
					"name":"의사",
					"emoji":"👨‍⚕️"
				},
				"status":"pending",
				"age":30
			},
			"introduction":"Qui corrupti dolo....ur ipsa. Atque totam consequatur.",
			"status":"pending"
		},
    ...
	]
}
```

방장이 모임 멤버를 승인하는 화면에서 사용합니다. 리스트에서 방장 자신은 제외되며, request.user가 방장인 경우만 요청이 가능합니다.

> Response: 403 Forbidden - 방장이 아닌 유저가 요청시

```json
{
 "detail": "You do not have permission to perform this action."
}
```

### Response Parameters

| Parameters | Type | Required | description                      |
| ---------- | ---- | -------- | -------------------------------- |
| uuid       |      |          | Participation 객체의 uuid입니다. |

## [POST] Flag Gathering (신고하기)

> https Request Example

```http
POST https://baseUrl/api/gatherings/flag/
```

> Request Body

```json
{
 "gathering": "8VTrapmbNJSNksiU98qWwB",
 "flag": "removal suggestion",
 "reason": "불쾌한 모임입니다."
}
```

> Response: 201 Created

```json
{
  "id": 1,
  "gathering": "8VTrapmbNJSNksiU98qWwB",
  "reason": "불쾌한 모임입니다.",
  "flag": "removal suggestion"
}
```

주의: [Comment Flag](#post-flag-comment)와 API와는 flag payload 필드의 string값이 다릅니다. 또한, Comment 신고시와는 달리 reason 필드가 추가되었습니다.