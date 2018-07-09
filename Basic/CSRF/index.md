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

CSRF 보호
소개하기
특정 URI 제외시키기
X-CSRF-Token
X-XSRF-Token

# 소개하기
---
라라벨은 크로스-사이트 요청 위조 공격 (CSRF)으로부터 애플리케이션을 손쉽게 보호할 수 있도록 해줍니다. 사이트 간 요청 위조는 인증된 사용자를 대신해서 승인되지 않은 커맨드를 악의적으로 활용하는 것입니다.

라라벨은 애플리케이션에 의해 관리되는 모든 활성화된 사용자 세션마다 CSRF "토큰"을 자동으로 만들어 줍니다. 이 토큰은 인증된 사용자가 애플리케이션에 request-요청을 할 수 있는 고유한 사용자라는 것을 확인하는데 사용됩니다.

HTML 폼을 정의할 때, CSRF 토큰을 hidden 필드로 포함시켜서, CSRF 보호 미들웨어가 request-요청을 검증(validate)할 수 있도록 해야합니다. 토큰 필드를 생성하기 위해 @csrf 블레이드 지시어를 사용할 수 있습니다:

```php
<form method="POST" action="/profile">
    @csrf
    ...
</form>
```

web 미들웨어 그룹에 속한 VerifyCsrfToken 미들웨어는 자동으로 request-요청에 포함된 토큰이 세션에 저장된 토큰과 일치하는지 확인할 것입니다.

## CSRF 토큰 & JavaScript
---
자바스크립트를 기반으로한 애플리케이션을 구성할 때는, 자바스크립트 HTTP 라이브러리에서 모든 서버 request-요청에 CSRF 토큰을 자동으로 추가해주도록 하면 편리합니다. 기본적으로 resources/assets/js/bootstrap.js 파일은 csrf-token 메타 태그 값을 Axios HTTP 라이브러리에 등록합니다. Axios 를 사용하지 않는 경우 애플리케이션에 이 작업을 직접 구성하도록 해야합니다.

