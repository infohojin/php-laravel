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

## 컨트롤러 미들웨어
---

라우팅에서 컨트롤러의 미들웨어를 지정할 수 있습니다. 다음과 같이 메서드 체인 방식으로 연결이 가능합니다.

```php
Route::get('profile', 'UserController@show')->middleware('auth');
```

일일이 라우터 파일에서 미들웨어를 설정하는 방법과 또 다른 방식이 있습니다. 컨트롤러를 생성할때, 생성자에서 미들웨어를 설정하는 방법입니다.

컨트롤러의 매직 메서드 `__construct()` 안에서 필요한 `middleware` 메소드를 추가할 수 있습니다.
컨트롤러의 몇몇 메소드에서만 제한하여 미들웨어를 지정할 수도 있습니다.

다음은 컨트롤러에서 미들웨어를 설정하는 예 입니다:
```php
class UserController extends Controller
{
    /**
     * Instantiate a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('auth');

        $this->middleware('log')->only('index');

        $this->middleware('subscribed')->except('store');
    }
}
```

위의 예제를 보면 3개의 미들웨어를 설정하고 있습니다. 또한 컨트롤러에서 미들웨어를 설정할 경우에 클로저 함수를 통하여 등록을 처리 할 수도 있습니다.

이는 전체 미들웨어 클래스를 정의하지 않고 단일 컨트롤러에 대한 미들웨어를 정의하는 편리한 방법을 제공합니다:

```php
$this->middleware(function ($request, $next) {
    // ...

    return $next($request);
});
```

{tip} 컨트롤러 액션의 하위 집합에 미들웨어를 할당 할 수 있습니다. 
그러나 컨트롤러가 너무 커질 수 있음을 인지하여야 합니다. 대신 컨트롤러를 여러 개의 작은 컨트롤러로 나누는 것을 고려하세요.


