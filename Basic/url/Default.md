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

## 기본값
---
몇몇 애플리케이션에서는 특정 URL 파라미터에 대해 요청-reqeust 전의 기본값을 지정할 수 있습니다. 예를 들어, 다수의 라우트에서 {locale} 파라미터를 정의한다고 가정해보겠습니다:

```php
Route::get('/{locale}/posts', function () {
    //
})->name('post.index');
```

route 헬퍼를 호출할 때마다 locale 을 전달해야 하는 것은 번거로운 일일 수 있습니다. 따라서 URL::defaults 메소드를 사용하여 현재 요청-request 중에서 항상 적용될 파라미터의 기본값을 정의 할 수 있습니다. 라우트 미들웨어에서 이 메소드를 호출하여 현재의 요청-request에 엑세스 할 수 있습니다:

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Support\Facades\URL;

class SetDefaultLocaleForUrls
{
    public function handle($request, Closure $next)
    {
        URL::defaults(['locale' => $request->user()->locale]);

        return $next($request);
    }
}
```

locale 파라미터의 기본값이 설정되면 rotue 헬퍼를 통해서 URL을 생성 할 때 더 이상 매번 값을 전달하지 않아도 됩니다.