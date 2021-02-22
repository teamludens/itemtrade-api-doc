# Professions

## [GET] List Professions

> https Request Example

```http
GET https://staging.glamtutor.com/api/professions/
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
   "uuid": "6jPaGgDuJeeVffM7SXTBGx",
   "name": "의사"
  },
  ...
 ]
}
```

Pagination 페이지당 200개의 Profession이 들어올 수 있습니다.