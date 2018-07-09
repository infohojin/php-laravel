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

## 이름이 지정된 라우트 URL
---

route 헬퍼 함수는 이름이 지정된 라우트에 대한 URL을 생성하는데 사용할 수 있습니다. 라우트에 이름을 지정하여 사용하면, 라우트에 정의된 실제 URL에 구애받지 않고서도 URL을 생성할 수 있습니다. 따라서 라우트의 URL이 변경되었다고 해서 route 함수를 호출한 곳을 모두 수정할 필요가 없습니다. 예를 들자면 애플리케이션에서 다음과 같이 정의된 라우트를 가지고 있다고 가정해보십시오:

```php
Route::get('/post/{post}', function () {
    //
})->name('post.show');
```
이 라우트에 대한 URL을 생성하려면 route 헬퍼 함수를 다음과 같이 사용하면 됩니다:

```php
echo route('post.show', ['post' => 1]);

// http://example.com/post/1
```

여러분은 종종 Eloquent 모델의 기본 키를 사용하여 URL을 생성하게 될 겁니다. 이러한 이유로, 파라미터 값으로 Eloquent 모델을 전달할 수도 있습니다. route 헬퍼 함수는 자동으로 모델의 기본 키를 추출하여 사용합니다:

```php
echo route('post.show', ['post' => $post]);
```

## 서명이 적용된 (signed) URL
---
라라벨에서는 이름이 지정된 라우트(named routed)에 "서명이 적용된(signed)" URL을 손쉽게 만들 수 있습니다. 이러한 URL에는 "서명이 적용된" 해쉬가 쿼리 스트링뒤에 추가되어 URL이 생성된 이후에 수정되지 않았음을 확인할 수 있게 합니다. 서명이 적용된 URL은 공개적으로 엑세스가 가능하지만 조작에 대한 보호 레이어가 필요한 라우트에 특별히 유용합니다

예를 들어, 여러분은 서명이 적용된 URL을 고객들이 이메일을 보내는 공개된 "구독 취소" 링크를 구현하는데 사용할 수 있습니다. 이름이 지정된 라우트에 서명이 적용된 URL을 구성하려면, URL 파사드에 signedRoute 메소드를 사용하면 됩니다:

```php
use Illuminate\Support\Facades\URL;

return URL::signedRoute('unsubscribe', ['user' => 1]);
```

여러분은 temporarySignedRoute 메소드를 사용하여, 임시적으로 생성되어 만료되는, 서명이 적용된 라우트 URL을 생성할 수 있습니다:

```php
use Illuminate\Support\Facades\URL;

return URL::temporarySignedRoute(
    'unsubscribe', now()->addMinutes(30), ['user' => 1]
);
```

## 서명이 적용된 라우트 Request-요청 Validting-유효성검사하기
유입되는 request-요청이 유효한 서명이 있는지 확인하기 위해서는 유입되는 Request 에 hasValidSignature 메소드를 호출해야 합니다:

```php
use Illuminate\Http\Request;

Route::get('/unsubscribe/{user}', function (Request $request) {
    if (! $request->hasValidSignature()) {
        abort(401);
    }

    // ...
})->name('unsubscribe');
```

또는, 라우트에 Illuminate\Routing\Middleware\ValidateSignature 미들웨어를 지정할 수도 있습니다. 이 미들웨어가 존재하지 않는다면, HTTP 커널의 routeMiddleware 배열에 이 미들웨어를 지정해야 합니다:

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

커널에 미들웨어를 등록한다음, 라우트에 이 미들웨어를 추가해야 합니다. 유입되는 request-요청에 유효한 서명이 없다면, 미들웨어는 자동으로 403 오류 response-응답을 반환합니다:

```php
Route::post('/unsubscribe/{user}', function (Request $request) {
    // ...
})->name('unsubscribe')->middleware('signed');
```

