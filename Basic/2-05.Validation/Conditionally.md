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

## 조건부로 규칙 추가하기
현재 값이 존재할때 유효성 검사하기
어떤 상황에서는 필드가 입력 배열에 존재할 때에만 그 필드의 유효성 검사를 실행하고 싶을수도 있습니다. 이를 위해서는 룰의 목록에 sometimes를 추가하기만 하면 됩니다:

```php
$v = Validator::make($data, [
    'email' => 'sometimes|required|email',
]);
```

이 예제에서는 $data 배열에 email 필드가 존재할 경우에만 그 필드의 유효성 검사가 실행됩니다.

{tip} 필드가 항상 존재하고 비어있지 않은지 확인하고자 한다면, 옵션 필드에 대한 주의사항부분을 참고하십시오.

## 복잡한 조건부 유효성 검사
때때로 여러분은 한가지 이상의 복잡한 로직에 대해서 유효성 규칙을 추가하고자 할 수도 있습니다. 예를 들어, 어떤 다른 필드가 100 이상의 값을 가질때에만 주어진 필드가 반드시 존재하길 바랄 수도 있습니다. 또는 다른 필드가 존재할 때에만 두개의 필드가 주어진 값을 가질 필요가 있을수도 있습니다. 이러한 유효성 검사 룰을 추가하는 것이 어렵지 않을 수 있습니다. 우선 고정 룰 을 변경할 필요 없이 그대로 사용하여 Validator 인스턴스를 생성합니다:

```php
$v = Validator::make($data, [
    'email' => 'required|email',
    'games' => 'required|numeric',
]);
```

여러분의 웹 애플리케이션이 게임 수집가들을 위한 사이트라고 가정해보겠습니다. 만약 100개 이상의 게임을 소유하고 있는 게임 수집가가 우리 사이트에 가입을 한다면, 우리는 그들이 왜 그렇게 많은 게임을 소유하고 있는지 설명을 듣고 싶을수 있습니다. 예를 들어, 아마 그들이 중고게임 판매점을 운영하거나, 단순히 수집을 취미로 할 수도 있습니다. 이런 요구사항을 조건부로 추가하기 위하여 Validator 인스턴스의 sometimes 메소드를 사용할 수 있습니다.

```php
$v->sometimes('reason', 'required|max:500', function ($input) {
    return $input->games >= 100;
});
```

sometimes 메소드에 전달되는 첫번째 인자는 필드의 이름입니다. 두번째 인자는 추가하려는 룰입니다. 만약 세번째 파라메터로 전달된 Closure가 true를 리턴한다면 그 룰은 유효성 검사에 추가될 것입니다. 이 메소드는 복잡한 조건부 유효성 검사의 구성을 손쉽게 만들어 주며, 한번에 여러개의 필드에 대한 조건부 유효성 검사를 추가할 수도 있습니다.

```php
$v->sometimes(['reason', 'cost'], 'required', function ($input) {
    return $input->games >= 100;
});
```

{tip} Closure로 전달된 $input 파라메터는 Illuminate\Support\Fluent의 인스턴스입니다. 그리고 입력된 데이터와 파일에 접근하기 위해 이 오브젝트가 사용할 수 있습니다.


