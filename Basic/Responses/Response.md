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

## Response 생성하기
---

### 문자열 & 배열
---
모든 라우트와 컨트롤러는 response 를 사용자 브라우저로 다시 반환해야 합니다. 라라벨은 response 를 반환하기 위한 몇가지 다른 방법을 제공합니다. 가장 기본적인 response 는 라우트나 컨트롤러에서 문자열을 반환하는 것입니다. 프레임워크는 자동으로 문자열을 전체 HTTP response로 변환할것입니다:

```php
Route::get('/', function () {
    return 'Hello World';
});
```

라우트나 컨트롤러에서 문자열을 반환하는것에 더하여, 배열을 반환할 수도 있습니다. 프레임워크는 자동으로 배열을 JSON response로 변환할 것입니다:

```php
Route::get('/', function () {
    return [1, 2, 3];
});
```

{tip} 라우트나 컨트롤러에서 Eloquent 컬렉션 또한 반환할 수 있다는 것을 알고 있습니까? 이는 자동적으로 JSON으로 변환됩니다. 한번 해보세요!

## Response 객체
---
일반적으로, 라우트 액션에서 단순히 문자열이나 배열만 반환하지는 않습니다. 대신에 Illuminate\Http\Response 인스턴스 또는 views을 반환합니다.

Response 인스턴스를 반환하는 것은 여러분이 response 의 HTTP 상태 코드나 헤더를 변경할 수 있도록 지원합니다. Symfony\Component\HttpFoundation\Response 클래스를 구현하고 있는 Response 인스턴스는 HTTP response 를 만들기 위한 다양한 메소드를 제공합니다:

```php
Route::get('home', function () {
    return response('Hello World', 200)
                  ->header('Content-Type', 'text/plain');
});
```

## Response에 헤더 추가하기
---
대부분의 response 메소드는 response 인스턴스의 유연한 생성을 위해서 체이닝이 가능합니다. 예를 들어서 사용자에게 response를 되돌려 주기 전에 header 메소드를 통해서 header를 추가할 수 있습니다:

```php
return response($content)
            ->header('Content-Type', $type)
            ->header('X-Header-One', 'Header Value')
            ->header('X-Header-Two', 'Header Value');
```

또는, response 에 추가하고자 하는 헤더의 배열을 지정하기 위해서 withHeaders 메소드를 사용할 수 있습니다:

```php
return response($content)
            ->withHeaders([
                'Content-Type' => $type,
                'X-Header-One' => 'Header Value',
                'X-Header-Two' => 'Header Value',
            ]);
```

## Responses에 Cookies 추가하기
---
response 인스턴스의 cookie 메소드는 response 에 쿠키를 손쉽게 추가할 수 있게 해줍니다. 예를 들어 다음과 같이 response 인스턴스에서 cookie 메소드를 사용하여 쿠키를 생성하고 우아하게 이를 response 인스턴스에 추가할 수 있습니다.

```php
return response($content)
                ->header('Content-Type', $type)
                ->cookie('name', 'value', $minutes);
```

cookie 메소드는 또한 자주 사용되지 않는 몇가지 인자를 더 받아들입니다. 일반적으로 이 인자들은 PHP의 내장된 setcookie 메소드에 제공되는 인자들과 동일한 목적과 의미가 있습니다:

```php
->cookie($name, $value, $minutes, $path, $domain, $secure, $httpOnly)
```

또는, Cookie 파사드를 애플리케이션의 응답에 쿠키를 추가하기 위해서 "queue"할 수 있습니다. queue 메소드는 Cookie 인스턴스나 Cookie 인스턴스를 생성하는데 필요한 인자를 받습니다. 이 쿠키는 응답이 브라우저로 보내지기 전에 추가됩니다:

```php
Cookie::queue(Cookie::make('name', 'value', $minutes));

Cookie::queue('name', 'value', $minutes);
```

## 쿠키 & 암호화
---
기본적으로, 라라벨에서 생성되는 모든 쿠키는 암호화 되고, 서명이 적용되어 클리이언트에서는 수정하거나 확인할 수 없습니다. 애플리케이션에서 의해서 생성되는 쿠키의 일부분에서 암호화를 비활성화 하고자 한다면, app/Http/Middleware 디렉토리 안에 들어 있는 App\Http\Middleware\EncryptCookies 미들웨어의 $except 속성을 사용할 수 있습니다:

```php
/**
 * The names of the cookies that should not be encrypted.
 *
 * @var array
 */
protected $except = [
    'cookie_name',
];
```

