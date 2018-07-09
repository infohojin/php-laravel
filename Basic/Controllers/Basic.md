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

## 기본적인 컨트롤러
---
컨트롤러 정의
아래는 기본 컨트롤러 클래스의 예입니다. 컨트롤러는 Laravel에 포함 된 기본 컨트롤러 클래스들을 확장합니다. 기본 클래스는 미들웨어를 컨트롤러 액션에 연결하는 데 사용할 수있는 'middleware'메소드와 같은 몇 가지 편리한 메소드를 제공합니다.

```php
<?php

namespace App\Http\Controllers;

use App\User;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return Response
     */
    public function show($id)
    {
        return view('user.profile', ['user' => User::findOrFail($id)]);
    }
}
```

여러분은 다음과 같이 컨트롤러의 액션에 라우트를 지정할 수 있습니다.

```php
Route::get('user/{id}', 'UserController@show');
```

이제 사용자의 요청이 지정된 라우트의 URI와 일치할 때 UserController 클래스의 showProfile 메소드가 실행될것입니다. 이때, 라우트의 파라미터들 또한 메소드에 전달될 것입니다.

{tip} 컨트롤러는 기본 클래스를 확장하기 위해 필수가 아닙니다. 그러나 middleware, validate, dispatch 함수와 같은 편리한 기능을 사용할 수는 없습니다.


## 컨트롤러 & 네임스페이스
---
컨트롤러에 대응하는 라우트를 정의 할 때 전체 컨트롤러의 전체 네임 스페이스를 지정할 필요가 없다는 점에 유의해야합니다. RouteServiceProvider는 네임 스페이스를 포함하는 라우트 그룹 내에서 라우트 파일을 로드하기 때문에 네임 스페이스의 App\Http\Controllers 부분의 뒤에 오는 클래스 이름부분만 지정했습니다.

컨트롤러를 App \ Http \ Controllers 디렉토리내에 위치시키려면 App \ Http \ Controllers 루트 네임 스페이스와 관련된 특정 클래스 이름을 사용하기 만하면됩니다. 따라서 만약 컨트롤러가 App\Http\Controllers\Photos\AdminController 처럼 구성되어 있다면 다음처럼 라우트를 구성하면 됩니다. :

```php
Route::get('foo', 'Photos\AdminController@method');
```

## 단일 동작 컨트롤러
---
단일 액션만을 처리하는 컨트롤러를 정의하고 싶다면 컨트롤러에 하나의__invoke 메소드를 넣을 수 있습니다 :

```php
<?php

namespace App\Http\Controllers;

use App\User;
use App\Http\Controllers\Controller;

class ShowProfile extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return Response
     */
    public function __invoke($id)
    {
        return view('user.profile', ['user' => User::findOrFail($id)]);
    }
}
```

단일 액션 컨트롤러에 대한 경로를 등록 할 때 함수를 지정할 필요가 없습니다:

```php
Route::get('user/{id}', 'ShowProfile');
```


