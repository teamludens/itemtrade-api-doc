# Comments

[django-commentx-xtd](https://github.com/danirus/django-comments-xtd)라는 외부 라이브러리를 사용하여, 불필요한 필드들이 다소 있습니다. 사용시 무시하시면 됩니다.

## [POST] Create Comment

> https Request Example

```http
POST https://baseUrl/api/comments/
```

> Request Body

```json
{
 "comment": "댓글 내용입니다.",
 "content_type": "gatherings.gathering",
 "object_pk": "8VTrapmbNJSNksiU98qWwB"
}
```

> Response: 201 Created

```json
{
 "comment": "코멘트 내용입니",
 "content_type": "gatherings.gathering",
 "followup": false,
 "honeypot": "",
 "id": 1,
 "object_pk": "8VTrapmbNJSNksiU98qWwB",
 "reply_to": 0,
 "security_hash": "22f40d2...abd3",
 "timestamp": "1612532499",
 "humanized_created": "지금"
}
```

### Request Parameters

| Parameters   | Type | Required | description                                                  |
| ------------ | ---- | -------- | ------------------------------------------------------------ |
| content_type | str  |          | 현재는 gathering에 대한 코멘트만 달 수 있으므로, `gatherings.gathering`으로 고정해서 값을 보내주면됩니다. 향후 다른 모델에도 댓글을 달 경우 이 값을 바꿔주면 됩니다. (django app name + model name의 조합 형태입니다.) |
| object_pk    | str  |          | gathering의 uuid                                             |

## [GET] Comment List

> https Request Example

```http
GET https://baseUrl/api/comments/gatherings-gathering/:gathering_uuid/
GET https://baseUrl/api/comments/gatherings-gathering/8VTrapmbNJSNksiU98qWwB/
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
   "by_host": true,
   "comment": "코멘트 내용입니다",
   "humanized_created": "지금",
   "id": 3,
   "user": {
    "age": 40,
    "birth": "1980-01-10",
    "gender": "male",
    "interests": [
     {
      "emoji": "⛳",
      "name": "골프",
      "uuid": "fQW6UJWc5eNzvTNxSHK000"
     },
     ...
    ],
    "profession": {
     "emoji": "👨‍⚕️",
     "name": "의사",
     "uuid": "6jPaGgDuJeeVffM7SXTBGx"
    },
    "profile": "http://localhost:8000/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
    "status": "accepted",
    "username": "도도한도도새",
    "uuid": "NgUwhvm8xh3pVwzbJUy5qu"
   }
  },
  ...
 ]
}
```

### Request Parameters

| Parameters | Type | Required | description                           |
| ---------- | ---- | -------- | ------------------------------------- |
| by_host    | bool |          | 방장이 쓴 댓글인지 아닌지 확인합니다. |

## [GET] Comment Count

> https Request Example

```http
GET https://baseUrl/api/comments/gatherings-gathering/:gathering_uuid/count/
GET https://baseUrl/api/comments/gatherings-gathering/8VTrapmbNJSNksiU98qWwB/count/
```

> Request Body

```json
없음
```

> Response: 200 OK

```json
{
 "count": 2
}
```

## [PATCH] Update Comment

> https Request Example

```http
PATCH https://baseUrl/api/comments/:comment_id/
PATCH https://baseUrl/api/comments/1/
```

> Request Body

```json
{
 "comment": "수정된 메세지"
}
```

> Response: 200 OK

```json
{
 "comment": "수정된 메세지",
 "id": 1
}
```

## [DELETE] Delete Comment

> https Request Example

```http
DELETE https://baseUrl/api/comments/:comment_id/
DELETE https://baseUrl/api/comments/1/
```

> Request Body

```json
없음
```

> Response: 204 No Content

```json
없음
```

DB에서 삭제되지 않고, is_public=False 및 is_removed=True flag 처리되어 Comment List API 등에서 필터링됩니다.

## [POST] Flag Comment (신고하기)

> https Request Example

```http
POST https://baseUrl/api/comments/flag/
```

> Request Body

```json
{
 "comment": "1",
 "flag": "report"
}
```

> Response: 201 Created

```json
{
 "comment": 1,
 "flag": "removal suggestion"
}
```

