---
layout: laravel
title: Laravel
subtitle: Routing
cate:
    name: "라우팅"
    link: "/Laravel/Basic/Routing"

submenus:
    -
        name: 라우팅
        link: /Laravel/Basic/Routing
    -
        name: 기본적인 라우팅
        link: /Laravel/Basic/Routing/Basic
    -
        name: 라우트 파라미터
        link: /Laravel/Basic/Routing/Parameters
    -
        name: 이름이 지정된 라우트
        link: /Laravel/Basic/Routing/Named
    -
        name: 라우트 그룹
        link: /Laravel/Basic/Routing/Groups
    -
        name: 라우트 모델 바인딩
        link: /Laravel/Basic/Routing/ModelBinding
    -
        name: Rate 제한
        link: /Laravel/Basic/Routing/RateLimiting
    -
        name: Form 메소드 Spoofing-속이기
        link: /Laravel/Basic/Routing/FormMethod
    -
        name: 현재 라우트에 엑세스하기
        link: /Laravel/Basic/Routing/Accessing
---

## Form 메소드 Spoofing-속이기
---

HTML form은 PUT, PATCH 와 DELETE 액션을 지원하지 않습니다. 따라서 PUT, PATCH 이나 DELETE 로 지정된 라우트를 호출하는 HTML form을 정의한다면 _method 의 숨겨진 필드를 지정해야합니다. _method 필드로 보내진 값은 HTTP request 메소드를 판별하는데 사용됩니다:

```php
<form action="/foo/bar" method="POST">
    <input type="hidden" name="_method" value="PUT">
    <input type="hidden" name="_token" value="{{ csrf_token() }}">
</form>
```
_method 입력을 생성하기 위해서 @method 블레이드 지시어를 사용할 수 있습니다:

```php
<form action="/foo/bar" method="POST">
    @method('PUT')
    @csrf
</form>
```