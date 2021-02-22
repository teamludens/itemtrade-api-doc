# Participations

## [POST] Create Participation

> https Request Example

```http
POST https://baseUrl/api/participations/
```

> Request Body

```json
{
 "gathering": "2gDNm5uXJ6kyjcMYjjhoUF",
 "introduction": "반갑습니다."
}
```

> Response: 201 Created

```json
{
 "introduction": "반갑습니다.",
 "status": "pending",
 "uuid": "FQp4D9DBHoTmAweQ2EWViE"
}
```

> Response: 409 Conflict - 이미 같은 모임에 신청했을 때

```json
{
 "detail": "해당 모임에 이미 참여 신청했습니다.",
 "status_code": 409,
 "code": "10001"
}
```

## [PATCH] Update Participation

>  https Request Example

```http
PATCH https://baseUrl/api/participations/:participation_uuid/
```

> Request Body

```json
{
 "status": "accepted",
}
```

> Response: 200 OK

```json
{
 "gathering": "2gDNm5uXJ6kyjcMYjjhoUF",
 "introduction": "반갑습니다.",
 "status": "accepted",
 "uuid": "HMjsneT3JgQHZ3pUMitVga"
}
```

> Response: 403 Forbidden - 방장이 아닌 유저가 요청시, participant 인 유저가 accept로 수정시

```json
{
  "detail": "이 작업을 수행할 권한(permission)이 없습니다.",
  "status_code": 403,
  "error_code": "permission_denied"
}
```

방장, 유저 모두 수정 요청을 할 수 있지만 permission 이 다릅니다. 

- 방장 : 모든 상태(status) 변경 가능
- 유저(participant가 아닌) : 모든 수정 불가
- 유저(participant인 유저): 아래와 같은 상황만 변경 가능
  - accepted -> withdrawn
  - declined -> declined_confirmed

