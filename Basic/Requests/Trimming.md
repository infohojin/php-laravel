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

## 입력값 Trim 처리 & 일반화처리
---
기본적으로 라라벨은 애플리케이션의 글로벌 미들웨어 스택에 TrimStrings 그리고 ConvertEmptyStringsToNull 미들웨어를 포함하고 있습니다. 이 미들웨어는 App\Http\Kernel 클래스의 미들웨어 스택에 나열되어 있습니다. 이 미들웨어는 request-요청에 유입되는 모든 문자 필드들을 자동으로 trim 처리하고, 빈 문자필드는 null로 변환합니다. 따라서 라우트와 컨트롤러에서 이러한 일반화 문제를 걱정할 필요가 없습니다.

이 동작을 비활성화 시키려면, App\Http\Kernel 클래스의 $middleware 속성에서 두 미들웨어를 제거하면 됩니다.