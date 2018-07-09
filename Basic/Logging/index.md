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
로깅

소개하기
설정하기
    로그 스택 구성하기
로그 메세지 작성하기
    채널을 지정하여 로그 기록하기
확장된 Monolog 채널 커스터마이징하기
    채널에서 사용하는 Monolog 커스터마이징하기
    Monolog 핸들러 채널 생성하기
    팩토리를 사용하여 채널 생성하기

# 소개하기
---
애플리케이션에서 어떤일이 일어나고 있는지 알기 위해서, 라라벨은 편리한 로깅 서비스를 제공합니다. 이 로깅 서비스는 로그 메세지를 파일에 남기거나, 시스템 에러에 출력하거나 또는 팀 슬랙에 알림을 보낼 수 있도록 해줍니다.

라라벨은 내부적으로 다양하고 강력한 로그 핸들러를 제공하는 Monolog 라이브러리를 사용합니다. 라라벨은 이러한 핸들러 설정을 간편하게 해주고 이러한 로그 핸들러를 조합하고 커스터마이징하기 쉽도록 해줍니다.


