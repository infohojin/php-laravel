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

## 미들웨어 등록하기 (Registering Middleware)
---
### 글로벌-전역 미들웨어
만약 애플리케이션의 모든 HTTP 요청에 대하여 미들웨어가 작동되기를 원한다면, app/Http/Kernel.php 클래스의 $middleware 프로퍼티에 미들웨어를 등록하시면 됩니다.


라우트에 미들웨어 지정하기
미들웨어를 특정 라우트에만 할당하고 싶을 때에는 우선 app/Http/Kernel.php 파일에 그 미들웨어의 키를 지정해야 합니다. 기본적으로, 이 Kernel 클래스의 $routeMiddleware 속성은 라라벨에 포함된 미들웨어들의 목록을 가지고 있습니다. 추가하려는 미들웨어에 원하는 키를 지정하고 이 목록에 붙여넣으십시오. 예를 들면:

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

미들웨어를 HTTP 커널에 등록했다면, 라우트에 middleware 메소드를 사용하여 미들웨어를 지정할 수 있습니다:

```php
Route::get('admin/profile', function () {
    //
})->middleware('auth');
```

라우트에 여러개의 미들웨어를 지정할 수도 있습니다: Route::get('/', function () { // })->middleware('first', 'second');

미들웨어를 지정할 때, 전체 클래스 이름을 전달할 수도 있습니다:

```php
use App\Http\Middleware\CheckAge;

Route::get('admin/profile', function () {
    //
})->middleware(CheckAge::class);
```

## 미들웨어 그룹
---
때로는 몇몇 미들웨어를 하나의 키로 묶어서 손쉽게 라우트에 할당하도록 하고자 할 수도 있습니다. HTTP 커널의 $middlewareGroups 속성을 사용해서 이를 지정할 수 있습니다.

별다른 설정 없이도, 라라벨은 웹 UI 와 API 라우트에 적용할 수 있는 일반적인 미들웨어를 포함하는 web 그리고 api 의 미들웨어 그룹을 제공합니다:

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

미들웨어 그룹은 개별 미들웨어와 동일한 문법으로 라우트들과 컨트롤러 액션들에 할당될 수 있습니다. 다시 한번, 미들웨어 그룹은 보다 편리하게 많은 미들웨어를 한번에 라우트에 할당할 수 있게 해줍니다:

```php
Route::get('/', function () {
    //
})->middleware('web');

Route::group(['middleware' => ['web']], function () {
    //
});
```

{tip} 별다른 설정없이도, web 미들웨어 그룹이 자동으로 RouteServiceProvider의 routes/web.php 파일에 지정되어 있습니다.



