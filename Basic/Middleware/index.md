---
layout: laravel
title: Laravel
subtitle: 미들웨어
cate:
    name: "미들웨어"
    link: "/Laravel/Basic/Middleware"
---


# 미들웨어
---

부트스트래핑을 통하여 실제 적인 동작을 처리하는 라우터, 컨트롤러로 전달되는 과정에서 중간에 처리되어야 하는 과정들을 정의한 동작들을 미들웨어라고 합니다.
미들웨어는 실제코드를 처리하기 전에 시작되기 전에 HTTP Request의 필터링이라고 할 수 있습니다.

라라벨 프레임워크에는 인증(authentication)과 CSRF 보안을 위한 다양한 미들웨어들이 포함되어 있습니다. 

이러한 미들웨어 필터링 동작들의 대표적으로는 회원 인증을 검사하는 것입니다. 만약 어떠한 동작들이 인증된 회원의 접속에만 처리되는 것이라면, 이 회원 인증과정을 미들웨어 부분에 등록하여 미리 처리할 수 있습니다.

그외 `CORS` 미들웨어는 애플리케이션에서 내보내는 모든 응답에 적절한 헤더들을 추가하는 역할을 담당하기도 합니다. 또한 `로깅` 미들웨어는 애플리케이션으로 들어오는 모든 요청을 기록하는 동작을 수행합니다.

라라벨은 미들웨어는 `app/Http/Middleware` 디렉토리 안에 위치하고 있습니다.


<br><br><br>

## 미들웨어 정의하기 (Defining Middleware)
---

라라벨은 새로운 미들웨어를 생성할때 쉽게 기본코드를 생성할 수 있도록 아티즌 명령을 제공합니다. 아티즌 명령어로 `make:middleware`를 사용합니다.

```php
php artisan make:middleware CheckAge
```

위와 같이 아티즌 명령을 수행하면 `app/Http/Middleware` 디렉토리에 `CheckAge` 클래스 파일을 생성합니다.

다음은 생성된 `CheckAge` 클래스 파일 입니다.

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

위 코드에서 볼 수 있듯이, 
미들웨어인 `CheckAge` 클래스는 주어진 age가 200보다 작거나 같으면 HTTP 리다이렉트를 클라이언트로 반환합니다. 
그렇지 않다면 HTTP 요청은 (미들웨어를 지나) 애플리케이션 안으로 계속 진행을 하게 됩니다.

이렇게 미들웨어는 HTTP 요청을 애플리케이션 안으로 계속 전달 하기 위한 통과 처릴 필터와 같습니다. `$next` 콜백함수에 `$request` 인자를 넣어 호출로 진행합니다.

미들웨어를 HTTP request들이 애플리케이션에 도달하기 전에 반드시 통과해야 하는 일련의 관문(레이어)이라고 생각하는 것이 가장 좋습니다. 

각각의 레이어는 요청을 검사할 수 있고 요청을 거절할 수도 있습니다.

<br>

### Before & After 미들웨어
---

미들웨어는 HTTP 요청 전 또는 후에 처리 될 수도 있습니다.이는 미들웨어 자신이 결정하여 동작을 할 수 있습니다.

예를 들어 다음 `BeforeMiddleware` 미들웨어는 HTTP 요청이 처리되기 이전 에 실행됩니다.

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

반대로, 아래의 경우에서 `AfterMiddleware` 미들웨어는 요청이 처리된 이후에 실행될 것입니다:

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

<br><br><br>





## 미들웨어 등록하기 (Registering Middleware)
---

다음은 생성한 미들웨어를 등록하는 방법에 대한 설명입니다.

### 글로벌-전역 미들웨어
---

생성한 미들웨어를 모든 HTTP요청에 대해서 적용하기를 원하는 경우에는 클로벌로 등록을 하면 됩니다. 
클로벌 등록은 `app/Http/Kernel.php` 클래스의 `$middleware` 프로퍼티 배열에 추가하시면 됩니다.

<br>

### 라우트에 미들웨어 지정하기
---

미들웨어를 라우트에서도 등록을 할 수 있습니다. 라우트에서 미들웨어를 등록 사용하기 위해서는 미리 `app/Http/Kernel.php` 파일에 미들웨어의 키를 만들어야 합니다.

그리고 특정 라우트에만 미들웨어를 할당할 수 있습니다.


`Kernel` 클래스의 `$routeMiddleware` 는 라라벨에서 지원하는 모든 미들웨어 목록을 가지고 있습니다. 

추가하는 미들웨어에 키를 지정합니다.

```php
// Within App\Http\Kernel Class...

protected $routeMiddleware = [
    'auth' => \Illuminate\Auth\Middleware\Authenticate::class,
    'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
    'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
    'can' => \Illuminate\Auth\Middleware\Authorize::class,
    'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
    'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
];
```

위와 같이 생성한 미들웨어를 HTTP 커널에 등록했다면, 라우트에서 `middleware` 메소드를 사용하여 지정할 수 있습니다:
다음은 라우터에서 미들웨어를 지정하는 방법입니다.

```php
Route::get('admin/profile', function () {
    //
})->middleware('auth');
```

한개의 라우트 설정에 여러개의 미들웨어를 지정할 수도 있습니다: 

```php
Route::get('/', function () { 
    // 
})->middleware('first', 'second');
```

여러개를 지정할 경우에는 콤마로 구분하여 키를 나열합니다.

미들웨어를 지정할 때, 전체 클래스 이름을 전달할 수도 있습니다:

```php
use App\Http\Middleware\CheckAge;

Route::get('admin/profile', function () {
    //
})->middleware(CheckAge::class);
```

<br>


### 미들웨어 그룹
---

여러개의 미들웨어를 지정하여 통과 싶을 경우, 매번 콤마로 나열해서 지정해 주는 것은 불편합니다. 미들웨어를 하나의 배열로 만들어서 그룹 적용을 할 수 있습니다.

HTTP 커널의 `$middlewareGroups` 은 여러개의 미들웨어를 배열로 그룹화 지정을 할 수 있습니다.

```php
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],

    'api' => [
        'throttle:60,1',
        'auth:api',
    ],
];
```

위와 같이 별다른 추가 설정을 하지 않아도 라라벨은 웹 UI 와 API 라우트로 적용할 수 있습니다. 라라벨은 기본적으로 미들웨어를 포함하는 web 그리고 api 의 미들웨어 그룹을 제공합니다:

이렇게 등록한 미들웨어 그룹은 개별 미들웨어과 동일한 방식으로 라우트와 컨트롤러 액션 메소드에 할당을 할 수 있습니다.

```php
Route::get('/', function () {
    //
})->middleware('web');

Route::group(['middleware' => ['web']], function () {
    //
});
```

이처럼 미들웨어 그룹은 보다 편리하게 다수의 미들웨어를 한번에 라우트 할당을 할 수 있게 도와 줍니다:

web 미들웨어 그룹은 자동으로 `RouteServiceProvider`의 `routes/web.php` 파일에 지정되어 있습니다.



<br><br><br>


## 미들웨어 파라미터 (Middleware Parameters)
---

미들웨어에서도 추가 파라미터를 전달 할 수 있습니다. 파라미터는 매개변수인 클로저(`$next`)뒤에 추가하여 전달합니다.

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

위는 코드는 사용자 인증처리에 대한 예제 입니다.  파라미터로 `$role`을 전달하는데, 프로그램에서 사용자가 "역할(role)"을 가진 사용자인지 확인해야 하는 경우 입니다.
역할의 이름을 추가 인자로 전달 받는 CheckRole 을 만들 수 있습니다.


라우터에서 미들웨어의 파라미터를 전달할때는 이름과 콜론(`:`)으로 구분하여 지정합니다.

```php
Route::put('post/{id}', function ($id) {
    //
})->middleware('role:editor');
```
여러개의 파라미터는 쉼표로 구분합니다:

<br><br><br>


## 종료시 동작하는 미들웨어 (Terminable Middleware)
---

우리는 라라벨로 개발을 할때 대로는 HTTP 요청에 대한 모든 response 를 수행하고 나서 동작해야 되는 미들웨어도 필요할 수 있습니다.

예를 들어, 라라벨에 내장된 "session" 미들웨어는 response이 준비된 이후에 세션 데이터를 저장소에 저장을 하게 됩니다.

만약 미들웨어에 `terminate` 메소드를 추가로 정의했다면, response이 처리 준비된 상태에서 자동으로 이 미들웨어 매소드가 호출 됩니다.
`terminate` 메소드를 작성할때는 인자값으로 Http 요청과 응답의 두가지를 받습니다.

```php
<?php

namespace Illuminate\Session\Middleware;

use Closure;

class StartSession
{
    public function handle($request, Closure $next)
    {
        return $next($request);
    }

    public function terminate($request, $response)
    {
        // Store the session data...
    }
}
```

종료시 동작하는 미들웨어를 정의하면 작성한 미들웨어를 `app/Http/Kernel.php` 파일의 라우트 또는 글로벌 미들웨어 목록에 추가해 주어야 합니다.

작성한 미들웨어의 `terminate` 메소드가 호출될 때, 라라벨은 서비스 컨테이너에서 미들웨어에 대한  생성된 인스턴스의 의존성 해결합니다.

미들웨어의 `handle` 과 `terminate` 메소드를 호출할 때, 새로운 인스턴스를 생성하지 않고 기존의 생성된 미들웨어 인스턴스를 사용하고자 한다면 컨테이너의 `singleton` 메소드를 사용하여 미들웨어를 등록해야 합니다.

