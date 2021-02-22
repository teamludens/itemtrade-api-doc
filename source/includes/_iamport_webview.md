# (Web) Iamport

## [GET] Mobile Authentication Web Page

> https Request Examples

```http
GET https://baseUrl/webviews/imp_auth/?device=android
```

### Query Parameters

| Parameters | Type | Required | Descriptions                                    |
| ---------- | ---- | -------- | ----------------------------------------------- |
| device     | str  | false    | 현재 디바이스 종류(android, ios), default='ios' |

핸드폰 본인인증을 위한 웹 페이지를 반환합니다.

