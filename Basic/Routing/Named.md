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

## 이름지정
---
라우터에 이름을 지정할 수 있습니다.

### 이름이 지정된 라우트
---
이름이 지정된 라우트는 URL 이나 지정된 라우트로의 리다이렉션을 손쉽게 생성하기 편리하게 해줍니다. 
라우트 정의에 name 메소드를 체이닝 하여 라우트에 이름을 지정할 수 있습니다:

```php
Route::get('user/profile', function () {
    //
})->name('profile');
```

`user/profile` 라우트에 대해서 `profile`이라는 이름을 지정합니다.

컨트롤러의 액션에도 라우트 이름을 지정할 수 있습니다:

```php
Route::get('user/profile', 'UserController@showProfile')->name('profile');
```
<br>

### 이름이 지정된 라우트들에 대한 URL 생성하기
---
라우트에 이름을 할당하게 되면 몇가지 편리함 점들이 있습니다.
전역 route 함수를 이용하여 URL 또는 리다이렉션을 이름을 통하여 생성하여 사용할 수 있습니다:

```php
// Generating URLs...
$url = route('profile');

// Generating Redirects...
return redirect()->route('profile');
```
앞에서 지정한 라우트 이름을 통하여 라우트를 지정합니다. 또는 리다이렉션도 할 수 있습니다.

이름이 지정된 라우트에 파라미터를 정의하였다면, route 메소드의 두번째 인자로 파라미터를 전달할 수 있습니다. 
주어진 파라미터는 자동으로 올바른 위치에 있는 URL 에 삽입될 것입니다:

```php
Route::get('user/{id}/profile', function ($id) {
    //
})->name('profile');

$url = route('profile', ['id' => 1]);
```
<br>

### 현재의 라우트 검사하기
---
현재의 request 가 주어진 이름의 라우트가 맞는지 확인하고자 한다면, Route 인스턴스의 named 메소드를 사용하면 됩니다. 
예를 들어 라우트 미들웨어에서 현재 라우트의 이름을 확인할 수 있습니다:

```php
/**
 * Handle an incoming request.
 *
 * @param  \Illuminate\Http\Request  $request
 * @param  \Closure  $next
 * @return mixed
 */
public function handle($request, Closure $next)
{
    if ($request->route()->named('profile')) {
        //
    }

    return $next($request);
}
```

<br>