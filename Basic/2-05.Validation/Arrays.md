---
layout: laravel
title: Laravel
subtitle: Basic
cate:
    name: "기본사항"
    link: "/Laravel/Basic"

submenus:
    -
        name: 기본사항
        link: /Laravel/Basic
    -
        name: 라우팅
        link: /Laravel/Basic/Routing/Basic
    -
        name: 미들웨어
        link: /Laravel/Basic/Middleware
    -
        name: CSRF 보호
        link: /Laravel/Basic/CSRF
    -
        name: 컨트롤러
        link: /Laravel/Basic/Controllers
    -
        name: Requests-요청
        link: /Laravel/Basic/Requests
    -
        name: Responses-응답
        link: /Laravel/Basic/Responses
    -
        name: Views-뷰
        link: /Laravel/Basic/Views
    -
        name: URL 생성
        link: /Laravel/Basic/url
    -
        name: 세션
        link: /Laravel/Basic/session
    -
        name: Validation-유효성검사
        link: /Laravel/Basic/Validation
    -
        name: 에러처리
        link: /Laravel/Basic/Error
    -
        name: 로깅
        link: /Laravel/Basic/Logging
---

## 배열값 유효성 검사
form 입력필드의 배열을 유효성 검사하는 것을 어렵게 할 필요 없습니다. "점 표기법"을 사용하여 배열안에 있는 유효성 속성을 지정할 수 있습니다. 예를 들어 HTTP request가 가지고 있는 photos[profile] 필드에 대해서 다음과 같이 검사할 수 있습니다:

```php
$validator = Validator::make($request->all(), [
    'photos.profile' => 'required|image',
]);
```

또한 배열의 각각의 요소들을 검사할 수 있습니다. 예들 들어 주어진 배열 입력값이 각각의 유니크한 이메일인지 확인하려면 다음과 같이 하면 됩니다:

```php
$validator = Validator::make($request->all(), [
    'person.*.email' => 'email|unique:users',
    'person.*.first_name' => 'required_with:person.*.last_name',
]);
```

마찬가지로, 여러분은 배열을 기반으로한 단일 유효성 검사 메세지 를 사용하는데 * 문자를 여러분의 언어파일에 들어 있는 유효성 검사 메세지를 지정할 때 사용할 수 있습니다:

```php
'custom' => [
    'person.*.email' => [
        'unique' => 'Each person must have a unique e-mail address',
    ]
],
```
