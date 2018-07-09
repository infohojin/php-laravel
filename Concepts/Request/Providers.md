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

서비스 프로바이더는 라라벨 애플리케이션의 부팅(부트스트래핑) 단계의 주요한 핵심입니다. 애플리케이션의 인스턴스가 생성되고, 서비스 프로바이더가 등록된후 부트스트래핑 과정을 마친 프로그램이 요청을 처리합니다. 매우 간단합니다!

라라벨 애플리케이션이 어떻게 구성되어 있는지, 서비스 프로바이더를 통해 부트스트랩되는 과정을 구체적으로 이해하는 것은 매우 중요합니다. 물론 여러분의 애플리케이션을 위한 기본 서비스 프로바이더는 app/Providers 디렉토리에 있습니다.

기본적으로 AppServiceProvider 는 거의 비어 있습니다. 이 프로바이더는 여러분의 고유한 부트스트래핑과 서비스 컨테이너 바인딩 코드를 추가하기 위한 곳입니다. 물론 보다 큰 애플리케이션의 경우, 보다 세부적인 유형으로 구분된 종류별로 서비스 프로바이더를 만들 수도 있습니다.
