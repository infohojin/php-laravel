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
## 미들웨어 정의하기 (Defining Middleware)
---
새로운 미들웨어를 생성하기 위하여 make:middleware 아티즌 명령을 사용할 수 있습니다:

```php
php artisan make:middleware CheckAge
```

이 명령은 CheckAge 클래스를 생성하여 app/Http/Middleware 디렉토리에 위치시킬 것입니다. 이 미들웨어 에서 우리는 입력받은 age가 200보다 클 때에만 요청된 주소에 접근할 수 있도록 허용하려고 합니다. 그렇지 않은경우 사용자를 home URI 으로 리다이렉트할 것입니다.

```php
<?php

namespace App\Http\Middleware;

use Closure;

class CheckAge
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if ($request->age <= 200) {
            return redirect('home');
        }

        return $next($request);
    }
}
```

위 코드에서 볼 수 있듯이, 주어진 age가 200보다 작거나 같으면 미들웨어는 HTTP 리다이렉트를 클라이언트에게 반환할 것입니다; 그렇지 않다면 HTTP 요청은 (미들웨어를 지나) 애플리케이션 안으로 계속 진행될 것입니다. (미들웨어가 "통과"를 허용하는) HTTP 요청을 애플리케이션 안으로 계속 전달하기 원한다면, $next 콜백함수에 $request인자를 넣어 호출하면 됩니다.

미들웨어를 HTTP request들이 애플리케이션에 도달하기 전에 반드시 통과해야 하는 일련의 "레이어"라고 생각하는 것이 가장 좋습니다. 각각의 레이어는 요청을 검사할 수 있고 완벽하게 요청을 거절할 수도 있습니다.

## Before & After 미들웨어
---
애플리케이션이 HTTP 요청을 미들웨어가 처리하기 전에 실행될지 처리한 후에 실행될지는 미들웨어 자신이 결정할 수 있습니다. 예를 들어 다음의 미들웨어는 애플리케이션에서 HTTP 요청이 처리되기 이전 에 실행됩니다.

```php
<?php

namespace App\Http\Middleware;

use Closure;

class BeforeMiddleware
{
    public function handle($request, Closure $next)
    {
        // Perform action

        return $next($request);
    }
}
```

반대로, 아래의 경우에서 미들웨어는 요청이 애플리케이션에 의해 처리된 이후에 실행될 것입니다:

```php
<?php

namespace App\Http\Middleware;

use Closure;

class AfterMiddleware
{
    public function handle($request, Closure $next)
    {
        $response = $next($request);

        // Perform action

        return $response;
    }
}
```


