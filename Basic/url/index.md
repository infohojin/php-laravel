---
layout: laravel
title: Laravel
subtitle: Basic
cate:
    name: "기본사항"
    link: "/Laravel/Basic"

---
URL 생성
소개하기
기본적인 내용
기본 URL 생성하기
현재 URL 엑세스하기
이름이 지정된 라우트 URL
Signed URLs
컨트롤러 액션 URL
기본값

## 소개하기
---

라라벨은 URL을 생성을 위해서 몇개의 헬퍼 함수를 제공합니다. 
url 헬퍼함수를 통하여 템플릿 내에서 API 응답을 위한 링크를 생성할 수 있습니다. 또는, 다른 곳으로 이동할 수 있는 리다이렉트 response를 생성할 때도 매우 유용하게 사용됩니다.

<br><br><br>

## 기본적인 내용
---

### 기본 URL 생성하기
---

`url` 헬퍼는 애플리케이션에서 필요한 임의의 URL을 생성 합니다.
생성한 URL은 자동으로 현재 request의 (HTTP 또는 HTTPS) 스키마와 호스트를 사용합니다:

```php
$post = App\Post::find(1);

echo url("/posts/{$post->id}");

// http://example.com/posts/1
```

<br>

### 현재 URL 엑세스하기
---

`url` 헬퍼 함수를 호출할때 아무런 인자값을 지정하지 않는 경우 `Illuminate\Routing\UrlGenerator` 인스턴스를 반환합니다.
이렇게 반환되는 인스턴스를 통하여 메소드 체인연결을 통하여 URL에 대한 다양한 정보를 엑세스 할 수 있습니다:

```php
// Get the current URL without the query string...
echo url()->current();

// Get the current URL including the query string...
echo url()->full();

// Get the full URL for the previous request...
echo url()->previous();
```

또는 각각의 메소드를 URL 파사드 형식으로엑세스 할 수 있습니다:

```php
use Illuminate\Support\Facades\URL;

echo URL::current();
```

<br><br><br>

### 이름이 지정된 라우트 URL
---

`route` 헬퍼 함수는 이름이 지정된 라우트를 URL로 생성을 할 수 있습니다.

라우트에 이름을 지정하게 되면 실제 URL에 영향을 받지 않고 정의된 URL을 생성할 수 있습니다. 

따라서 라우트에서 URL을 변경한대 해도 `route` 함수로 호출한 곳은 다시 수정할 필요가 없습니다. 

다음과 깉이 라우터에 이름이 지정되어 있다고 가정해 봅니다:

```php
Route::get('/post/{post}', function () {
    //
})->name('post.show');
```

이렇게 지정된 라우터의 이름을 통하여 

```php
echo route('post.show', ['post' => 1]);

// http://example.com/post/1
```

`route` 헴퍼 함수를 통하여 지정한 라우트 이름에 대한 URL을 생성할 수 있습니다.

다음과 같이 Eloquent 모델의 기본 키를 이용하여 URL을 생성할 수도 있습니다. 


```php
echo route('post.show', ['post' => $post]);
```
위와 같이 파라미터 값을 Eloquent 모델을 전달할 수도 있습니다. 이때, `route` 헬퍼 함수는 자동으로 Eloquent 모델의 기본 키를 추출하여 사용하게 됩니다:

<br>

### 서명이 적용된 (signed) URL
---

이름이 지정된 라우트(named routed)를 통하여 "서명이 적용된(signed)" URL을 손쉽게 만들 수 있습니다.

URL은 "서명이 적용된" 해쉬를 자동적으로 쿼리 스트링뒤에 추가하게 됩니다. 그리고 URL이 생성된 이후에는 수정되지 않았음을 확인할 수 있게 됩니다. 

서명이 적용된 URL은 공개적으로 엑세스가 가능하지만 조작에 대한 보호 레이어가 필요한 라우트에 특별히 유용합니다.


서명이 적용된 URL은 고객들의 이메일이 공개된 되어 있습니다. 이를 "구독 취소" 링크를 구현하는데 사용할 수 있습니다. 

이름이 지정된 라우트에 서명이 적용된 URL을 구성을 위해서는 URL 파사드에 signedRoute 메소드를 사용합니다:

```php
use Illuminate\Support\Facades\URL;

return URL::signedRoute('unsubscribe', ['user' => 1]);
```

`temporarySignedRoute` 메소드를 사용하면 임시적인 서명이 적용된 라우트 URL을 생성할 수 있습니다:

```php
use Illuminate\Support\Facades\URL;

return URL::temporarySignedRoute(
    'unsubscribe', now()->addMinutes(30), ['user' => 1]
);
```

<br>

## 서명이 적용된 라우트 Request-요청 Validting-유효성검사하기
--

request에 유효한 서명이 있는지 확인하기 위해서는 Request의 `hasValidSignature` 메소드를 호출 합니다:

```php
use Illuminate\Http\Request;

Route::get('/unsubscribe/{user}', function (Request $request) {
    if (! $request->hasValidSignature()) {
        abort(401);
    }

    // ...
})->name('unsubscribe');
```

또는, 라우트의 `Illuminate\Routing\Middleware\ValidateSignature` 미들웨어를 지정할 수도 있습니다. 

만일 이 미들웨어가 존재하지 않는 경우에 다음과 같이 HTTP 커널의 `routeMiddleware` 배열에 이 미들웨어를 추가 하여야 합니다:

```php
/**
 * The application's route middleware.
 *
 * These middleware may be assigned to groups or used individually.
 *
 * @var array
 */
protected $routeMiddleware = [
    'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
];
```

커널에 미들웨어를 등록을 한 후에는 라우트에도 이 미들웨어를 같이 추가해야 합니다. 

```php
Route::post('/unsubscribe/{user}', function (Request $request) {
    // ...
})->name('unsubscribe')->middleware('signed');
```

request에 유효한 서명이 없다면, 미들웨어는 자동으로 403 오류 response-응답을 반환합니다:

<br><br><br>










## 컨트롤러 액션 URL
---

`action` 헬퍼 함수는 주어진 컨트롤러 액션에 대한 URL을 생성합니다. `App\Http\Controllers` 네임스페이스로부터 시작하는 컨트롤러 클래스 이름을 전달합니다.

이때 컨트롤러의 전체 네임스페이스를 전달할 필요는 없습니다. 

```php
$url = action('HomeController@index');
```

컨트롤러의 메소드에 라우트 파라미터 인자를 같이 전달할 때는 두번째 인자에 배열을 전달 합니다:

```php
$url = action('UserController@profile', ['id' => 1]);
```

<br><br><br>


## 기본값
---

URL 파라미터에 대힌 reqeust의 기본값을 지정할 수 있습니다. 

예를 들어, 다수의 라우트에서 {locale} 파라미터를 정의한다고 할때:

```php
Route::get('/{locale}/posts', function () {
    //
})->name('post.index');
```

매번 `route` 헬퍼를 호출할 때마다 locale 을 같이 전달하는 것은 번거로울 수 있습니다.

`URL::defaults` 메소드를 사용하면 현재 request 중에서 항상 적용되는 파라미터 기본값을 같이 사용할 수 있습니다.

또한 라우트 미들웨어에서 이 메소드를 호출을 통하여 현재의 request에 엑세스 할 수 있습니다:

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Support\Facades\URL;

class SetDefaultLocaleForUrls
{
    public function handle($request, Closure $next)
    {
        URL::defaults(['locale' => $request->user()->locale]);

        return $next($request);
    }
}
```

locale 파라미터의 기본값이 설정되면 `rotue` 헬퍼 함수는 URL을 생성 할 때 매번 값을 전달하지 않아도 됩니다.