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

## X-XSRF-TOKEN
---
라라벨은 현재의 CSRF 토큰을 프레임워크가 생성하는 모든 요청에 포함되어 있는 XSRF-TOKEN 쿠키에 저장합니다. X-XSRF-TOKEN request-요청 헤더를 세팅하기 위해 쿠키 값을 사용할 수 있습니다.

이 쿠키는 주로 편의를 위해 보내집니다. 왜냐하면 Angular 와 Axios 같은 자바스크립트 프레임워크나 라이브러리는 그 값을 X-XSRF-TOKEN 헤더에 자동으로 설정하기 때문입니다.