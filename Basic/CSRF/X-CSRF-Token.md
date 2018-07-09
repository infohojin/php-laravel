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

## X-CSRF-TOKEN
---
POST 파라메터으로 넘어오는 CSRF 토큰을 체크하는 것에 더하여, VerifyCsrfToken 미들웨어는 X-CSRF-TOKEN request-요청 헤더 또한 확인합니다. 예를들자면, HTML meta 태그에 토큰을 저장할 수 있습니다:

```
<meta name="csrf-token" content="{{ csrf_token() }}">
```

그리고, meta 태그를 만들고나서, jQeury와 같은 라이브러리를 사용하여 자동적으로 모든 헤더에 토큰을 추가할 수 있습니다. 이러한 방식은 AJAX 기반 애플리케이션을 위한 간단하고 편리한 CSRF 보호 방법을 제공해줍니다.

```
$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
    }
});
```

{tip} 기본적으로 resources/assets/js/bootstrap.js 파일은 csrf-token 메타 태그 값을 Axios HTTP 라이브러리에 등록합니다. Axios 라이브러리를 사용하지 않는 경우 애플리케이션에 이 작업을 직접 구성하도록 해야합니다.

