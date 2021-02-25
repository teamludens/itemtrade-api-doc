---

title: API Reference

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - api/accounts
  - api/sign_s3
  - api/users
  - api/errors

search: true

---

# General

## Introduction

GlamTutor API 문서입니다. [Slate](https://github.com/lord/slate)를 사용하여 만들었습니다.

아래의 링크를 클릭하면, Postman Collection을 다운받을 수 있습니다. 

## Authentication

GlamTutor는 JWT를 사용합니다.

`Authorization Bearer: [ACCESS_TOKEN]` 을 헤더에 담아 서버에 요청을 보냅니다.

토큰은 [Login API](#post-login)을 통해 발급받을 수 있고, expire되는 경우 refresh 토큰을 사용해 [Token Refresh API](#post-token-refresh)에서 Access Token을 재발급 받습니다.

모든 요청의 request header에 `Timezone`을 포함해서 보내야 합니다. (예. `Timezone`: `Asia/Seoul`)

## Release Notes

### 2021.2.13

* gathering detail에서 my_participant_request 값 object -> array로 변경
* create/update gatherings: request & response 모두 participants 필드 제거 -> participations API에서 처리

### 2021.2.11

- Iamport 인증 조회 API description 업데이트
- Policy Page URL 업데이트
- Comment Create API response 수정
  - humanized_created 추가

### 2021.2.10

* interests에 image 필드 추가(gathering 생성시 지정 안하는 경우 preset으로 사용)

### 2021.2.9

- Login Step 1 API response 수정
  - response body 안에 담겨 있던 error를 status code 여러개로 분리
- Gathering Detail description 수정
- Gathering Update description 수정
- Gathering Create descrioption 수정
  - 미정인 값들은 null로 요청
- Logout API 오타 수정
- 모임에 참여신청한 유저 리스트 API response 수정

### 2021.2.7

- Gathering 관련 API 전반적으로 수정된 곳이 많으니 한번 살펴봐 주세요.
  - Gathering Detail, List API에서 host 필드  내 interests값을 쿼리 최적화 위해 제거 (user detail에는 interests가 있음)
  - [Gathering 생성](#post-create-gathering)시 payload 관련
    - 날짜, 장소, 인원제한 미정시 null이 아니라 key-value값을 json payload에서 빼주세요.(그냥 안보내면 됨))
    - 장소 처리 방법 수정: is_online 필드와 place 필드의 조합으로 처리
  - [Gathering detail](#get-gathering-detail) 필드들 변화(필요한 것만 처리하도록) -> my_participation_request 필드 추가
- Register User API: gender의 값 male, female로 변경
- Participation List API 삭제 및 Participation Update 권한 제한 추가

### 2021.2.5

- Login Step 1 API 수정

  - user 상태에 따라 구분된 error code

- Accounts API 수정

  - token명 access, refresh로 통일

- User Me API description 추가

  - user 승인 상태 명시

- Login Step 1 API 수정

  승인되지 않은 회원 Error Code (withdrawn) 삭제 

### 2021.2.4

- Accounts API response 업데이트
  - Register User API 성공시 token 반환
  - multipart-form 가이드 제거
- Comment API response 업데이트
  - profession 출력 변경
- Gathering API response 업데이트
  - date, humanized_date, displayed_date, humanized_created,participation_status 추가
- token 명 통일
  - access -> access_token, refresh -> refresh_token

### 2021.2.2

- Participation List 추가
- Update Participation API 수정 - 방장이 아닌 유저도 요청 가능 
- User Me API 수정
  - 승인 상태(status), 나이(age) 추가

###  2021.2.1

- 방장이 아닌 유저가 모임에 승인 됐는지 알 수 있는 필드(i_accpeted) 추가(Gathering Detail, Gathering  List)
- user avatar 필드 제거, profession 필드 추가
- 모임 통계 정보 추가(max_age, min_age) - Gathering stat

### 2020.1.26

* 모임 신고 API 추가 (gathering flag)
* 약관 페이지 API 추가 (policy)

### 2020.1.21

* participation 관련 API url scheme 변경 및 의미 정리
  * api/gatherings/me/ -> api/gathering/i_participated/
  * api/gatherings/:uuid/participants -> api/gathering/:uuid/participation_requests/
* username 중복 체크 및 users/me/ API 추가

### 2020.1.20

* 모임 참여 신청 생성 및 상태 변경 API - participation create/update
* (방장이) 모임 승인용 리스트(gatherings/:gathering_uuid/participants/)
* 모임 통계 API: gatherings/:gathering_uuid/stats/
* 내가 참여한 모임 리스트 API - gatherings/me/
* 내가 만든 모임 리스트 API - gathering-list API에 `host` query string으로 필터링
* gathering 생성시 인원, 모임, 날짜 미정시 처리 방법 추가

### 2021.1.19

* Iamport API 및 Web Page 추가
* Comments API 추가

### 2021.1.18

* Login, Logout, Refresh API 추가

### 2021.1.17

* S3 signing API 추가
* Accounts(회원가입 및 로그인) 추가
* Interests List 추가
* Professions List 추가
* Gatherings 관련 CRUD 추가

### 2021.1.13

* Place List 추가