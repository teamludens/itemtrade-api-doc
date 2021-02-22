# Users

## [GET] User Check Duplicate

> https Request Examples

```http
GET https://baseUrl/users/check_duplicate/?username=도도한도도새
```

> Request Body

```reStructuredText
없음
```

>  Response: 204 NO CONTENT

```json
없음
```

> Response: 409 Conflict

```json
{"message":"Duplicate record exists."}
```

회원가입시 username중복 여부를 확인합니다.

### Response Parameters

| Parameters | Type | Required | 필터방법 | Descriptions                                                 |
| ---------- | ---- | -------- | -------- | ------------------------------------------------------------ |
| username   | str  | false    | exact    | 회원가입시 username을 사용할 수 있는지 확인합니다. `주의`: 중복여부만 확인하며 인스타그램 형태의 regex 여부는 확인하지 않기 때문에 이는 client에서 확인해야 합니다.<br />`^(?!.*\.\.)(?!.*\.$)[^\W][\w.]{0,29}$` |

## [GET] User Me

> https Request Examples

```http
GET https://baseUrl/users/me/
```

> Request Body

```reStructuredText
없음
```

>  Response: 200 OK

```json
{
  "uuid": "NgUwhvm8xh3pVwzbJUy5qu",
  "username": "도도한도도새",
  "profile": "http://localhost:8000/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
  "gender": "male",
  "birth": "1980-01-10",
  "profession": "의사",
  "interests": [
    {
      "uuid": "fQW6UJWc5eNzvTNxSHK000",
      "name": "골프",
      "emoji": "⛳",
      "image": "http://localhost:8000/media/images/interests/interest_P6kjacMDyoBQZXHVy3d6Q8.jpeg"
    },
		...
  ],
  "status": "accepted",
  "age": 40
}
```

### User 승인 상태

- pending : 승인 대기
- accepted : 승인 완료
- declined : 승인 거절



## [DELETE] User Delete

> https Request Example

```http
DELETE https://baseUrl/users/:uuid/
```

> Request Body

```json
없음
```

> Response: 204 No Content

```json
없음
```

내부적으로는 유저가 삭제되는 것이 아니라, `inactive` 상태로 변경됩니다.

## [PATCH] Update User

> https Request Example

```http
PATCH https://baseUrl/users/:uuid/
```

> Request Body

```json
{
 "interests": [
  "fQW6UJWc5eNzvTNxSHK000",
   ...
 ],
 "profile": "images/users/profiles/profile_dIdjeka9Mk28fsidje8X29d.jpeg",
 "username": "안도도한도도새"
}
```

> Request: 이미지를 null로 지정하는 경우

```json
{
  "profile": null,
}
```

유저의 프로필을 null값으로 수정하는 경우입니다.(실제 profile에 null을 넣는것이 아니라 빈 스트링, `""`을 넣어줘야 동작합니다.) 자신의 프로필 사진을 디폴트 이미지로 변경 할 때 사용합니다.

> Response: 200 OK

```json
{
 "birth": "1980-01-10",
 "gender": "male",
 "interests": [
  "fQW6UJWc5eNzvTNxSHK000"
 ],
 "username": "안도도한도도새",
 "uuid": "NgUwhvm8xh3pVwzbJUy5qu"
}
```

> Response: 400 Bad Request

```json
{
  "username":["A user with that username already exists."]
}
```

유저의 정보를 수정합니다.


### Request Body Parameters

| Parameter | Type   | Required | Description                                                  |
| --------- | ------ | -------- | ------------------------------------------------------------ |
| username  | string | false    | Can only contain letters, numbers, and @/./+/-/_ characters.<br />수정하려는 유저의 `닉네임`으로, 공백이 포함될 수 없습니다.<br />중복 여부 체크는 [user-check-duplicate api](#get-user-check-duplicate) 통해서도 가능합니다. |
| profile   | string | false    | 수정하려는 유저의 `프로필 사진`의 파일명입니다. 파일 확장자를 반드시 포함합니다. |
| interests | list   | false    | 관심사 리스트에서 받아온 uuid의 리스트                       |

<aside class="warning">
   presigned_url을 이용하여 AWS S3와 통신하는 방법은 <a href="#sign-s3">AWS S3</a>를 참고해주세요.
</aside>

