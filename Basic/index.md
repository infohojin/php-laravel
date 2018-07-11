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

# 라라벨 기본
---

라라벨 프레임워크를 시작하는데 필요한 기본 사항들에 대해서 설명을 합니다. 기본사항들은 라라벨의 각각의 동작과 처리방식에 대해서 이해할 수 있습니다.

프레임워크는 다양한 기술들이 종합적으로 적용된 결정체 입니다. 따라서, 여러분들이 학습하였던 PHP의 지식과 웹과 관련된 다양한 기술 테크닉으로 구현된 프로그램입니다.

기본사항들은 라라벨이 시작과 끝, 동작을 넘어가면서 수행되는 다양한 기본기술 들에 대해서 설명을 합니다.

* 기본적인 라우팅
    - 리다이렉트 라우트
    - 뷰-View 라우트
* 라우트 파라미터
    - 필수 파라미터
    - 선택적인 파라미터
    - 정규표현식 제약
* 이름이 지정된 라우트
* 라우트 그룹
    - 미들웨어
    - 네임스페이스
    - 서브 도메인 라우팅
    - 라우트 Prefixes
    - 라우트 이름 접두사
* 라우트 모델 바인딩
    - 명시적 바인딩
    - 묵시적 바인딩
* Rate 제한
* Form-폼 메소드 Spoofing-스푸핑
* 현재 라우트에 엑세스하기

<hr>
* 미들웨어
* 소개하기
* 미들웨어 정의하기
* 미들웨어 등록하기
    - 글로벌-전역 미들웨어
    - 라우트에 미들웨어 지정하기
    - 미들웨어 그룹
* 미들웨어 파라미터
* 종료시 동작하는 미들웨어

