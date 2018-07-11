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

## Request 엑세스 하기
---

컨트롤러에서 요청된 HTTP `request`의 인스턴스를 얻기 위해서는 의존성 주입이 필요합니다. 의존성 주입을 할 경우 `Illuminate\Http\Request` 클래스를 타입 힌트로 설정할 수 있습니다.

`request` 인스턴스는 서비스 컨테이너에 의해서 자동으로 의존성 주입이 처리될 것입니다.


다음은 의존성 주입의 예제 입니다:
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

<br>

## 의존성 주입 & 라우트 파라미터
---

컨트롤러의 어떤 매소드가 라우트에서 치리된 파라미터값을 필요로 할 때가 있습니다. 이런경우 다른 의존성을 지정한 후에 라우트 파라미터를 나열해야 만 합니다. 

예를 들어 라우트에서 다음과 같이 정의가 되어 있다고 생각해 봅니다:

```php
Route::put('user/{id}', 'UserController@update');
```

위와 같이 라우터 정의에 대해서, 컨트롤러의 메소드는 다음과 같이 정의 할 수 있습니다.
`Illuminate\Http\Request`를 타입 힌트하여 라우트 파라미터 id에 접근할 수 있습니다:

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

<br>

## 라우트 클로저를 통해서 Request 엑세스하기
---

라우트를 설정할때 같이 정의되는 클로저에서  `Illuminate\Http\Request` 클래스를 타입 힌트 정의 할 수 있습니다. 

서비스 컨테이너는 클로저 함수가 실행될 때 자동으로 유입된 `request` 를 의존성 주입을 하게 됩니다:

```php
use Illuminate\Http\Request;

Route::get('/', function (Request $request) {
    //
});
```

위의 코드는 라우터 설정에서 의존성 주입을 설정합니다.

<br>

## Request 경로 & 메소드
---

`Illuminate\Http\Request` 인스턴스는 애플리케이션의 HTTP `request`를 검사할 수 있는 다양한 메소드를 제공합니다.
이는 `Symfony\Component\HttpFoundation\Request` 클래스를 상속 받고 있습니다. 

이와 관련된 몇가지 중요한 메소드에 대해서 알아보겠습니다:

### Request 경로 조회하기
---

`path()` 메소드는 `request`의 경로 정보를 반환합니다. 
HTTP 요청처리가 `http://domain.com/foo/bar`와 같다면 `path` 메소드는 도메인 이후의 `foo/bar`를 반환 하게 됩니다:

```php
$uri = $request->path();
```

`is()` 메소드는 요청된 `request`에 대해서 특정 패턴을 확인할 수 있습니다. 
이 메소드를 이용하면 * 기호를 와일드카드로 사용 할 수 있습니다:

```php
if ($request->is('admin/*')) {
    //
}
```

<br>

### Request URI 조회하기
---

유입되는 `request` 의 전체 URL을 조회하기 위해서 url 또는 fullUrl 메소드를 사용할 수 있습니다. 
url 메소드는 쿼리 스트링 없는 URL을, fullUrl 은 쿼리 스트링을 포함한 URL을 반환합니다:

```php
// Without Query String...
$url = $request->url();

// With Query String...
$url = $request->fullUrl();
```

### Request HTTP 메소드(verb) 조회하기
---
method 메소드는 request에 대해 HTTP 메소드를 반환합니다. HTTP 메소드가 특정 문자열에 대응하는 것을 확인하기 위해 isMethod 메소드를 사용할 수 있습니다:

```php
$method = $request->method();

if ($request->isMethod('post')) {
    //
}
```
<br>

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


