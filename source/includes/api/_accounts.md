# Accounts

## [POST] Register User

> https Request Example

```http
POST https://baseUrl/registration/
```

> Request Body

```json
{
	"birth":"1984-10-23",
	"carrier":"SK telecom",
	"foreigner":false,
	"gender":"male",
	"imp_resp":{
		"code":0,
		"message":null,
		"response":{
			"birth":887829380,
			"birthday":"1990-07-20",
			"carrier":"SKT",
			"certified":true,
			"certified_at":1610889283,
			"foreigner":false,
			"gender":"female",
			"imp_uid":"imp_289888049273",
			"merchant_uid":"ORD202039131-0002931",
			"name":"손흥민",
			"origin":"https://api.alyke.app/webviews/imp_auth/?device=android",
			"pg_provider":"danal",
			"pg_tid":"202101172046340829382718",
			"phone":"01012345678",
			"unique_in_site":"MC0GCCqG....AfRO48=",
			"unique_key":"iEfjM/HqDwP....5nVRikslpCUE0uQwHVtrdw=="
		}
	},
	"mobile":"+821012345678",
	"name":"손흥민",
	"profile":"images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
	"unique_in_site":"239dfj239dfjasdf",
	"unique_key":"asdfi23jadsf92",
	"username":"도도도새"
}
```

> Response: 201 Created

```json
{
	"access":"eyJ0eXAiOiJKV1QiLCJh...BwlpX8",
	"refresh":"eyJ0eXAiOiJKV1Q...X9hOmZ4",
	"user":{
		"profile":"http://localhost:8000/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
		"status":"active",
		"username":"도도도새",
		"uuid":"PdLT2G8ae6sdWWuTgqigiS"
	}
}
```

> Response: 400 Bad Request: mobile, unique_in_site, unique_key, username 등이 겹칠 때

```json
{
 "code": "invalid",
 "detail": {
  "mobile": [
   "이 필드는 반드시 고유(unique)해야 합니다."
  ],
  "unique_in_site": [
   "유저의 unique in site은/는 이미 존재합니다."
  ],
  "unique_key": [
   "유저의 unique key은/는 이미 존재합니다."
  ],
  "username": [
   "해당 사용자 이름은 이미 존재합니다."
  ]
 }
}
```

### Request Parameters

| Parameters | Type | Required | description                                               |
| ---------- | ---- | -------- | --------------------------------------------------------- |
| imp_resp   | json | false    | 아임포트에서 오는 응답값을 그대로 보내서 db에 저장합니다. |

회원가입에 사용합니다. 

Unique한 필드들: `mobile`, `unique_key_in_site`, `unique_key`, `username` 

### 주의: 이미지/파일 업로드

* avatar, profile, verification_files 모두 [presigned_url을 사용하며 먼저 이미지를 S3](#sign-s3)에 업로드 후, 해당 파일의 str 경로를 `POST`합니다.

## [POST] Login Step 1 (Get token)

> https Request Example

```http
POST https://baseUrl/auth/mobile/
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

로그인을 위한 첫 번째 과정으로 핸드폰 번호를 보내면, 6자리 인증 토큰이 twilio API를 통해 사용자의 휴대폰 SMS로 보내지게 됩니다. 

### 승인 되지 않은 회원 Error Code


## [POST] Login Step 2 (Use token)

> https Request Example

```http
POST https://baseUrl/token/
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
POST https://baseUrl/logout/
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

> Response: 401 Unauthorized - 이미 로그아웃한 토큰으로 재시도시

```json
{
 "detail": "Token is blacklisted"
}
```

서버에서 refresh token을 blacklist 합니다. 로그아웃 후에도 access은 valid합니다. 서버에서 access의 수명을 짧게 해야하며, client에서도 삭제해주는 것이 좋습니다.

