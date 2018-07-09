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

에러 핸들링
소개하기
설정하기
Exception 핸들러
Report 메소드
Render 메소드
Reportable & Renderable Exceptions
HTTP Exceptions
커스텀-사용자 지정 HTTP 에러 페이지

# 소개하기
---

새로운 라라벨 프로젝트가 시작될 때, 기본적인 오류 및 예외 처리는 모두 설정되어 있습니다. 애플리케이션에서 발생하는 모든 예외-exceptions 들은 App\Exceptions\Handler 클래스에 의해서 로그를 남기고 사용자에게 응답을 보여주게 됩니다. 이 문서에서는 이 클래스에 대해서 좀 더 자세히 알아보겠습니다.

라라벨은 로그를 남기기 위해서 강력하고 다양한 로그 핸들러를 지원하는 Monolog 라이브러리를 사용합니다. 라라벨은 하나의 파일에 로그를 남기거나, 로그 파일을 일정 주기에 따라서 바꾸는, 또는 시스템 로그에 에러를 기록하도록 하는 여러 핸들러를 설정할 수 있습니다.


