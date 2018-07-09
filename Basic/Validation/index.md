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
Validation-유효성 검사
소개하기
빠르게 유효성 검사 살펴보기
라우트 정의하기
컨트롤러 생성하기
유효성 검사 로직 작성하기
유효성 검사 에러 표시하기
옵션 필드에 대한 주의사항
Form Request 유효성 검사
Form Requests 생성하기
Form Requests 사용자 승인
에러 메세지 사용자 정의하기
Validators 수동으로 생성하기
자동으로 리다이렉트하기
이름이 지정된 Error Bags
유효성 검사 이후에 후킹하기
에러 메세지 작업하기
사용자 정의 에러 메세지
사용가능한 유효성 검사 규칙
조건부로 규칙 추가하기
배열값 유효성 검사
사용자 정의 유효성 검사 규칙
Rule 객체 사용하기
클로저 사용하기
확장기능 사용하기

## 소개하기
---
라라벨은 애플리케이션에 유입되는 데이터의 유효성을 검사하기 위한 다양한 방법을 제공합니다. 기본적으로, 라라벨의 베이스 콘트롤러 클래스는 다양하고 강력한 유효성 검사 규칙을 적용하여 유입되는 HTTP 요청의 유효성 검사를 위한 편리한 메소드를 제공하는 ValidatesRequests 트레이트-trait을 사용하고 있습니다.


