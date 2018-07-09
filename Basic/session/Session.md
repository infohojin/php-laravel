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

## 세션 사용하기
---
### 데이터 조회하기
라라벨 안에서 세션 데이터를 조작하려면 두 가지 주요한 방법이 있습니다: 글로벌 session 헬퍼를 사용하는 것과 Request 인스턴스를 통한 방법입니다. 먼저 컨트롤러 메소드에서 타입-힌트 할 수 있는 Request 인스턴스를 통해서 세션에 엑세스 하는 것을 살펴보겠습니다. 유의할 것은, 컨트롤러 메소드는 라라벨의 서비스 컨테이너를 통해서 자동으로 의존성 주입된다는 것입니다:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  Request  $request
     * @param  int  $id
     * @return Response
     */
    public function show(Request $request, $id)
    {
        $value = $request->session()->get('key');

        //
    }
}
```

세션에서 특정 아이템을 찾을 때, get 메소드의 두번째 인자로 기본 값을 전달 할 수 있습니다. 이 기본값은 지정된 키가 세션 안에서 존재하지 않을 경우 반환될 것입니다. get 메소드의 기본값으로 클로저를 전달하는 경우 요청받은 키가 존재하지 않는다면, 클로저가 실행결과를 반환할 것입니다:

```php
$value = $request->session()->get('key', 'default');

$value = $request->session()->get('key', function () {
    return 'default';
});
```

### 글로벌 세션 헬퍼
---

또한 세션에서 데이터를 찾거나, 저장하기 위해서 글로벌 session PHP 함수를 사용할 수도 있습니다. session 헬퍼를 하나의 문자열을 인자로 호출하게 되면, 해당하는 세션 키에 대한 값을 반환합니다. 헬퍼가 키 / 값 쌍으로 구성된 배열과 함께 호출되면 해당 세션 값이 세션에 저장됩니다:

```php
Route::get('home', function () {
    // Retrieve a piece of data from the session...
    $value = session('key');

    // Specifying a default value...
    $value = session('key', 'default');

    // Store a piece of data in the session...
    session(['key' => 'value']);
});
```

{tip} 글로벌 session 헬퍼를 사용하는 것에 비해서 HTTP 요청-request 인스턴스에서 세션을 사용하는 것에는 약간의 실질적인 차이가 있습니다. 두가지 메소드는 테스트 케이스 안에서 사용가능한 assertSessionHas 메소드를 통해서 테스트가 가능합니다.

### 모든 세션 데이터 조회하기
---
세션에서 모든 데이터를 찾고자 한다면 all 메소드를 사용하면 됩니다:

```php
$data = $request->session()->all();
```

## 세션에 아이템이 존재하는지 확인하기
세션안에 아이템이 존재하는지 확인하려면 has 메소드를 사용하면 됩니다. has 메소드는 값이 현재 존재하고 null이 아니라면 true를 반환합니다:

```php
if ($request->session()->has('users')) {
    //
}
```

값이 null이더라도 세션안에 아이템이 들어 있는지 확인하려면 exists 메소드를 사용할 수 있습니다. exists 메소드는 값이 존재한다면 true를 반환합니다:

```php
if ($request->session()->exists('users')) {
    //
}
```

### 데이터 저장하기
---

세션에 데이터를 저장하기 위해서는, 일반적으로 put 메소드나 session 헬퍼를 사용합니다:

```php
// Via a request instance...
$request->session()->put('key', 'value');

// Via the global helper...
session(['key' => 'value']);
```

### 세션에 들어 있는 배열에 값 추가하기
---

push 메소드는 세션에 들어 있는 배열에 새로운 값을 추가할 때 사용할 수 있습니다. 예를 들어 어떤 팀의 이름들에 대한 배열을 나타내는 user.teams 키가 있을 때, 다음과 같이 값을 추가할 수 있습니다:

```php
$request->session()->push('user.teams', 'developers');
```

### 아이템을 조회 & 삭제하기
---
하나의 구문안에서 pull 메소드는 세션에서 아이템을 가져오면서 삭제합니다:

```php
$value = $request->session()->pull('key', 'default');
```

### 데이터 임시저장하기
---
때로는 바로 다음번의 요청에서만 사용하기 위해서 세션에 아이템을 저장할 수 있습니다. 바로 flash 메소드를 사용하는 것입니다. 이 메소드를 사용하여 세션에 저장된 데이터는 바로 이어지는 HTTP 요청에 대해서만 사용이 가능하고, 이후에는 삭제됩니다. 임시 데이터는 주로 상태 메세지등 잠깐동안 사용하데 유용합니다:

```php
$request->session()->flash('status', 'Task was successful!');
```

여러 요청-request에 대해서 임시 데이터를 보다 오랫동안 유지할 필요가 있을 때, reflash 메소드를 사용하면 임시 데이터가 추가 요청-request에 대해서도 계속 유지됩니다. 지정한 임시 데이터만을 계속 유지하고자 한다면, keep 메소드를 사용하면 됩니다:

```php
$request->session()->reflash();

$request->session()->keep(['username', 'email']);
```

## 데이터 삭제하기
---
forget 메소드는 세션에서 데이터를 삭제합니다. 세션에서 모든 데이터를 삭제하기를 원한다면 flush 메소드를 사용하면 됩니다:

```php
$request->session()->forget('key');

$request->session()->flush();
```

## 세션 ID 다시 생성하기
---
세션 ID를 다시 생성하는 것은 종종 악의적인 사용자의 애플리케이션에 대한 세션 fixation 공격을 방지하기 위해서 수행합니다.

라라벨에 포함된 LoginController 사용하는 경우 인증 과정에서 세션 ID가 자동으로 재생성됩니다; 그렇지만 세션 ID를 수동으로 다시 생성할 필요가 있다면 regenerate 메소드를 사용할 수 있습니다.

```php
$request->session()->regenerate();
```
