---
layout: laravel
title: Laravel
subtitle: Routing
cate:
    name: "라우팅"
    link: "/Laravel/Basic/Routing"

submenus:
    -
        name: 라우팅
        link: /Laravel/Basic/Routing
    -
        name: 기본적인 라우팅
        link: /Laravel/Basic/Routing/Basic
    -
        name: 라우트 파라미터
        link: /Laravel/Basic/Routing/Parameters
    -
        name: 이름이 지정된 라우트
        link: /Laravel/Basic/Routing/Named
    -
        name: 라우트 그룹
        link: /Laravel/Basic/Routing/Groups
    -
        name: 라우트 모델 바인딩
        link: /Laravel/Basic/Routing/ModelBinding
    -
        name: Rate 제한
        link: /Laravel/Basic/Routing/RateLimiting
    -
        name: Form 메소드 Spoofing-속이기
        link: /Laravel/Basic/Routing/FormMethod
    -
        name: 현재 라우트에 엑세스하기
        link: /Laravel/Basic/Routing/Accessing
---
## 기본적인 라우팅
<hr>

라라벨의 라우트 매소드는 기본적으로 두개의 입력값을 전달 받습니다. 첫번째 인자로는 URI, 두번째 인자로는 클로저 함수를 전달 합니다.

작성 방법은 대략 다음과 같습니다.
```php
Route::get('foo', function () {
    return 'Hello World';
});
```
위의 예제에서 url 입력이 `foo`로 입력시 콜백함수를 실행합니다. 위의 예제에서 콜백함수는 문자열을 반환하는데, 라우터는 이 반환된 값을 화면에 출력을 합니다. 

<br><br>

## 라우트 파일
라라벨의 라우트 설정 파일은 `/route/web.php` 파일에 설정을 합니다. 이 라우트 설정 파일은 라라벨 프레임워크가 시작될때 시스템에 의해서 자동으로 로딩처리 됩니다.
개발자들은 프로젝트별 처리해야 되는 라우트 설정을 이곳에 작성을 해두면 됩니다.

라우트에는 세션상태, CSRF 보호와 같은 미들웨어가 같이 할당되어 있습니다. 
routes/api.php 안에 들어 있는 라우트들은 stateless 하고 api 미들웨어 그룹이 할당되어 있습니다.

<br><br>

## 컨트롤러 연결
라우트 설정의 역활은 브라우저를 통해서 유입되는 URL을 재정의하는데 목적이 있습니다. 
예를 들어 브라우저에서 http://도메인/user와 같이 접속할때, 특정 클래스와 메서드가 동작을 하기 위해서는 다음과 같이 설정을 합니다.

```php
Route::get('/user', 'UserController@index');
```

우와 같이 GET 방식, `/user` 경로로 입력을 했을때 `UserController` 컨트롤러의 `index`메소드를 실행하는 의미입니다.

미들웨어 그룹 routes/api.php 파일은 RouteServiceProvider의 라우트 그룹안에 중첩되어 정의되어 있습니다. 
이 그룹을에 의해서 /api URI가 자동으로 앞에 붙게 되므로, 이 파일에 정의한 모든 라우트에 일일이 적용하지 않아도 됩니다. 

RouteServiceProvider 파일의 라우트 그룹 옵션을 수정하면, 다른 prefix를 붙일 수도 있습니다.

<br><br>

## 가능한 라우터 메소드
사용자가 입력하는 URL은 실제적인 서버가 HTTP응답을 받아 처리하는 것은 약간의 구별이 있습니다.

라라벨의 라우터는 HTTP 응답에 따른 메소드로 열결되어 처리가 됩니다. 연결 가능한 메소드 들은 다음과 같습니다.

```php
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```
위와 같이 6가지의 메소드를 지원합니다.

때로는 한개의 uri 요청에 대해서 여러 가지 방식의 HTTP응답을 같이 처리해야 되는 경우도 발생을 합니다. 이런 경우 여러 매소드로 매칭할 수 있는 `match` 매소드를 추가로 제공하고 있습니다.

`match`메소드는 추가로 배열인자를 하나 더 받습니다. 이 배열에는 처리해야 되는 HTTP 응답요청을 등록할 수 있습니다.

```php
Route::match(['get', 'post'], '/', function () {
    //
});
```

또는 선택적 응답이 아닌, 모든 HTTP의 응답을 처리해야 하는 경우 `any`매소드를 추가로 같이 제공합니다.

```php
Route::any('foo', function () {
    //
});
```

이 처럼 라라벨 라우터는 기본 메서드 6개와 추가 메서드 2개를 포함하여 8개의 사용가능한 메서드를 제공하고 있습니다.

<br><br>

## CSRF 보호하기
HTTP요청중에 POST, PUT, DELETE는 서버에 데이터의 변경작업을 요청하는 request입니다. 동작을 보호하기 위해서 라라벨은 CSRF 토큰 기능을 같이 사용합니다.
매 요청된 request에서 CSRF 토큰키 값을 검사하고, 일치하지 않은 경우 요청동작을 거부합니다.

보다 자세한 CSRF는 관련 문서를 더 참고해 보기실 바랍니다. 다음 예제는 블레이드 템플릿에서 CSRF 코드를 삽입한 예제 입니다.
```php
<form method="POST" action="/profile">
    @csrf
    ...
</form>
```

<br><br>

## 리다이렉트 라우트

입력된 url을 다른 url의 다루트로 전달을 할 수 있습니다.
다른 URI로 리다이렉트 시키는 라우트를 정의하려면, Route::redirect 메소드를 사용하면 됩니다.
 
```php
Route::redirect('/here', '/there', 301);
```
`here`로 입력된 요청을 `there` 로 리다이렉션을 처리합니다.

이 메소드는 간단한 리다이렉트를 위해서 복잡한 라우트나 컨트롤러 전체를 정의하지 않아도 되는 편리한 방법을 제공합니다.

<br><br>

## 뷰 라우트

입력된 URL이 컨트롤러로 연결되지 않고, 단순한 View로 연결하여 화면을 출력할 수도 있습니다. 
단지 뷰를 반환하기만 하는 라우트가 필요하다면, Route::view 메소드를 사용할 수 있습니다. 

redirect 메소드와 같이 이 메소드는 간단한 리다이렉트를 위해서 복잡한 라우트나 컨트롤러 전체를 정의하지 않아도 되는 편리한 방법을 제공합니다. 

```php
Route::view('/welcome', 'welcome');

Route::view('/welcome', 'welcome', ['name' => 'Taylor']);
```

view 메소드는 첫번째 인자로 URI를, 두번째 인자로 뷰 파일의 이름을 전달 받습니다. 또한 이에 더해, 세번째 인자로 view 에 제공할 데이터들의 배열을 제공할 수도 있습니다: