---
layout: laravel
title: Laravel
subtitle: Basic
cate:
    name: "Request"
    link: "/Laravel/Basic"
---

* Request-요청 엑세스 하기
    - Request-요청 경로 & 메소드
    - PSR-7 Requests-요청
    - 
* 입력값 Trim 처리 & 일반화처리

* 입력값 조회하기
    - 이전 입력값 확인하기
    - 쿠키
    - 
* 파일처리
    - 업로드된 파일 조회하기
    - 업로드된 파일 저장하기

* 신뢰할 수 있는 프록시 설정하기

## HTTP 통신

웹브라우저에서 서버로 전달되는 HTTP 메지지를 받아 상호 정보를 교환하게 됩니다.
이때 클라이언트인 브라우저에서 서버로 전달되는 요청을 `request`라고 합니다. 그리고 이에 응답하여 서버가 반응하여 처리한 결과를 전달하는 것을 `response`라고 합니다.

라라벨은 이러한 `request`와 `response`를 처리하기 위한 `http` 모듈을 제공합니다.

## 메시지
http 메시지는 `ASCII`형태로 인코딩된 텍스트 정보입니다. 이러한 메시지는 한줄이 아닌 여러줄 형태로 작성을 하여 전달이 됩니다.

HTTP 메시지는 프로토콜로 `http/1.1` 과 `http/2` 두가지 스팩으로 구분이 됩니다.
`http/2` 스팩은 컴퓨터 보다는 사람이 좀더 친화적으로 가독하기 좋게금 개선된 방식입니다. 

보통 HTTP 메시지는 브라우저, 프록시, 웹서버 등에 의해서 자동으로 생성이 되고, 전달이 됩니다.

참고] https://developer.mozilla.org/ko/docs/Web/HTTP/Messages


<br><br><br>

## 라라벨 Request 엑세스
---

브라우저로 부터 요청된 HTTP `request` 정보를 컨트롤러에서 이를 이용하기 위해서는 컨트롤러로 `request`의 인스턴스를 의존성 주입을 받아야 합니다.
컨트롤로는 `request`의 의존성을 주입받을때 `Illuminate\Http\Request` 클래스를 통하여 타입 힌트로 설정할 수 있습니다.

다음은 의존성 주입의 간단한 예제 코드 입니다:
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

`store()` 메서드는 매개변수 인자로 `Illuminate\Http\Request` 타입 힌트된 `$request` 변수값을 입력 받습니다. 이렇게 의존성 주입된 인스턴스를 통하여 `request`의 메서드 및 프로퍼티에 접근을 할 수 있습니다.

`request` 인스턴스는 서비스 컨테이너에 의해서 자동으로 의존성 주입이 처리됩니다.


<br>

## 컨트롤러에서 접근
---

컨트롤러 클래스에는 여러 기능 처리를 위한 다양한 메소드를 작성할 것입니다. 이중에서 라우트 처리가 된 `파라미터`값의 일부분을 참고해야 할 경우가 있습니다. 

예를 들어 다음과 같이 라우터에서 매핑을 통하여 컨트롤러와 매소드가 호출될때 

```php
Route::put('user/{id}', 'UserController@update');
```

컨트롤러는 다음과 같이 의존성과 라우팅된 파라미터명을 같이 인자명을 나열해 주어야 합니다.

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

클래스의 `update` 메소드는 의존성 주입된 `$request`를 통하여 `$id` 파라미터값에 접근을 할 수 있습니다.

<br>

## 라우트에서 Request 접근
---

컨트롤러에서 요청한 `request`를 접근하는 방법 외에도 라우팅 정의시 사용되는 클로저 익명함수에서도 http 요청된 `request`에 접근을 할 수 있습니다. 

```php
use Illuminate\Http\Request;

Route::get('/', function (Request $request) {
    //
});
```

위의 코드에서 처럼 URL 루트 `/`에 대해서 클로저 함수를 라우트 매핑을 정의하였습니다. 클로저 함수는 인자값으로  `Illuminate\Http\Request` 클래스 타입 힌드된 `$requst`를 의존성 주입 형태로 인자전달을 받을 수 있습니다.

`request` 인스턴스는 서비스 컨테이너에 의해서 자동으로 의존성 주입이 처리됩니다.

<br>

## Request 매소드

라라벨에서 제공하는 `Illuminate\Http\Request` 클래스는 HTTP 요청되는 정보를 확인할 수 있는 몇가지 메소드를 제공합니다. `request` 의존성을 인스턴스로 가지고 있다면 다음과 같은 기능들을 사용할 수 있습니다.

라라벨의 `Illuminate\Http\Request` 클래스는 심포니의 `Symfony\Component\HttpFoundation\Request` 클래스를 상속 받고 있습니다. 따라서 심포니의 패키지 일부가 같이 설치가 필요합니다. 이는 컴포저 의존성관리로 자동으로 설치되기 때문에 개발자는 신경을 쓰지 않아도 됩니다.

### Request 경로 조회하기
---

`path()` 메소드는 `request`가 받은 URL 경로 정보를 반환합니다. 사용자가 브라우저를 통하여 URL 요청을 `http://domain.com/foo/bar`와 같이 입력을 했다면 request의 `path` 메소드는 도메인 이후 부분의 `foo/bar`를 반환 합니다.

코드에서 사용방법은 다음과 같습니다.
```php
$uri = $request->path();
```

`is()` 메소드는 HTTP 요청 중에서 특정 패턴을 확인할 수 있습니다. 

```php
if ($request->is('admin/*')) {
    //
}
```

이 메소드를 이용하면 * 기호를 와일드카드로 사용 할 수 있습니다:

<br>

### URI 조회하기
---

전체 URL을 조회하기 위해서 `url` 또는 `fullUrl` 메소드를 사용할 수 있습니다. 

```php
// Without Query String...
$url = $request->url();

// With Query String...
$url = $request->fullUrl();
```

`url` 메소드는 쿼리 스트링 없는 전체URL을 반한하는 것과 달리 `fullUrl` 은 쿼리 스트링을 포함한 URL을 반환하게 됩니다.

### HTTP 메소드(verb) 조회하기
---

`method`는 request에 대해 HTTP 메소드를 반환합니다. 
또는 HTTP 메소드가 특정 문자열을 확인하기 위해 isMethod 메소드를 사용할 수 있습니다:

```php
$method = $request->method();

if ($request->isMethod('post')) {
    //
}
```
<br>

## PSR-7 Requests
---

PHP는 HTTP 메소드를 처리하기 위한 공통된 표준을 제안하였습니다. 이를 표준화 하기 위한 인터페이스롤 PSR-7 규약이 있습니다.

PSR-7 규약에 대한 인스턴스를 얻기 위해서는 라라벨의 `request` 외에 몇개의 추가 라이브러리 설치가 필요로 합니다. 라이브러리는 컴포저를 통하여 설치할 수 있습니다.

```php
composer require symfony/psr-http-message-bridge
composer require zendframework/zend-diactoros
```

위와 같이 추가 라이브러리를 설치하게 되면 라우트 클로저나 컨트롤러 메소드에서 `request`를 `Psr\Http\Message\ServerRequestInterface` 타입힌트를 통하여 `PSR-7 request`를 얻을 수 있습니다

```php
use Psr\Http\Message\ServerRequestInterface;

Route::get('/', function (ServerRequestInterface $request) {
    //
});
```

라라벨은 Symfony HTTP Message Bridge 컴포넌트를 사용하여 라라벨의 `request`와 `response`를 PSR-7에 맞는 구현체로 변환 처리 합니다.

라우트 또는 컨트롤러에서 PSR-7 규약으로 변환된 response 인스턴스는 자동 처리됩니다. 

<br><br><br>

## Trim처리 (Trimming)
---

HTTP 요청 처리시 포함된 문자열에 대해서 사전에 미리 정리작업이 필요할 수 있습니다. 그러기 위해서 라라벨은 미들웨어로 스택에 `TrimStrings` 과 `ConvertEmptyStringsToNull`를 포함하고 있습니다.

기본 설정된 미들웨어의 목록은 `App\Http\Kernel.php` 클래스에서 확인할 수 있습니다.
```php
/**
     * The application's global HTTP middleware stack.
     *
     * These middleware are run during every request to your application.
     *
     * @var array
     */
    protected $middleware = [
        \App\Http\Middleware\CheckForMaintenanceMode::class,
        \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
        \App\Http\Middleware\TrimStrings::class,
        \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
        \App\Http\Middleware\TrustProxies::class,
    ];
```

사전에 미들웨어 등록으로 `request` 요청시 들어오는 모든 문자 필드들은 자동으로 `trim` 처리되어 집니다.

만일 빈 문자필드가 있을 경우 null로 변환 됩니다. 라우트와 컨트롤러에서 빈 문자 처리로 인한 문제를 생각할 필요는 없습니다.

이 미들 웨어를 비활성화는 `App\Http\Kernel.php` 클래스에서 `$middleware` 배열 설정값을 제거하만 하면 됩니다.

<br>

## 입력값 조회 (Retrieving)
---

입력된 HTTP 요청 `request` 값을 조회할 수 있습니다. 이와 관련된 몇개의 메소드를 추가로 제공합니다.

<br>

### 모든 입력값 조회하기
---

`all()` 메소드는 모든 입력데이터를 배열값 형태로 조회할 수 있습니다.

```php
$input = $request->all();
```

<br><br><br>

## 입력값 조회하기
---

`input` 메소드를 사용하면 `request`에서 어떤 HTTP verb 가 사용되었는지에 대한 걱정없이 `Illuminate\Http\Request` 인스턴스를 통하여 모든 사용자 입력을 확인할 수 있습니다.
HTTP verb에 관계없이 `input` 메서드는 사용자 입력을 조회하는데 사용됩니다:

```php
$name = $request->input('name');
```

input 메소드에 두번째 인자로 기본값을 전달할 수 있습니다. Request에 요청된 입력 값이 존재하지 않는다면 기본값이 반환됩니다:

```php
$name = $request->input('name', 'Sally');
```

배열 입력을 가진 폼에서 동작할 때에는, 배열에 접근하기 위하여 "점" 표기법을 사용할 수 있습니다:

```php
$name = $request->input('products.0.name');

$names = $request->input('products.*.name');
```

<br>

### 쿼리 스트링에서 입력값 조회하기
---

`input` 메소드가 request 전체에 대하여 쿼리스트링을 포함한 payload 값을 조회 합니다. 하지만 `query` 메소드는 쿼리 스트링에서만 값을 조회합니다:

```php
$name = $request->query('name');
```

만일 쿼 리스트링에서 찾고자 하는 데이터가 없을 경우에 두번째 인자를 통하여 기본값을 설정 할 수 있습니다.

```php
$name = $request->query('name', 'Helen');
```

`query` 메소드를 호출할 때 인자 없이 호출을 할 수도 있습니다. 이런 경우 쿼리 스트링 전체 값을 배열로 반환합니다:

```php
$query = $request->query();
```

<br>

### 동적 속성을 통한 입력값 조회하기
---

`Illuminate\Http\Request` 인스턴스의 동적 속성을 사용할 수 있습니다. 사용자 입력값에 엑세스할 수 있습니다. 

만일 처리되는 응용 애플리케이션에서 html `form` 요소가 포함되어 있고, 이중에 하나가 입력값을 받는 name 필드를 가지고 있다고 생각해 봅시다.
이런 경우 다음과 같이 필드의 값에 엑세스 할 수 있습니다:

```php
$name = $request->name;
```

동적 속성을 사용하면 라라벨은 먼저 request payload 안에 있는 파라미터의 값을 우선적으로 찾습니다. 
그중에서 값이 없다면 라우트 파라미터 안에 있는 필드를 찾습니다.

<br>

### JSON 입력 값 조회하기
---

Content-Type 헤더 속성이 `application/json` 으로 지정시 `input` 메소드를 통해서 입력된 `JSON` 데이터에 접근할 수 있습니다.

```php
$name = $request->input('user.name');
```

json 데이터는 다중배열로 구성이 됩니다. 배열의 선택 구분은 `점`을 통하여 구분하여 접근근을 할 수 있습니다.

<br>

### 입력 데이터의 한 부분 조회하기
---

`only`와 `except` 메소드는 입력 데이터의 일부분만 조회할 수 있습니다. 

```php
$input = $request->only(['username', 'password']);

$input = $request->only('username', 'password');

$input = $request->except(['credit_card']);

$input = $request->except('credit_card');
```

이 두 메소드 모두 하나의 배열 또는 동적인 인자의 목록을 받아 들입니다:
only 메소드는 유입되는 request-요청에서 입력한 키 / 값 쌍을 반환합니다. 그렇지만 현재 request 에서 존재하지 않는 키/값은 반환하지 않습니다.

```php
$input = $request->intersect(['username', 'password']);
```

<br>

### 입력값이 존재하는지 확인하기
---

`has` 메소드는 HTTP 요청 `Request`에 특정한 값이 존재하는지를 확인할 수 있습니다. `has` 메소드는 현재 request 에서 값이 존재할 때 true를 반환합니다:

```php
if ($request->has('name')) {
    //
}
```

`has` 메소드에 배열이 주어지면, 지정된 모든 값이 존재하는지 확인하게 됩니다:

```php
if ($request->has(['name', 'email'])) {
    //
}
```

주어진 변수값이 현재 request 에 존재하고 비어 있지 않은 것을 함께 확인을 하려면 `filled` 메소드를 사용할 수 있습니다.

```php
if ($request->filled('name')) {
    //
}
```

<br><br><br>

## 이전 입력값 확인하기
---

라라벨은 현재 입력한 `request`의 값을 다음 `request` 요청에서도 값을 접근할 수 있도록 유지 시킬 수 있습니다. 이 기능은 HTML 폼을 작성할때 오류를 체크하여 다시 입력시 값을 유지할 수 있도록 처리하는데 매우 유용합니다.

라라벨의 유효성 검사기능을 사용할때 이전값을 유지하는 기능은 자동으로 같이 처리가 됩니다. 따라서 유효성 검사를 할때는 일일이 수동으로 매소드 동작을 처리할 필요가 없습니다.

### 세션에 임시 저장
---

입력값 유지는 원리는 세션을 이용하는 것입니다. `Illuminate\Http\Request` 클래스의 flash 메소드는 현재의 입력들을 세션에 저장합니다. 
이로 인하여 사용자는 요청한 request를 다음 request 에도 값을 유지하여 사용할 수 있게 되는 것입니다.

```php
$request->flash();
```

전체의 데이터를 세션에 유지하는 것 과 달리 `flashOnly`와 `flashExcept` 메소드를 이용하여 request 데이터의 일부분만 세션에 임시 저장(flash)할 수 있습니다. 

```php
$request->flashOnly(['username', 'email']);

$request->flashExcept('password');
```

이 메소드를 이용하면 전체 값을 저장하지 않고, 패스워드와 같은 민감한 정보들은 세션에 저장하는 것을 제외할 수 있습니다.

<br>

### 입력값을 임시저장한 후 리다이렉트하기
---

HTTP의 요청을 처리할 경우 가끔식 우리는 다른 URL로 리다이렉트를 할때가 많이 발생을 합니다. 그리고 이때 입력값을 세션에 임시로 저장하여 전달이 필요한 경우도 발생을 합니다.

```php
return redirect('form')->withInput();

return redirect('form')->withInput(
    $request->except('password')
);
```

리다이렉트 메소드 실행과 함께 체인방식으로 입력값 임시 저장을 함께 수행할 수도 있습니다.

<br>

### 이전 입력값 조회하기
---

이전 HTTP 요청의 `request` 로 인하여 저장된 입력값은 Request 인스턴스의 `old `메소드를 사용하여 조회 할 수 있습니다.
`old` 메소드는 세션에 입력 저장된 임시 데이터를 찾아서 반환합니다.

```php
$username = $request->old('username');
```

라라벨은 `old` 메서드를 호출할 수 있는 글로벌 `old 헬퍼 함수`도 같이 제공합니다. 헬퍼 함수를 이용하면 템플릿 엔진에서도 이전의 request 입력값을 찾아 사용을 할 수 있는 장점이 있습니다.
다음은 템플릿 엔진에서 헬퍼함수를 호출하는 예제 코드 일부입니다:

```php
<input type="text" name="username" value="{{ old('username') }}">
```

주어진 필드명에 대해서 이전 입력값이 없는 경우, null 을 반환합니다.

<br>


## 쿠키
---

쿠기는 서버측이 아닌 접속자 브라우저에 저장이 되는 데이터 입니다. 라라벨은 쿠키 값의 보안을 위해서 인증코드와 함께 암호화 작업을 하게 됩니다. 
접속되는 클리이언트 PC가 다른 경우 쿠기값은 유효성을 상실하게 됩니다.


### Request 에서 쿠키 조회하기

`Request`에서 쿠키 값을 가져오는 방법은 `Illuminate\Http\Request` 인스턴스의 `cookie` 메소드를 사용하면 됩니다.

```php
$value = $request->cookie('name');
```

위 방법 외에도 쿠키에 접근하는 방법은 `Cookie` 파사드를 사용 하는 것입니다.

```php
$value = Cookie::get('name');
```

<br>

## 쿠키를 Response에 추가
---

라라벨은 응답처리인 `Illuminate\Http\Response` 인스턴스에 대해서 `cookie` 메소드를 활용하여 쿠키값을 추가할 수 있습니다. 

쿠키값을 추가할때는 쿠키명과 값 그리고 쿠키의 유효시간을 같이 설정하여야 합니다.
다음은 응답처리를 생성할때 쿠키값을 추가하는 예제 코드 일부 입니다:

```php
return response('Hello World')->cookie(
    'name', 'value', $minutes
);
```

`cookie` 메소드는 추가 인자 몇개를 더 받아 처리할 수도 있습니다.이 인자들은 PHP의 고유 내장된 `setcookie` 함수에서 제공되는 인자들과 동일한 목적과 의미가 있습니다:

```php
return response('Hello World')->cookie(
    'name', 'value', $minutes, $path, $domain, $secure, $httpOnly
);
```

`Cookie` 파사드를 통하여 쿠키를 추가할 수도 있습니다.
`queue` 메소드는 Cookie 인스턴스, 인스턴스를 생성에 필요한 인자를 전달 합니다.


```php
Cookie::queue(Cookie::make('name', 'value', $minutes));

Cookie::queue('name', 'value', $minutes);
```

쿠키 처리는 응답이 서버에서 브라우저로 전송되기 전에 미리 작업이 되어야 합니다.

<br>

## 쿠키 인스턴스 생성하기
---

라라벨 `response` 인스턴스에 포함되어 있는 `Symfony\Component\HttpFoundation\Cookie` 인스턴스값에 쿠키를 생성하기 위해서 글로벌 cookie 헬퍼 함수를 사용합니다.

```php
$cookie = cookie('name', 'value', $minutes);

return response('Hello World')->cookie($cookie);
```

이 쿠키는 response 인스턴스에 첨부하지 않는 한 클라이언트에게 다시 보내지지 않습니다:

<br><br><br>

## 파일처리
---

사용자가 html 폼을 통하여 파일을 업로드 하는 경우는 많이 발생을 합니다. 이렇게 폼을 통하여 파일을 업로드 할 경우 이를 처리할 수 있습니다.

<br>

### 업로드된 파일 조회하기
---

`Illuminate\Http\Request` 인스턴스의 file 메소드를 사용하면, 동적 속성을 이용하여 업로드된 파일을 엑세스 할 수 있습니다. 

`file` 메소드는 PHP의 `SplFileInfo` 클래스가 상속된 `Illuminate\Http\UploadedFile` 클래스 인스턴스를 반환 합니다. 이는 파일과 상호작용할 수 있는 다양한 메소드를 추가로 같이 제공하게 됩니다.

```php
$file = $request->file('photo');

$file = $request->photo;
```

`hasFile` 메소드를 사용하면 `request`에 파일을 가지고 있는지 확인할 수 있습니다:

```php
if ($request->hasFile('photo')) {
    //
}
```

<br>

### 성공적으로 업로드 되었는지 확인하기
---

파일 존재여부 확인과 더불어, 실제 파일의 정상적으로 업로드가 되었는지 확인할 수 있는 추가 기능을 제공합니다.
`isValid` 메소드는 업로드된 파일에 아무런 문제가 없는지 확인하는 메소드 입니다.

```php
if ($request->file('photo')->isValid()) {
    //
}
```

<br>

### 파일 경로 & 확장자
---

`UploadedFile` 클래스는 파일의 전체 경로와 확장자에 대해서 엑세스 할 수 있는 추가 메소드를 제공합니다.
그중에서 `extension` 메소드는 파일의 확장자를 알아 낼 수 있습니다. 

```php
$path = $request->photo->path();

$extension = $request->photo->extension();
```

업로드한 파일의 확장자를 검사하는 것은 보안상 매우 중요합니다. 악의적인 스크립트 코드를 업로드 할려고 하는 경우 미리 사전에 정의한 확장자 목록과 검사할 수 있습니다.

<br>

### 기타 파일 관련 메소드들
---

`UploadedFile` 인스턴스는 그외 다양한 다른 메소드를 제공합니다.
이 메소드들에 관한 보다 자세한 정보는 클래스의 API documentation을 참고 하시길 바랍니다.

<br>

### 업로드된 파일 저장하기
---

폼을 통하여 전송된 파일을 저장할 수 있습니다. 업로드 파일을 서버에 저장을 하기 위해서는 파일시스템을 선택하여야 합니다.
`UploadedFile` 클래스의 `store`메소드는 업로드 파일을 로컬 파일 시스템 또는 클라우드에 저장을 할 수 있도록 지원을 합니다.

`store` 메소드는 선택된 파일 시스템에 루트 디렉토리로 부터 파일이 어디에 저장되어야 할지에 대한 전체경로를 전달 받습니다.
또한 파일 이름은 자동적으로 고유한 ID를 부여받게 됩니다. 그러기 때문에 이 경로에는 파일 이름을 포함하지 않아야 합니다.

`store` 메소드는 선택적으로 파일을 저장합니다. 이때 사용되는 디스크 이름을 두번째 인자로 전달할 수 있습니다.
메소드는 디스크의 루트를 기준으로 파일의 경로를 반환합니다:

```php
$path = $request->photo->store('images');

$path = $request->photo->store('images', 's3');
```

만일 라라벨이 자동적으로 파일 이름을 생성하지 않도록 할 수도 있습니다. 이런경우에는 경로명과 파일이름 그리고 디스크 이름 모두를 인자로 받아 설정하는 `storeAs` 메소드를 사용하면 됩니다.

```php
$path = $request->photo->storeAs('images', 'filename.jpg');

$path = $request->photo->storeAs('images', 'filename.jpg', 's3');
```

<br><br><br>


## 신뢰할 수 있는 프록시 설정하기
---

TLS / SSL 인증서를 적용한 상태에서 라라벨을 부하분산과 같은 로드발런서와 같이 사용할 수도 있습니다. 이런경우 애플리케이션은 HTTPS 링크가 생성이 되지 않는 경우도 발생을 합니다.
이러한 문제는 로드 발렌서의 80 포트의 트래픽 부분에서 HTTPS 링크 생성을 처리해야 된다는 것을 알지 못하기 때문입니다.

이 문제를 해결하기 위해서, 라라벨은 `App\Http\Middleware\TrustProxies` 미들웨어를 제공합니다. 라라벨에서 신뢰할 수 있는 로드밸런서 또는 프록시를 설정할 수 있습니다. 

```php
<?php

namespace App\Http\Middleware;

use Illuminate\Http\Request;
use Fideloper\Proxy\TrustProxies as Middleware;

class TrustProxies extends Middleware
{
    /**
     * The trusted proxies for this application.
     *
     * @var array
     */
    protected $proxies = [
        '192.168.1.1',
        '192.168.1.2',
    ];

    /**
     * The headers that should be used to detect proxies.
     *
     * @var string
     */
    protected $headers = Request::HEADER_X_FORWARDED_ALL;
}
```

이렇게 신뢰할 수 있는 프록시들은 미들웨어에서 `$proxies` 속성 배열에 지정해놓으면 됩니다. 
신뢰할 수 있는 프록시를 구성하는 것 이외에도 신뢰해야 하는 프록시 헤더를 설정 할 수도 있습니다:


만일 AWS Elastic 로드 발란싱을 사용하고 있다면, `$headers` 값은 `Request::HEADER_X_FORWARDED_AWS_ELB` 로 되어 있어야 합니다. 
`$headers`에 사용될 수 있는 상수에 대한 정보는, Symfony 문서의 trusting proxies 부분을 참고하시면 됩니다.

<br>

## 프록시 신뢰
---

아마존 AWS 또는 다른 "클라우드" 로드밸런서를 사용하는 경우에는, 실제 로드밸런서의 IP를 알 수가 없습니다. 
모든 프록시를 신뢰할 수 있도록 하기 위해서 * 를 사용합니다.

```php
/**
 * The trusted proxies for this application.
 *
 * @var array
 */
protected $proxies = '*';
```

