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

## 라우트 그룹
---

라우트 그룹을 사용하면 미들웨어나, 네임스페이스와 같은 라우트 속성을 공유할 수 있습니다.
그룹은 많은 수의 라우트를 등록할 때 라우터 각각의 개별 속성들을 정의하지 않아도 되게 해줍니다. 
공유하려는 속성은 배열 형식으로 지정되어 Route::group 메소드의 첫번째 인자로 전달됩니다.


## 미들웨어
---
그룹 안의 모든 라우트에 미들웨어를 할당하기 위해서는, 그룹을 정의하기 전에 middleware 메소드를 사용하면 됩니다. 미들웨어는 배열에 나열된 순서대로 실행될 것입니다:

```php
Route::middleware(['first', 'second'])->group(function () {
    Route::get('/', function () {
        // Uses first & second Middleware
    });

    Route::get('user/profile', function () {
        // Uses first & second Middleware
    });
});
```

## 네임스페이스
---
라우트 그룹을 사용하는 또 다른 사용 예로는 namespace 메소드를 사용하는 컨트롤러들에 동일한 PHP 네임스페이스를 할당하는 경우 입니다:

```php
Route::namespace('Admin')->group(function () {
    // Controllers Within The "App\Http\Controllers\Admin" Namespace
});
```

주의할점은, 기본적으로 RouteServiceProvider 는 App\Http\Controllers 네임스페이스를 접두사로 굳지 지정하지 않아도 컨트롤러가 등록되도록, 네임스페이스 그룹 안에서 라우트 파일을 로드한다는 것입니다. 따라서 여러분들이 네임스페이스에서 필요한 부분은 App\Http\Controllers 네임스페이스 뒷부분만 지정하면 됩니다.


## 서브 도메인 라우팅
---
라우트 그룹은 또한 카드형태의 서브 도메인을 처리하는데 사용할 수도 있습니다. 서브 도메인은 라우트 URI와 같이 서브 도메인의 일부를 추출하여, 라우트 파라미터로 할당할 수 있습니다. 서브 도메인은 그룹을 정의하기 전에 domain 메소드를 호출하여 지정할 수 있습니다:

```php
Route::domain('{account}.myapp.com')->group(function () {
    Route::get('user/{id}', function ($account, $id) {
        //
    });
});
```

## 라우트 Prefix
---
prefix 메소드는 그룹안의 라우트에 특정 URI을 접두어로 지정할 때 사용합니다. 그룹의 모든 라우트 URI 앞에 admin 을 붙이고 싶다면 다음과 같이 지정하면 됩니다:

```php
Route::prefix('admin')->group(function () {
    Route::get('users', function () {
        // Matches The "/admin/users" URL
    });
});
```

## 라우트 이름 접두사
---
name 메소드는 주어진 문자열을 가진 그룹 내의 각각의 라우트 이름에 접두사를 붙이는데 사용됩니다. 예를 들어, 그룹화 된 라우트들 전부에 admin 접두사를 붙일 수 있습니다. 전달된 문자열 그대로 라우트 이름 앞에 붙기 때문에, 접두사는 뒤에 . 문자를 포함하여 전달해야하는 것을 잊지말아야 합니다.

```php
Route::name('admin.')->group(function () {
    Route::get('users', function () {
        // Route assigned name "admin.users"...
    })->name('users');
});
```
