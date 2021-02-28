---

title: API Reference

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - api/accounts
  - api/posts
  - api/servers
  - api/sign_s3
  - api/users
  - api/errors

search: true

---

# General

## Introduction

아이템 트레이드 API 문서입니다. [Slate](https://github.com/lord/slate)를 사용하여 만들었습니다.

아래의 링크를 클릭하면, Postman Collection을 다운받을 수 있습니다. 

## Authentication

아이템 트레이드는 JWT를 사용합니다.

`Authorization Bearer: [ACCESS_TOKEN]` 을 헤더에 담아 서버에 요청을 보냅니다.

토큰은 [Login API](#post-login-step-2-use-token)을 통해 발급받을 수 있고, expire되는 경우 refresh 토큰을 사용해 [Token Refresh API](#post-refresh-token)에서 Access Token을 재발급 받습니다.

## Release Notes

### 2021.2.28

* Post update, bump, detail API 추가
* Server list API 추가

### 2021.2.26

* Post create, list API 추가

### 2021.2.25

- Init: accounts, sign s3, users 및 errors 관련 기본 API 문서 셋업