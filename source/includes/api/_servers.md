# Servers

## [GET] Server List

> https Request Example

```http
GET https://baseUrl/servers/
GET https://baseUrl/servers/?depth=2
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
      "uuid":"UH7HRZwucrHKsLVbbwojso",
      "name":"스카니아",
      "coin":"100000000",
      "currency":"억 메소"
    },
    ...
  ]
}
```

```text
+-- 메이플 (depth=1)
|  +-- 스카니아 (depth=2)
|  +-- 루나 (depth=2)
|  +-- 크로아 (depth=2)
|  +-- ...
+-- 리니지 (depth=1)
|  +-- 바츠 (depth=2)
|  +-- 지그하르트 (depth=2)
|  +-- ...

```

### Query Parameters

| Parameters | Type | Required | 선택가능한 상태 값과 설명                                    |
| ---------- | ---- | -------- | ------------------------------------------------------------ |
| depth      | int  | false    | 현재 server db는 트리구조로 구성되어 있습니다.<br/>depth=1은 게임 이름을, depth=2는 특정 게임 하위 서버 이름을 뜻합니다.<br/>현재는 메이플 서버만 있기 때문에 일단 depth=2로 필터링을 해도 메이플 서버 리스트만을 뽑을 수 있습니다. 향후 게임이 다양해지면 변경이 필요합니다. |

### Response Parameters

| Parameters | Type | description                                                  |
| ---------- | ---- | ------------------------------------------------------------ |
| coin       | int  | 해당 게임의 가상 화폐가 실제 거래될 때, 일반적으로 거래 기준이 되는 단위. 예) 메이플 - 1억 메소 |
| currenty   | str  | coin의 거래 단위를 한글로 표현                               |

### Server DB 구조

* depth=1은 게임 이름 (ex. 메이플, 리지니)
* depth=2는 특정 게임(=해당 node의 parent)의 서버 이름 (ex. 스카니아, 바츠)