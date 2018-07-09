---
layout: laravel
title: Laravel
subtitle: 미들웨어
cate:
    name: "미들웨어"
    link: "/Laravel/Basic/Middleware"
submenus:
    -
        name: "미들웨어"
        link: "/Laravel/Basic/Middleware"
    -
        name: 정의
        link: "/Laravel/Basic/Middleware/Defining"
    -
        name: 등록하기
        link: "/Laravel/Basic/Middleware/Registering"
    -
        name: 파라미터
        link: "/Laravel/Basic/Middleware/Parameters"
    -
        name: 종료동작
        link: "/Laravel/Basic/Middleware/Terminable"
---

# 소개하기
---

미들웨어는 애플리케이션으로 들어온 HTTP 요청을 간편하게 필터링할 수 있는 방법을 제공합니다. 예를 들어, 라라벨은 애플리케이션의 사용자가 인증되었는지 검사하는 미들웨어를 내장하고 있습니다. 만약 인증되지 않은 사용자라면, 미들웨어는 그 사용자를 로그인 화면으로 리다이렉트 시킬 것입니다. 반대로, 인증된 사용자라면, 미들웨어는 애플리케이션에서 HTTP 요청이 계속해서 더 처리되도록 허용할 것입니다.

물론, 인증이외에도 다양한 작업을 수행하는 추가적인 미들웨어를 작성할 수도 있습니다. CORS 미들웨어는 애플리케이션에서 내보내는 모든 응답에 적절한 헤더들을 추가하는 역할을 담당할 수도 있습니다. 로깅 미들웨어는 애플리케이션으로 들어오는 모든 요청을 기록할 수도 있습니다.

라라벨 프레임워크에는 인증(authentication)과 CSRF 보안을 위한 미들웨어들이 포함되어 있습니다. 이러한 미들웨어들은 모두 app/Http/Middleware 디렉토리 안에 위치하고 있습니다.


