---
layout: laravel
title: Laravel
subtitle: Concepts
cate:
    name: "컨셉"
    link: "/Laravel/Concepts"

submenus:
    -
        name: 설계컨셉
        link: /Laravel/Concepts
    -
        name: Request 라이프사이클
        link: /Laravel/Concepts/Request
    -
        name: 라이프사이클 개요
        link: /Laravel/Concepts/Request/Lifecycle
    -
        name: 서비스 프로바이더
        link: /Laravel/Concepts/Request/Providers
---

## 서비스 프로바이더
---

서비스 프로바이더는 라라벨 애플리케이션들을 초기화 하는 중요 동작입니다. 서비스 프로바이터는 목록화된 각각의 애플리케이션 인스턴스를 생성합니다. 
서비스 프로바이더가 목록으로 등록되면 부트스트래핑 과정 통하여 프로그램으로 요청이 전달됩니다. 라라벨 애플리케이션이 어떻게 구성되어 있는지, 서비스 프로바이더를 통해 부트스트랩되는 과정을 구체적으로 이해하는 것은 매우 중요합니다. 

물론 사용자 애플리케이션을 위한 서비스 프로바이더는 `app/Providers` 디렉토리에 있습니다. 기본적으로 `AppServiceProvider` 는 거의 비어 있습니다. 

이 프로바이더는 사용자 여러분의 고유한 부트스트래핑과 서비스 컨테이너 바인딩 코드를 추가하기 위해 제공되는 곳입니다. 

물론 보다 큰 애플리케이션의 경우, 보다 세부적인 유형으로 구분된 종류별로 서비스 프로바이더를 만들 수도 있습니다.
