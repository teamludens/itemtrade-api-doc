# Sign S3

서버를 거치지 않고 Client에서 직접 S3로 업로드하기 위해 필요합니다. 기존과 다른 점은 API에 따라 저장해야 되는 경로가 달라지는데 이를 Client에서 지정해줘야 한다는 것입니다. (예. user의 avatar 사진의 경우 `"images/teacher_portfolio/"`, 유저 프로필의 경우 `"images/users/profiles/"` 등등)

이렇게 이미지 또는 영상을 업로드하고 난 후, 필요한 API에 업로드된 url을 담아서 다시 요청을 보냅니다. (예. 라이브 썸네일을 업로드한 후, 성공시 live create api에 요청보내기). 이때 **주의해야 할 점은  이미지 영상을 업로드 S3에 업로드할 때와 django db에 해당 url을 기록할 때 그 format이 조금 차이가 난다는 점입니다.**

S3에 업로드가 성공하면 반환되는 key값은 `"media/images/teacher_portfolio/2020/07/3XBfd65ekuxtY3CvsPpPJD.jpeg"` 형태이지만, django db에 기록을 위해 보낼 때는 `media/`를 제거하고 `images/teacher_portfolio/2020/07/3XBfd65ekuxtY3CvsPpPJD.jpeg`와 같이 보내줘야 합니다.

## [POST] Presigned Post Url 생성

> https Request Example

```http
POST https://baseUrl/sign_s3/
```

> Request Body

```json
{
 "mime": "image/jpeg",
 "path": "images/users/profiles/profile_",
 "number": 1
}
```

### Request Body Parameters

| Parameters | Type    | Required | Descriptions                                                 |
| ---------- | ------- | -------- | ------------------------------------------------------------ |
| mime       | string  | true     | Content-Type에서 사용하는 MIME 타입입니다. <br />예) `image/jpeg`,`video/mp4` 등 |
| path       | string  | true     | S3 디렉토리 구조에 맞춰 API별로 client에서 설정하여 보냅니다. 하단 참고 |
| number     | integer | true     | 여러 개의 파일을 동시에 업로드해야 하는 경우에 받아와야 하는 presigned post url의 수를 지정합니다. 예를 들어 상품 썸네일을 여러 개 저장하는 경우 필요합니다. 대부분의 경우 1로 설정하면 됩니다. |

### S3 구조별 path 설정

```text
.
+-- media 
|  +-- images
|    +-- users: 유저 관련
|      +-- 세부사진 분류(복수형)/파일종류(단수형)_랜덤uuid.확장자 구조
|      +-- profiles: 유저 프로필 사진 등
|        +-- profile_G2TdKFK6Awycfi6m5reJjd.jpeg
|        ...
|    +-- posts: Post(=아이템) 관련
|      +-- post_P6kjacMDyoBQZXHVy3d6Q8.jpeg
|      ...
|-- static

```

S3 디렉토리 구조는 위와 같은 형식이다. Presigned post url를 생성하기 위해 요청을 보낼 때 body에 담기는 `path`의 값은 다음과 같이 정해야 한다.

1. media는 제외한다.

2. `"images/users/profiles/profile_"`와 같이 `images/{모델명}/{세부사진 분류(복수형)}/{세부사진 분류_}` 형태를 만들어 준다.

3. 서버에서 요청하면 path 양 옆에 `media/`와  `random_uuid.jpeg`을 달아주기 때문에, client에서는 중간 경로만 만들어주면 된다.

4. 구체적으로는 다음과 같이 보내면 된다.

   유저

   * 유저 프로필: `images/users/profiles/profile_`

   포스팅(=아이템)

   * 모임 사진: `images/posts/post_`

> Response: 201 Created

```json
[
	{
		"url":"https://bucket.s3.amazonaws.com/",
		"fields":{
			"acl":"private",
			"Content-Type":"image/jpeg",
			"x-amz-meta-userid":"NgUwhvm8xh3pVwzbJUy5qu",
			"key":"media/images/users/posts/post_3L5fgCyhLwTX5LWfT7dhya.jpeg",
			"x-amz-algorithm":"AWS4-HMAC-SHA256",
			"x-amz-credential":"AKIAJACNW25AK2YBNKXQ/20210117/region/s3/aws4_request",
			"x-amz-date":"20210117T082812Z",
			"policy":"eyJle......ICIyMDIxMDExN1QwODI4MTJaIn1dfQ==",
			"x-amz-signature":"4f157d4cd25e0aff1e70c32f1fca3a7bb62583fbbdc29fe0298d7ad3265497e3"
		}
	}
]
```

## [POST] 생성된 Presigned Url로 S3에 직접 업로드하기

> https Request Example

```http
POST 생성된 url(sign_s3에 받은 응답값의 "url")
POST https://bucket.s3.amazonaws.com/
```

> Request Body

```json
# sign_s3에서 받은 응답값의 "fields" 들을 form-data로 만들어주고, "file"이라는 form-data도 추가한다.

"acl": "private"
"Content-Type": "image/jpeg"
"x-amz-meta-userid": "NgUwhvm8xh3pVwzbJUy5qu",
"key": "media/images/users/posts/post_34jhrgTKDNEwyyUwX8vs9J.jpeg",
"x-amz-algorithm": "AWS4-HMAC-SHA256",
"x-amz-credential": "AKIAR5WQRDCTEM6S5CIU/20200712/region/s3/aws4_request",
"x-amz-date": "20200712T162900Z",
"policy": "eyJleHBpcmF0aW9uIjogIjIwMjAt...ifV19",
"x-amz-signature": "505feab7655502a3968b6beb619cd5bceafbb3a32430c556d77c70e04fd90d2d"
"file": 업로드할 파일
```

> Response: 204 No Content (업로드 성공)

```reStructuredText
없음
```

## [DELETE] 업로드 된 파일 삭제하기 (실패시 rollback 개념)

이미지나 영상을 업로드 한 후 다른 API에 요청을 보냈을 때, 실패한 다면 일종의 rollback 개념으로 이미지/영상을 삭제해줘야 합니다. 예를 들어, 라이브 썸네일 업로드 후 라이브 생성 요청을 하였으나 실패하는 경우 업로드한 라이브 썸네일을 지워줍니다.

(참고로, update/delete api에 대해서는 client가 이와 같이 직접 key값을 보내 객체를 지워야할 필요는 없습니다.)

> https Request Example

```http
DELETE https://baseUrl/sign_s3/
```

> Request Body

```json
# sign_s3에서 받은 응답값 "fields"에 포함된 key 값을 보낸다.

{
	"key": "media/images/users/posts/post_34jhrgTKDNEwyyUwX8vs9J.jpeg"
}
```

> Response: 204 No Content (삭제 성공)

```reStructuredText
없음
```