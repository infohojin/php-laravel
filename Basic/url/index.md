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
URL 생성
소개하기
기본적인 내용
기본 URL 생성하기
현재 URL 엑세스하기
이름이 지정된 라우트 URL
Signed URLs
컨트롤러 액션 URL
기본값

## 소개하기
---
라라벨은 애플리케이션에서 URL을 생성하는 것을 도와주는 몇가지 헬퍼 함수를 제공합니다. 당연하게도, 이는 템플릿 안에서 API 응답을 위한 링크를 생성하거나, 애플리케이션의 다른 곳으로 이동하는 리다이렉트 응답-response를 생성할 때 유용하게 사용됩니다.


