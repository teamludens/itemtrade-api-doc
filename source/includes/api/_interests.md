# Interests

## [GET] List Interests

> https Request Example

```http
GET https://baseUrl/api/interests/
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
			"uuid":"fQW6UJWc5eNzvTNxSHK004",
			"name":"부동산",
      "emoji":"🏠",
			"image":"http://localhost:8000/media/%F0%9F%8F%A0"
		},
    ...
	]
}
```

Pagination 페이지당 200개의 Interest가 들어올 수 있습니다.

