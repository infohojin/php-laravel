---
layout: laravel
title: Laravel
subtitle: Basic
cate:
    name: "기본사항"
    link: "/Laravel/Basic"

---


## Response
---

HTTP Response 는 클라이언트 브라우저가 서버로 요청한 Request의 서버 응답 입니다. 서버는 요청된 Request를 처리하여 Response를 생성을 해야 합니다. 이렇게 생성된 Reponse는 다시 브라우저에서 분석처리되어 화면에 출력하게 됩니다.

<br>

## Response 생성
---

라라벨은 요청된 Request의 Response를 생성 처리할 수 있는 다양한 방법을 제공합니다. 그리고 이렇게 생성된 response를 반환하게 됩니다.
라라벨이 생성된 response를 반환하는 방법은 몇가지가 있습니다.

<br>

### 문자열 & 배열
---

가장 간단한 Response는 문자열을 출력하는 것입니다. 라우터에서 다음과 같이 코드를 작성합니다:

```php
Route::get('/', function () {
    return 'Hello World';
});
```

라우터에서 이렇게 문자열을 반환하게 되면 라라벨은 자동으로 문자열을 HTTP response로 변환하여 처리하게 됩니다.
문자열을 반한것 외에도 배열을 반환 할 수도 있습니다. 배열을 반환하게 되면, 라라벨은 배열을을 JSON 타입의 문자열로 변환하여 응답 처리를 하게 됩니다.

```php
Route::get('/', function () {
    return [1, 2, 3];
});
```

라우트, 컨트롤러에서 Eloquent 컬렉션을 통하여 JSON으로 변환하여 반환을 할 수도 있습니다.

<br>

### Response 객체
---

문자열, 배열을 통하여 간하게 response 생성 방법에 대해서 알아 보았습니다.
하지만, 이렇게 간단한 응답처리와 달리 실제 개발환경은 좀더 복잡합니다.

응답처리를 위해서 `view`를 통하여 반환처리하거나, 또는 `Illuminate\Http\Response` 인스턴스를 생성하여 처리합니다.

다음은 response 헬퍼 함수를 통한 방법입니다:

```php
Route::get('home', function () {
    return response('Hello World', 200)
            ->header('Content-Type', 'text/plain');
});
```

라라벨의 `response` 헬퍼 함수는 Response 인스턴스를 반환 합니다. 따라서 관련 추가 메서드를 같이 연결하여 사용을 할 수 있습니다. 대표적으로 `response` 해더의 HTTP 상태 코드를 변경할 수도 있습니다.

라라벨의 `Response` 인스턴스는 `Symfony\Component\HttpFoundation\Response` 클래스를 상속받고 있습니다. 다양한 심포니 매소드를 추가적으로 사용할 수 있습니다.

<br>

### Response 헤더 추가
---

`response` 객체에서 제공되는 대부분의 메소드 들은 인스턴스를 통한 체인 형태로 연결해서 사용이 가능합니다.

```php
return response($content)
            ->header('Content-Type', $type)
            ->header('X-Header-One', 'Header Value')
            ->header('X-Header-Two', 'Header Value');
```

위와 같이 브라우저로 `response`를 전송하기전에 header 메소드를 계속해서 추가할 수 있습니다.

또는, 추가하고자 하는 해더가 여러개일 경우 `withHeaders` 메소드를 이용하여 배열로도 전달을 할 수 있습니다.

```php
return response($content)
            ->withHeaders([
                'Content-Type' => $type,
                'X-Header-One' => 'Header Value',
                'X-Header-Two' => 'Header Value',
            ]);
```
<br>

### Responses Cookies 추가
---

response에 `cookie` 메소드를 통하여 쿠키를 추가할 수 있습니다.
다음과 같이 `response` 인스턴스에서 cookie 메소드를 이용하여 체인 연결할 수 있습니다. 

```php
return response($content)
                ->header('Content-Type', $type)
                ->cookie('name', 'value', $minutes);
```

reponse에서 제공되는 `cookie` 메소드 또한 기존 PHP의 내장된 setcookie 함수와 동일한 몇개의 인자들을 더 설정할 수 있습니다.
추가 설정 가능한 인자들은 다음과 같습니다:
```php
->cookie($name, $value, $minutes, $path, $domain, $secure, $httpOnly)
```

또는, Cookie 파사드를 통하여 response 에 쿠키를 추가할 수 있습니다.  
queue 메소드는 Cookie 인스턴스를 생성하는데 필요한 인자를 설정할 수 있습니다. 쿠키는 응답이 브라우저로 보내지기 전에 추가 하여야 합니다.

```php
Cookie::queue(Cookie::make('name', 'value', $minutes));

Cookie::queue('name', 'value', $minutes);
```
<br>

### 쿠키 & 암호화
---

라라벨에서 생성되는 모든 쿠키는 보완을 위해서 암호화 됩니다. 그리고 서명이 적용되어 클리이언트에서는 수정하거나 확인할 수 없습니다. 
생성되는 쿠키의 일부분에 대해서 암호화 작업을 비활성화 하고자 한다면, `app/Http/Middleware` 디렉토리 안에 있는 `App\Http\Middleware\EncryptCookies` 미들웨어의 `$except` 속성을 이용하면 됩니다:

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

<br><br><br>

## 리다이렉트
---

Response의 리다리엑트 메소드는 사용자를 다은 URL로 전송하는 해더를 포함하고 있습니다. 리다이렉트는 `Illuminate\Http\RedirectResponse` 클래스의 인스턴스입니다.
`RedirectResponse` 인스턴스를 생성하는 방법은 두가지 방식이 있습니다.

가장 쉽게 사용할 수 있는 방법은 글로벌 redirect 헬퍼를 이용하는 것입니다:

```php
Route::get('dashboard', function () {
    return redirect('home/dashboard');
});
```

헬퍼함수는 어디에서든지 제약을 받지 않고 바로 사용할 수 있는 장점이 있습니다.

우리는 가끔씩 HTML 폼양식을 처리하는 과정에서 요청처리가 유효하지 않을 경우 다른 페이지로 리다이렉트 하는 경우가 종종 발생합니다. 예를 들어 로그인된 화면을 처리할 때 자주 사용합니다.
이런 경우에는 글로벌 `back` 헬퍼 함수를 사용할 수 있습니다. `back` 헬퍼 함수는 세션을 사용합니다.

다음과 같이 라우트 호출에서 back 함수를 사용할때 `web` 미들웨어 그룹 안에 있거나 세션 미들웨어가 적용되어 있어야 합니다:

```php
Route::post('user/profile', function () {
    // Validate the request...

    return back()->withInput();
});
```

<br>

## 이름을 이용한 라우트로 리다이렉트
---

`redirect` 헬퍼 함수를 호출할때 아무런 인자값 없이 호출을 하게 되면 `Illuminate\Routing\Redirector` 인스턴스를 반환합니다.

`Redirector` 인스턴스는 여러개의 메소드들을 제공하는데 다음과 같이 체인형태로 연결하여 사용을 할 수 있습니다.
그중에서 `route` 메소드는 이름이 지정된 라우터를 지정하여 리다이렉션을 할 수 있습니다.

```php
return redirect()->route('login');
```

이름이 지정된 라우트로 리다이렉션을 처리할때, 인자값도 같이 전달을 할 수 있습니다. 이때 두번째 인자로 배열을 전달 하면 됩니다.

```php
// For a route with the following URI: profile/{id}

return redirect()->route('profile', ['id' => 1]);
```

<br>








### Eloquent 모델을 통한 매개 변수 채우기
---

Eloquent 모델을 사용할 경우 얻게되는 "ID" 파라미터를 통하여 라우트를 리다이렉트 할 수 있습니다. 그리고 그 모델 자체를 전달할 수 있습니다. 
이때 ID 는 자동으로 추출됩니다:

```php
// For a route with the following URI: profile/{id}

return redirect()->route('profile', [$user]);
```

만일 라우트 파라미터에 저장되어 있는 값을 별도로 커스터마이징 할 수 있습니다. 이런경우, Eloquent 모델의 getRouteKey 메소드를 오버라이드 하여 처리해야 합니다.

```php
/**
 * Get the value of the model's route key.
 *
 * @return mixed
 */
public function getRouteKey()
{
    return $this->slug;
}
```

<br>

### 컨트롤러 액션으로 리다이렉트 하기
---

`action` 메소드를 이용하여 리다이렉트 동작으로 컨트롤러의 액션을 연결할 수도 있습니다. 액션으로 처리를 리다이렉션 할때는 컨트롤러와 액션 매소드를 같이 전달을 하여야 합니다.
이때 컨트롤러의 네임스페이스 부분은 고려하지 않아도 됩니다. 네임스페이스는 라라벨이 자동으로 베이스 컨트롤러로 설정을 해줍니다.

```php
return redirect()->action('HomeController@index');
```

`action` 메소드로 리다이렉트 할때 파라미터 값이 필요한 경우에는 두번째 인자로 배열을 전달합니다:

```php
return redirect()->action(
    'UserController@profile', ['id' => 1]
);
```

<br>

## 외부 도메인으로 리다이렉트 하기
---

리다이렉트 동작으로 외부의 도메인을 설정할 수도 있습니다.

`away` 메소드는 추가적으로 URL 인코딩, 유효성 검사와 같은 확인과정을 하지 않고도 `RedirectResponse` 처리를 생성할 수 있습니다.


```php
return redirect()->away('https://www.google.com');
```

<br>

### 세션의 임시 데이터와 함께 리다이렉트 하기
---

세션 데이터를 저장과 함께 디라이렉트를 같이 처리할 수도 있습니다. 성공 메시지와 같은 어떠한 값을 임시 저장을 하고 다음 엑션 메서드를 수행하여 완료를 할 수 있습니다.

```php
Route::post('user/profile', function () {
    // Update the user's profile...

    return redirect('dashboard')->with('status', 'Profile updated!');
});
```

이를 통하여 `RedirectResponse` 인스턴스를 생성과 동시에 체이닝 방식을 통하여 데이터를 세션으로 임시 저장할 수 있습니다.

리다이렉션 된 이후에는 세션에서 임시 저장된 데이터를 조회할 수 있습니다.

```php
@if (session('status'))
    <div class="alert alert-success">
        {{ session('status') }}
    </div>
@endif
```

위와 같이 블레이드 템플릿 문법으로 사용이 가능합니다.

<br><br><br>

## 다른 Response 타입들
---

`response` 헬퍼 함수는 다른 타입의 `response` 인스턴스를 생성할 수 있습니다. 

`response` 헬퍼 함수의 인자 없이 호출하면 `Illuminate\Contracts\Routing\ResponseFactory` contract 의 구현체가 반환됩니다. 
이 contract response를 생성하는 방식은 몇가지가 있습니다:

<br>

### View Responses
---

view 메소드를 사용하여 `response` 내용을 반환할 수 있습니다. 이때 status 와 header를 같이 설정하여 사용할 수 있습니다.


```php
return response()
            ->view('hello', $data, 200)
            ->header('Content-Type', $type);
```

위와 같이 개발자가 직접 HTTP 상태 코드나, 특정 헤더 값을 직접 설정할 필요가 없을 때에는 글로벌 `view` 헬퍼 함수를 사용합니다.

<br>

### JSON Responses
---

`response` 응답으로 JSON 데이터를 반환할 수 있습니다. json 데이터를 반환하기 위해서는 먼저 `Content-Type` 헤더를 `application/json` 으로 설정해야 합니다.

하지만 `json` 메소드를 사용하게 되면 자동으로 해더를 설정하여 배열을 json 으로 변환하여 출력을 합니다.
배열을 json으로 변환할때는 PHP의 기본 내장되어 있는 `json_encode` 함수를 사용합니다.

```php
return response()->json([
    'name' => 'Abigail',
    'state' => 'CA'
]);
```

JSONP response 형태로 생성이 필요한 경우에는 `json` 메소드와 `setCallback`를 같이 조합하여 사용하면 됩니다:

```php
return response()
            ->json(['name' => 'Abigail', 'state' => 'CA'])
            ->withCallback($request->input('callback'));
```            

<br>

## 파일 다운로드
---

클라이언트 브라우저에서 서버의 파일을 내려 받을수 있습니다. `download` 메소드는 주어진 경로에 있는 파일을 다운로드 하게 할 수 있는 response를 생성하여 줍니다.

`download` 메소드는 다운로드 파일 이름을 두번째 인자로 받습니다. 마지막으로 HTTP 헤더의 배열을 세번째 인자로 전달할 수 있습니다:

```php
return response()->download($pathToFile);

return response()->download($pathToFile, $name, $headers);

return response()->download($pathToFile)->deleteFileAfterSend(true);
```

다운로드하는 파일을 관리하기 위해서는 `Symfony HttpFoundation` 클래스를 사용합니다. 이때 파일명의 형식은 ASCII 로 지정해야 합니다.

<br>

### 스트리밍 다운로드
---

`streamDownload` 메소드는 파일을 다운로드와 달리 동작의 결과 내용을 디스크에 저장하지 않고 바로 다운로드 할 수 있도록 response를 생성할 수 있습니다. 

```php
return response()->streamDownload(function () {
    echo GitHub::api('repo')
                ->contents()
                ->readme('laravel', 'laravel')['contents'];
}, 'laravel-readme.md');
```

`streamDownload` 메소드는 콜백과, 파일이름 그리고 옵션값으로 헤더 배열을 인자로 전달받습니다.

<br>

### 파일 Responses
---

`file` 메소드는 파일을 다운로드 하는 것과 달리 브라우저에 직접 이미지나 PDF 와 같은 파일을 표시합니다.

```php
return response()->file($pathToFile);

return response()->file($pathToFile, $headers);
```

`file` 메소드는 첫번째 인자로 파일의 경로를, 두번째 인자롣 헤더의 배열을 전달 받습니다.

<br><br><br>






## Response 매크로
---

`Response` 파사드의 `macro` 메소드를 통하여 커스텀 `response`를 정의할 수 있습니다. 컴스텀 response는 다양한 라우트와 컨트롤러에서 재사용할 수 있습니다.

예를 들어 서비스 프로바이더의 boot 메소드를 살펴보겠습니다:

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Response;

class ResponseMacroServiceProvider extends ServiceProvider
{
    /**
     * Register the application's response macros.
     *
     * @return void
     */
    public function boot()
    {
        Response::macro('caps', function ($value) {
            return Response::make(strtoupper($value));
        });
    }
}
```

`macro` 메소드는 첫 번째 인자로 매크로 이름을 지정합니다. 두 번째로 클로저 함수를 인자로 받습니다.

```php
return response()->caps('foo');
```

매크로로 등록된 클로저는 response 헬퍼 함수로 ResponseFactory 구현 객체를 통하여 호출됩니다.
