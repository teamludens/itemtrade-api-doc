# Policy

> https Request Example

```http
GET https://baseUrl/pages/{약관 및 정책}/
```

```http
GET https://baseUrl/pages/privacy/
GET https://baseUrl/pages/terms/
GET https://baseUrl/pages/verification/
```

> Response: 200 OK

```reStructuredText
해당되는 약관(정책)이 포함된 HTML 파일이 렌더링됩니다.
```

앱에 사용되는 각종 약관(정책)들이 포함되어있는 notion HTML 창이 나타납니다.<br>

### flatpages 목록

| URL Address         | Description                           |
| ------------------- | ------------------------------------- |
| pages/privacy/      | `개인정보 처리방침` 주소입니다.       |
| pages/terms/        | `이용 약관` 주소입니다.               |
| pages/verification/ | `회원가입 인증 서류 지침` 주소입니다. |