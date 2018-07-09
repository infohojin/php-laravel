---
layout: laravel
title: Laravel
subtitle: Concepts
cate:
    name: "서비스 컨테이너"
    link: "/Laravel/Concepts"

submenus:
    -
        name: 설계컨셉
        link: /Laravel/Concepts

    -
        name: 서비스 컨테이너
        link: /Laravel/Concepts/Container

    -
        name: 바인딩
        link: /Laravel/Concepts/Container/Binding
    -
        name: 의존성 해결
        link: /Laravel/Concepts/Container/Resolving

    -
        name: 이벤트
        link: /Laravel/Concepts/Container/Events
    -
        name: PSR-11
        link: /Laravel/Concepts/Container/PSR-11
---

## PSR-11
라라벨의 서비스 컨테이너는 PSR-11 인터페이스를 구현합니다. 따라서, PSR-11 인터페이스를 타입 힌트하여 라라벨의 컨테이너 인스턴스에 접근이 가능합니다.

```php
use Psr\Container\ContainerInterface;

Route::get('/', function (ContainerInterface $container) {
    $service = $container->get('Service');

    //
});
```

{note} 만약 컨테이너에 명시적으로 선언되지 않은 서비스를 get 메서드로 호출 시 예외가 발생됩니다.