---
layout: laravel
title: Laravel
subtitle: Contracts
cate:
    name: "컨셉"
    link: "/Laravel/Contracts"
---

Contracts
소개
Contracts Vs. Facades
Contracts 사용시기
느슨한 결합
단순함
Contracts 사용법
Contract 참고

## 소개
---

라라벨의 Contract는 프레임워크에서 제공하는 코어 서비스들을 정의한 인터페이스들의 모음입니다. 

예를 들어, Illuminate\Contracts\Queue\Queue Contract에는 어떤 작업들을 큐에서 다룰때 필요한 메소드들이 정의되어 있고, Illuminate\Contracts\Mail\Mailer Contract에는 이메일을 보내기 위해 필요한 메소드들을 정의되어 있습니다.

라라벨 프레임워크에는 각각의 Contract에 상응하는 구현체(구현 클래스)가 있습니다. 예를 들어, 라라벨은 다양한 드라이버로 구현된 queue의 구현체를 가지고 있고, SwiftMailer를 mailer의 구현체로 가지고 있습니다.

라라벨의 모든 Contract는 각각의 Github 저장소를 가지고 있습니다. 이것은 별도의 패키지에 의존하지 않는 각각의 단일 패키지로, 개발자들이 사용할 수 있도록 하는 contract를 위한 하나의 레퍼런스를 제공합니다.

<br>

### Contracts VS Facades
---

라라벨의 파사드는 서비스 컨테이너 외부에서 타입 힌트나, contracts 의 의존성 없이도 라라벨의 서비스를 시용할 수 있는 쉬운 방법을 제공합니다.

클래스 생성자에서 요구하지 않아도 되는 facade와 달리 contracts를 통해 클래스에 대한 명시적 종속성을 정의 할 수 있습니다. 일부 개발자는 이러한 방식으로 종속성을 명시적으로 정의하는 contsract를 선호하지만 대다수의 개발자는 facades의 편리함을 누리고 있습니다.

{tip} 대부분의 애플리케이션은 facades나 contract중 선호하는 어느것을 사용해도 무방합니다. 그러나 패키지를 빌드하는 경우 패키지 컨텍스트에서 테스트하기 쉽기 때문에 contract 사용을 강력하게 고려해야합니다.


