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

# Request 엑세스 하기
---

의존성 주입을 통해서 현재 HTTP request의 인스턴스를 획득하기 위해서는 컨트롤러 메소드에서 Illuminate\Http\Request 클래스를 타입힌트해야 합니다. 유입된 request의 인스턴스는 서비스 컨테이너에 의해서 자동으로 주입될 것입니다.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Store a new user.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        $name = $request->input('name');

        //
    }
}
```

## 의존성 주입 & 라우트 파라미터
---

만약 컨트롤러 메소드에서 라우트 파라미터로 부터 입력값을 받아야 한다면, 다른 의존성을 지정한 뒤에 라우트 파라미터를 나열해야 합니다. 예를 들어 라우트는 다음과 같이 정의될 수 있습니다:

```php
Route::put('user/{id}', 'UserController@update');
```

다음과 같이 컨트롤러 메소드를 정의하면 Illuminate\Http\Request를 타입힌트하여 라우트 파라미터 id에 접근할 수 있습니다:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Update the specified user.
     *
     * @param  Request  $request
     * @param  string  $id
     * @return Response
     */
    public function update(Request $request, $id)
    {
        //
    }
}
```

## 라우트 클로저를 통해서 Request 엑세스하기
---

또한 라우트 클로저에서도 Illuminate\Http\Request 클래스를 타입힌트 할 수 있습니다. 서비스 컨테이너는 클로저가 실행될 때 자동으로 유입된 request 를 주입할 것입니다:

```php
use Illuminate\Http\Request;

Route::get('/', function (Request $request) {
    //
});
```

## Request 경로 & 메소드
---

Illuminate\Http\Request 인스턴스는 애플리케이션의 HTTP request를 검사할 수 있는 다양한 메소드를 제공하며 Symfony\Component\HttpFoundation\Request 클래스를 상속 받고 있습니다. 몇가지 가장 중요한 메소드를 알아보겠습니다:

## Request 경로 조회하기
---

path 메소드는 request의 경로정보를 반환합니다. 따라서 들어오는 request가 http://domain.com/foo/bar를 대상으로 한다면 path 메소드는 foo/bar를 반환합니다:

```php
$uri = $request->path();
```

is 메소드는 들어오는 request가 특정 패턴에 상응한다는 것을 확인할 수 있게 해줍니다. 이 메소드를 활용할 때 * 기호를 와일드카드로 쓸 수 있습니다:

```php
if ($request->is('admin/*')) {
    //
}
```

## Request URI 조회하기
---
유입되는 request 의 전체 URL을 조회하기 위해서 url 또는 fullUrl 메소드를 사용할 수 있습니다. url 메소드는 쿼리 스트링 없는 URL을, fullUrl 은 쿼리 스트링을 포함한 URL을 반환합니다:

```php
// Without Query String...
$url = $request->url();

// With Query String...
$url = $request->fullUrl();
```

## Request HTTP 메소드(verb) 조회하기
---
method 메소드는 request에 대해 HTTP 메소드를 반환합니다. HTTP 메소드가 특정 문자열에 대응하는 것을 확인하기 위해 isMethod 메소드를 사용할 수 있습니다:

```php
$method = $request->method();

if ($request->isMethod('post')) {
    //
}
```

## PSR-7 Requests
---

PSR-7 표준은 요청과 응답을 포함한 HTTP 메세지들에 대한 인터페이스를 지정합니다. 라라벨의 request 대신 PSR-7 요청의 인스턴스를 획득하기 위해서는 우선 몇 개의 라이브러리를 설치해야 합니다. 라라벨은 Symfony HTTP Message Bridge 컴포넌트를 사용하여 일반적인 라라벨의 request-요청과 response-응답을 PSR-7에 맞는 구현체로 변환합니다:

```php
composer require symfony/psr-http-message-bridge
composer require zendframework/zend-diactoros
```

이 라이브러리들을 설치하였다면 라우트 클로저나 컨트롤러 메소드에 request를 타입힌트하여 PSR-7 request를 얻을 수 있습니다:

```php
use Psr\Http\Message\ServerRequestInterface;

Route::get('/', function (ServerRequestInterface $request) {
    //
});
```
{tip} 라우트나 컨트롤러에서 반환된 PSR-7 response 인스턴스는 자동으로 라라벨 response 인스턴스로 변환되어 프레임워크에 표시될 것입니다.


