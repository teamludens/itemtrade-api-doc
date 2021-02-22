# Iamport

## [POST] 인증 정보 조회

> https Request Examples

```http
POST https://baseUrl/api/imp_auth/
```

> Request Body

```json
{
	"imp_uid": "imp_355752770728"
}
```

### Request Body Parameters

| Parameters | Type   | Required | Descriptions                                         |
| ---------- | ------ | -------- | ---------------------------------------------------- |
| imp_uid    | string | true     | 핸드폰 본인 인증을 완료하면 받는 유저 데이터 조회 값 |


>  Response: 200 OK

```json
{
  "birth":742057200,
  "birthday":"1993-07-08",
  "certified":true,
  "certified_at":1611020779,
  "foreigner":false,
  "mobile":"01090991005",
  "carrier":"SK telecom",
  "gender":"male",
  "imp_uid":"imp_355752770728",
  "merchant_uid":"nobody_1611020752747",
  "name":"신재민",
  "origin":"http://baseUrl/imp_url/",
  "pg_provider":"danal",
  "pg_tid":"202101191045528069323010",
  "unique_in_site":"MC0GCCqGSIb3DQIJAyEAsK2qgU68TvoLpeidQnnU60wcMqDmwLtXNk9eRN1ATac=",
  "unique_key":"pHjiYM6xYF/tqCwRsxWDfoK+Kyjls9AKRVUW0NHoqvJIzFUV1ldDVMWhHjFQ/0NdyQrxhwozHXnMzHbaNFbbLg=="
}
```

### Response Parameters

| Parameters     | Type | Descriptions                                                 |
| -------------- | ---- | ------------------------------------------------------------ |
| birthday       | str  | 생년월일 ('YYYY-MM-DD')                                      |
| foreigner      | bool | 외국인                                                       |
| mobile         | str  | 휴대폰 번호, `아임포트 응답키는 phone이나 ludens db는 mobile` |
| carrier        | str  | 통신사('KT', 'SK telecome',..)                               |
| gender         | str  | 성별('male', 'female')                                       |
| imp_uid        | str  | 인증한 유저 식별 값                                          |
| name           | str  | 이름                                                         |
| unique_in_site | str  | 사이트 별 개인식별 고유 키                                   |
| unique_key     | str  | 개인식별 고유 키                                             |

