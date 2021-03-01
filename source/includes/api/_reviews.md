# Reviews

### Feedback과 Review의 개념 정리

* Feedback: 사용자에 대한 후기 (당근의 매너평가와 유사)
* Review: 거래에 대한 후기 (당근의 거래후기와 유사)

## [POST] Create Review

> https Request Example

```http
POST https://baseUrl/reviews/
```

> Request Body

```json
{
  "body":"잘 쓰겠습니다",
  "feedback":[
    "cGjVqpjZLbrkag8CZgcaxx",
    ...
  ],
  "post":"P7pZYBbrPMDJMkvBTncit7",
  "target_user":"4XXDJX9WipJpcakuy3ibcH",
  "sentiment":"jzmQot3xF4LkoAogk8mXWH"
}
```

> Response: 201 Created

```json
{
  "post":"P7pZYBbrPMDJMkvBTncit7",
  "body":"잘 쓰겠습니다",
  "feedback":[
    "cGjVqpjZLbrkag8CZgcaxx",
    ...
  ],
  "target_user":"4XXDJX9WipJpcakuy3ibcH",
  "sentiment":"jzmQot3xF4LkoAogk8mXWH"
}
```

> Response: 400 Bad Request

```json
{
  "detail":{
    "non_field_errors":[
      "필드 author, target_user, post 는 반드시 고유(unique)해야 합니다."
    ]
  },
  "code":"invalid"
}
```

author, target_user, post의 조합은 unique합니다. 

## [GET] Review List

> https Request Example

```http
GET https://baseUrl/reviews/
GET https://baseUrl/reviews/?target_user=NgUwhvm8xh3pVwzbJUy5qu&fields=uuid,author,target_user,body,feedback
```

> Request Body

```json
없음
```

> Response: 200 OK (기본 url에 ?field parameter없이 GET 요청시)

```json
{
  "next":null,
  "previous":null,
  "results":[
    {
      "uuid":"b5vA2pQu2iVAsn4fSZqY2L",
      "target_user":"4XXDJX9WipJpcakuy3ibcH",
      "body":"잘 쓰겠습니다",
      "feedback":[
        {
          "uuid":"cGjVqpjZLbrkag8CZgcaxx",
          "body":"시간 약속을 잘 지켜요."
        },
        ...
      ]
    },
    ...
  ]
}
```

> Response: 200 OK (url에 fields parameter 지정시 - uuid,author,target_user,body,feedback)

```json
{
  "next":null,
  "previous":null,
  "results":[
    {
      "uuid":"b5vA2pQu2iVAsn4fSZqY2L",
      "author":{
        "uuid":"aVYk2fG5yWETFJwCiGbthF",
        "username":"asdf",
        "profile":"http://localhost:8000/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
        "reviews_by_others":3
      },
      "target_user":"4XXDJX9WipJpcakuy3ibcH",
      "body":"잘 쓰겠습니다",
      "feedback":[
        {
          "uuid":"cGjVqpjZLbrkag8CZgcaxx",
          "body":"시간 약속을 잘 지켜요."
        },
        ...
      ]
    },
    ...
  ]
}
```

### Query Parameters

| Parameters  | Type | Required | 선택가능한 상태 값과 설명                                    |
| ----------- | ---- | -------- | ------------------------------------------------------------ |
| target_user | uuid | false    | 특정 유저에 대해 남겨진 거래 후기                            |
| fields      | str  | false    | graphql과 비슷한 형태로, 응답값에 포함할 필드명을 `,`(공백없음)로 분리해서 지정해줍니다.<br\>예) ?fields=uuid,author,target_user,body,feedback |

### Response Parameters

| Parameters        | Type | description                                                  |
| ----------------- | ---- | ------------------------------------------------------------ |
| reviews_by_others | int  | 거래내역 건수(=다른 사람에 의해 남겨진 거래 후기 수. 현재 로직에서 transaction을 catch할 수 있는 유일한 방법)<br\>본인이 작성한 거래후기는 제외. |

## [PUT] Update Review

> https Request Example

```http
PUT https://baseUrl/reviews/:uuid/
```

> Request Body (아이템)

```json
{
  "body":"(수정) 잘 쓰겠습니다",
  "feedback":[
    "aisdj233isrkag8CZgcaxx",
    ...
  ],
  "post":"gcWDK3d3EAQB7ecrZ33ejB",
  "target_user":"4XXDJX9WipJpcakuy3ibcH"
}
```

> Response: 200 OK (아이템)

```json
{
  "body":"(수정) 잘 쓰겠습니다",
  "feedback":[
    "aisdj233isrkag8CZgcaxx",
    ...
  ],
  "post":"gcWDK3d3EAQB7ecrZ33ejB",
  "target_user":"4XXDJX9WipJpcakuy3ibcH"
}
```

* HTTP method는 `PUT`을 사용해야 합니다.
* 실제 post와 target_user는 바뀔 일이 없지만(바뀐다면 review를 update할 게 아니라 새로 만드는 개념이기 때문에), 디폴트로 넣어줍니다. (data를 serialize하는 단계에서 author(=request_user), target_user, post의 unique_together validation check을 위함.)

## [GET] Review Detail

> https Request Example

```http
GET https://baseUrl/reviews/:uuid/
```

> Request Body

```json
없음
```

> Response: 200 OK

```json
{
  "uuid":"b5vA2pQu2iVAsn4fSZqY2L",
  "sentiment":"jzmQot3xF4LkoAogk8mXWH",
  "author":{
    "uuid":"aVYk2fG5yWETFJwCiGbthF",
    "username":"asdf",
    "profile":"http://localhost:8000/media/images/users/profiles/profile_rVvDZa9Mk28f58ch2AXuNt.jpeg",
    "reviews_by_others":3
  },
  "target_user":"4XXDJX9WipJpcakuy3ibcH",
  "body":"잘 쓰겠습니다",
  "feedback":[
    {
      "uuid":"cGjVqpjZLbrkag8CZgcaxx",
      "body":"시간 약속을 잘 지켜요."
    },
    ...
  ]
}
```

### Response Parameter

| Parameters        | Type | Required | description                                                  |
| ----------------- | ---- | -------- | ------------------------------------------------------------ |
| reviews_by_others | int  |          | 거래내역 횟수. 정확히는 `거래 내역`이라기보다, 판매자로서 또는 구매자로서 거래 내역을 남긴 횟수의 총합을 의미합니다. |

## [GET] Sentiment List

> https Request Example

```http
GET https://baseUrl/sentiments/
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
      "uuid":"j5ZRavY9gh6yv2npdoTXUW",
      "emoji":"😀",
      "score":1,
      "text":"최고예요!",
      "available_feedback":[
        {
          "uuid":"TkuJNoHPB8ZAbdCqqccWpD",
          "body":"친절하게 대해주셨어요."
        },
        ...
      ]
    },
    ...
  ]
}
```

3가지 sentiment에 따라 선택할 수 있는 feedback의 종류가 다릅니다. 

나쁜 sentiment부터 좋은 sentiment순서대로 정렬됩니다. 예) score기준: -1 -> 0 -> 1, text기준: 별로예요 -> 좋아요 -> 최고예요

### Query Parameters

| Parameters | Type | Required | 선택가능한 상태 값과 설명               |
| ---------- | ---- | -------- | --------------------------------------- |
| score      | int  | false    | `-1`, `0`, `1` : score 별로 필터링 가능 |

### Response Parameters

| Parameters         | Type | description                                     |
| ------------------ | ---- | ----------------------------------------------- |
| score              | int  | `-1`: 별로예요<br> `0`: 좋아요<br>`1`: 최고예요 |
| available_feedback | list | sentiment별로 선택가능한 feedback 옵션들        |

## 