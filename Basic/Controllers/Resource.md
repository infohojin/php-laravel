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

## 컨트롤러 자원
---
Laravel 리소스 라우팅은 일반적인 "CRUD" 경로를 한 줄의 코드로 컨트롤러에 할당합니다. 예를 들어, 응용 프로그램에서 저장 한 "사진"에 대한 모든 HTTP 요청을 처리하는 컨트롤러를 만들 수 있습니다. make : controller Artisan 명령을 사용하여, 우리는 그러한 컨트롤러를 빠르게 만들 수 있습니다 :

```php
php artisan make:controller PhotoController --resource
```

아티즌 명령어는 app/Http/Controllers/PhotoController.php 파일을 생성할 것입니다. 이 컨트롤러는 각각의 resource 에 해당하는 메소드들을 가지고 있을 것입니다.

이제 생성된 컨트롤러에 resourceful 라우트를 등록하면 됩니다.

```php
Route::resource('photos', 'PhotoController');
```

한번의 선언만으로 photo 를 구성하는 RESTful 한 액션에 대한 다양한 라우트를 설정할 수 있습니다. 앞에서 직접 개별 메소드를 구성한것과 마찬가지로 생성된 컨트롤러는 각각의 메소드가 처리하는 URI와 액션에 대한 메모와 함께 구성됩니다.

resources 메소드에 배열을 전달하여 한번에 여러개의 리소스 컨트롤러를 등록할 수 있습니다:

```php
Route::resources([
    'photos' => 'PhotoController',
    'posts' => 'PostController'
]);
```

리소스풀 컨트롤러에 의해서 구성된 액션들
Verb	URI	Action	Route Name
GET	/photos	index	photos.index
GET	/photos/create	create	photos.create
POST	/photos	store	photos.store
GET	/photos/{photo}	show	photos.show
GET	/photos/{photo}/edit	edit	photos.edit
PUT/PATCH	/photos/{photo}	update	photos.update
DELETE	/photos/{photo}	destroy	photos.destroy

## 리소스 모델 지정하기
---
라우트 모델 바인딩을 사용하고 있고, 리소스 컨트롤러의 메소드가 모델 인스턴스에 대한 타입힌트를 하도록 원한다면 컨트롤러르르 생성할 대 --model 옵션을 사용할 수 있습니다:

```php
php artisan make:controller PhotoController --resource --model=Photo
```

## 스푸핑 폼 함수
---
HTML 폼은 PUT, PATCH 또는 DELETE 요청을 만들 수 없으므로 숨겨진 _method 필드를 추가하여 이들 HTTP 문법을 인용해야합니다. @method 블레이드 디렉티브로 필드를 생성 할 수 있습니다. :

```php
<form action="/foo/bar" method="POST">
    @method('PUT')
</form>
```

## Resource 라우트의 일부만 지정하기
---
resource 라우트를 선언할 때, 액션의 일부만을 지정할 수도 있습니다.

```php
Route::resource('photos', 'PhotoController')->only([
    'index', 'show'
]);

Route::resource('photos', 'PhotoController')->except([
    'create', 'store', 'update', 'destroy'
]);
```

## API 리소스 라우트
---
API에서 사용할 리소스 라우트를 선언하는 경우, 일반적으로 create, edit와 같은 HTML 템플릿을 표시하는 라우트는 제외하기를 원합니다. 편의를 위해서 apiResource를 사용하면 이 두가지의 라우트를 제외할 수 있습니다:

```php
Route::apiResource('photos', 'PhotoController');
```

apiResources 메소드에 배열형태의 API 리소스 컨트롤러를 전달하여 여러개를 한번에 등록할 수 있습니다:

```php
Route::apiResources([
    'photos' => 'PhotoController',
    'posts' => 'PostController'
]);
```

빠르게 create 혹은 edit 메서드들을 포함하지 않는 API 리소스 컨트롤러 생성을 원하신다면, make:controller 커맨드 명령에 --api 옵션을 사용하시면 됩니다:

```php
php artisan make:controller API/PhotoController --api
```

## Naming Resource Routes
---
기본적으로 모든 리루스 컨트롤러 액션은 라우트 이름을 가지고 있습니다. 그러나 names 옵션 배열을 전달하여 이름을 덮어씌울 수 있습니다.

```php
Route::resource('photos', 'PhotoController')->names([
    'create' => 'photos.build'
]);
```

## 리소스 라우트 파리미터 이름 지정하기
---
기본적으로 Route::resource 는 리소스 이름을 기반으로한 리소스 라우트들을 위한 라우트 파라미터들을 생성합니다. 여러분은 옵션 배열에 parameters 를 전달하여 손쉽게 각각의 리소스 마다 이를 덮어쓸 수 있습니다:

```php
Route::resource('user', 'AdminUserController')->parameters([
    'user' => 'admin_user'
]);
```

위의 예제는 리소스의 show 라우트에서 다음의 URI를 생성합니다:

```php
/user/{admin_user}
```

## 리소스 URI의 지역화(다국어 동사처리)
---
기본적으로 Route::resource 는 영어 동사형태로 된 리소스 URI를 구성합니다. 만약 create와 edit 액션 동사를 지역화 하고자 한다면, Route::resourceVerbs 메소드를 사용하면 됩니다. 이 작업은 AppServiceProvider 파일의 boot 메소드에서 수행해야 합니다:

```php
use Illuminate\Support\Facades\Route;

/**
 * Bootstrap any application services.
 *
 * @return void
 */
public function boot()
{
    Route::resourceVerbs([
        'create' => 'crear',
        'edit' => 'editar',
    ]);
}
```

액션 동사를 지역화되도록 설정하고 나면, Route::resource('fotos', 'PhotoController')와 같은 리소스 라우트는 다음의 URI를 구성하게 됩니다:

```php
/fotos/crear

/fotos/{foto}/editar
```

## Resource 컨트롤러 라우트에 추가하기
---
만약 리소스 컨트롤러에 추가적으로 라우팅을 구성해야할 필요가 있다면 Route::resource가 호출되기 전에 등록해야합니다. 그렇지 않으면 resource 메소드에 의해서 정의된 라우트들이 추가한 라우트들 보다 우선하게 되어 버립니다.

```php
Route::get('photos/popular', 'PhotoController@method');

Route::resource('photos', 'PhotoController');
```
{tip} Remember to keep your controllers focused. If you find yourself routinely needing methods outside of the typical set of resource actions, consider splitting your controller into two, smaller controllers.

{tip} 컨트롤러를 집중 관리하는 것을 잊지 마십시오. 일반적인 리소스 행동 세트 이외의 방법을 빈번하게 필요로하는 경우 컨트롤러를 두 개의 작은 컨트롤러로 분할하는 것을 고려하십시오.


