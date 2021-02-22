# Accounts

## [POST] Register User

> https Request Example

```http
POST https://baseUrl/api/registration/
```

> Request Body

```json
{
 "avatar": "images/users/avatars/avatar_dIdjeka9Mk28fsidje8X29d.jpeg",
 "birth": "1984-10-23",
 "carrier": "SK telecom",
 "foreigner": false,
 "gender": "male",
 "interests": [
  "fQW6UJWc5eNzvTNxSHK000",
  "fQW6UJWc5eNzvTNxSHK001"
 ],
 "mobile": "01090991005",
 "name": "손흥민",
 "profession": "6jPaGgDuJeeVffM7SXTBGx",
 "profile": "images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
 "unique_in_site": "239dfj239dfjasdf",
 "unique_key": "asdfi23jadsf92",
 "username": "도도",
 "verification_files": [
  {
   "display_order": 0,
   "original": "images/users/verifications/verification_aKJXhaUGtobGKWMJSRHiti.jpeg"
  },
  ...
 ]
}
```

> Response: 201 Created

```json
{
 "access": "eyJ0eXAiOiJKV1QiL...s3pAukovReIwA",
 "refresh": "eyJ0eXAiOiJKV...bX609JyklIKk",
 "user": {
  "age": 36,
  "birth": "1984-10-23",
  "gender": "male",
  "interests": [
   {
    "emoji": "⛳",
    "name": "골프",
    "uuid": "fQW6UJWc5eNzvTNxSHK000",
    "image": "http://localhost:8000/media/images/interests/interest_P6kjacMDyoBQZXHVy3d6Q8.jpeg"
   },
   ...
  ],
  "profession": {
   "emoji": "👨‍⚕️",
   "name": "의사",
   "uuid": "6jPaGgDuJeeVffM7SXTBGx"
  },
  "profile": "/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
  "status": "pending",
  "username": "도도도새",
  "uuid": "CLS9fByfifGwPy5YQWTQon"
 }
}
```

> Response: 400 Bad Request

```json
{
 "username":[
  "해당 사용자 이름은 이미 존재합니다."
 ]
}
```

회원가입에 사용합니다. 

### 주의: 이미지/파일 업로드

* avatar, profile, verification_files 모두 [presigned_url을 사용하며 먼저 이미지를 S3](#sign-s3)에 업로드 후, 해당 파일의 str 경로를 `POST`합니다.

## [POST] Login Step 1 (Get token)

> https Request Example

```http
POST https://baseUrl/api/auth/mobile/
```

> Request Body

```json
{
 "mobile": "+821012345678"
}
```

> Response: 200 OK

```json
{
 "detail":"We texted you a login code."
}
```

> Response: 404 NotFound (비회원 요청)

```json
{
  "detail": "가입하지 않은 회원입니다",
  "code": "not_found"
}
```

> Response: 406 NotAcceptable (승인 대기 회원 요청)

```json
{
  "detail": "승인 대기중인 회원입니다.",
  "code": "1006"
}
```

> Response: 403 Forbiden (승인 거절된 회원 요청)

```json
{
  "detail": "승인 거절된 회원입니다.",
  "code": "1005"
}
```

로그인을 위한 첫 번째 과정으로 핸드폰 번호를 보내면, 6자리 인증 토큰이 twilio API를 통해 사용자의 휴대폰 SMS로 보내지게 됩니다. 

### 승인 되지 않은 회원 Error Code

- 1005 : 승인 거절된 회원

- 1006 : 승인 대기중인 회원


## [POST] Login Step 2 (Use token)

> https Request Example

```http
POST https://baseUrl/api/token/
```

> Request Body

```json
{
 "mobile": "+821012345678",
 "token": "631321"
}
```

> Response: 200 OK

```json
{
 "access": "eyJ0eXAiO....Ej1HgcQY",
 "refresh": "eyJ0eXAiO....dx392JXI",
 "user": {
  "age": 36,
  "birth": "1984-10-23",
  "gender": "male",
  "interests": [
   {
    "emoji": "⛳",
    "name": "골프",
    "uuid": "fQW6UJWc5eNzvTNxSHK000",
    "image": "http://localhost:8000/media/images/interests/interest_P6kjacMDyoBQZXHVy3d6Q8.jpeg"
   },
   ...
  ],
  "profession": {
   "emoji": "👨‍⚕️",
   "name": "의사",
   "uuid": "6jPaGgDuJeeVffM7SXTBGx"
  },
  "profile": "/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
  "status": "pending",
  "username": "도도",
  "uuid": "aHApG3fYurtg7VMxTEZXtp"
 }
}
```

발급받은 access 토큰을 헤더에 담아 인증이 필요한 API에 요청을 보냅니다.

## [POST] Refresh token

> https Request Example

```http
POST https://baseUrl/api/token/refresh/
```

> Request Body

```json
{
 "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc......E_WUzCU4hLW4"
}
```

> Response: 200 OK

```json
{
 "access": "eyJ0eXAiOiJKV1QiLCJhbG...WI73w3PaVIlie-TlM",
 "refresh": "eyJ0eXAiOiJKV1QiLCJhb...TUXy6ZfZ1-YaY_YoA"
}
```

access 토큰이 만료된 경우 재발급 받습니다.

## [POST] Logout

> https Request Example

```http
POST https://baseUrl/api/logout/
```

> Request Body: 헤더에 refresh 값을 담아서 요청해야 함.

```json
{
 "refresh": "eyJ0eXAiOiJKV1QiLCJhb...TUXy6ZfZ1-YaY_YoA"
}
```

> Response: 200 OK

```json
{
 "detail": "Successfully logged out."
}
```

서버에서 refresh token을 blacklist 합니다. 로그아웃 후에도 access은 valid합니다. 서버에서 access의 수명을 짧게 해야하며, client에서도 삭제해주는 것이 좋습니다.

