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

컨트롤러는 입력은 URL, HTTP요청 그룹에 대한 처리 로직입니다.

컨트롤러는 기본적으로 라라벨에서 제공하는 기본 컨트롤러 클래스를 상속받습니다. 상속을 통하여 긴본적인 동작과 매소드를 사용할 수 있습니다.

기본적으로 제공되는 동작으로는 미들웨어를 컨트롤러 액션에 연결하는 것입니다. 'middleware'메소드와 같은 몇 가지 편리한 메소드를 제공합니다.

다음은 라라벨의 기본적인 컨트롤러의 양식입니다.
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

위와 같이 작성한 컨트롤러 클래스는 라우트 파일에서 호출을 지정할 수 있습니다.
다음은 라우터 파일의 일부분 입니다.

```php
Route::get('user/{id}', 'UserController@show');
```

사용자가 HTTP GET요청으로 `user/{id}`같이 입력을 할경우 라우터는 `UserController`컨트롤러의 `show`매소드를 실행하게 됩니다.
이때, 라우트의 파라미터들 또한 메소드에 전달될 것입니다.

모든 URL요청처리에 대해서 컨트롤러 클래스를 반드시 사용해야 되는 것은 아닙니다. 별도의 컨트롤러를 사용하게 되면 middleware, validate, dispatch 함수와 같은 편리한 기능을 사용할 수 있습니다.

<br>

## 컨트롤러 & 네임스페이스
---

라우터에서 컨트롤러 호출을 정의할때, 컨트롤러의 네임스페이스를 같이 지정할 필요가 없습니다.

라라벨의 RouteServiceProvider는 네임스페이스를 포함하는 라우트 그룹 내에서 라우트 파일을 로드합니다.
때문에 네임 스페이스의 App\Http\Controllers 부분의 뒤에 오는 클래스 이름부분만 지정했습니다.

예를 들어, 컨트롤러를 `App \ Http \ Controllers` 디렉토리내에 위치시키려면 `App \ Http \ Controllers` 루트 네임 스페이스와 관련된 특정 클래스 이름을 사용하기 만하면 됩니다. 
따라서 만약 컨트롤러가 App\Http\Controllers\Photos\AdminController 처럼 구성되어 있다면 다음처럼 라우트를 구성하면 됩니다. :

```php
Route::get('foo', 'Photos\AdminController@method');
```
<br>

## 단일 동작 컨트롤러
---

컨트롤러가 여러가지의 동작을 처리하지 않고 하나의 처리 로직 메서드만 존재하는 경우도 있습니다. 
이런경우, 매소드를 생성하는 것 대신에 `__invoke` 매직 메서드를 이용하여 생성할 수도 있습니다.
invoke는 클래스를 함수처럼 로드할 수 있는 특수한 클래스 매소드 입니다.

다음은 `__invoke` 를 적용한 컨트롤러 예제 입니다.

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

invoke를 활용한 단일 동작처리 컨트롤러를 생성한 경우에는 별도의 메소드를 지정할 필요가 없습니다.
다음과 같이 클래스명만 전달을 해주면 됩니다.

```php
Route::get('user/{id}', 'ShowProfile');
```

<br>