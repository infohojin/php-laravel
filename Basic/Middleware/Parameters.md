---
layout: laravel
title: Laravel
subtitle: 미들웨어
cate:
    name: "미들웨어"
    link: "/Laravel/Basic"
submenus:
    -
        name: "미들웨어"
        link: "/Laravel/Basic/Middleware"
    -
        name: 정의
        link: "/Laravel/Basic/Middleware/Defining"
    -
        name: 등록하기
        link: "/Laravel/Basic/Middleware/Registering"
    -
        name: 파라미터
        link: "/Laravel/Basic/Middleware/Parameters"
    -
        name: 종료동작
        link: "/Laravel/Basic/Middleware/Terminable"
---

## 미들웨어 파라미터 (Middleware Parameters)
---
미들웨어에서는 추가적으로 파라미터를 전달 받을 수 있습니다. 예를 들어 애플리케이션이 인증된 사용자인지 확인하기 위해서 사용자의 주어진 "역할(role)"을 가진 인증 된 사용자인지 응용 프로그램에서 확인해야하는 경우 역할의 이름을 추가 인자로 전달 받는 CheckRole 을 만들 수 있습니다.

추가적인 미들웨어 파라미터는 미들웨어에 $next 인자 뒤에 전달 될것입니다:

```php
<?php

namespace App\Http\Middleware;

use Closure;

class CheckRole
{
    /**
     * Handle the incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @param  string  $role
     * @return mixed
     */
    public function handle($request, Closure $next, $role)
    {
        if (! $request->user()->hasRole($role)) {
            // Redirect...
        }

        return $next($request);
    }

}
```

미들웨어 파라미터는 라우트를 정의 할 때 지정된 미들웨어 이름과 파라미터를 : 로 구분하여 지정합니다. 여러개의 파라미터는 쉼표로 구분합니다:

```php
Route::put('post/{id}', function ($id) {
    //
})->middleware('role:editor');
```