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

## 라우트 모델 바인딩
---
모델 ID 를 라우트나 컨트롤러 액션에 주입한 경우, 여러분은 곧잘 ID와 일치하는 모델을 조회하기 위해서 쿼리를 수행할 것입니다. 라라벨 라우트 모델 바인딩은 여러분의 라우트에 자동으로 모델 인스턴스를 직접 주입하는 편리한 방법을 제공합니다. 예를 들어 사용자의 ID를 주입하는 대신 주어진 ID와 매칭되는 전체 User 모델 인스턴스를 주입할 수 있습니다.


## 묵시적 바인딩
---
라라벨은 자동으로 라우트나 컨트롤러 액션에서 정의되어 있는 라우트 세그먼트 이름과 일치하는 타입힌트된 변수에 해당하는 Eloquent 모델을 의존성 해결합니다. 예를 들어:

```php
Route::get('api/users/{user}', function (App\User $user) {
    return $user->email;
});
```

App\User Eloquent 모델로 타입힌트된 `$user 변수와 {user} 세그먼트가 일치하기 때문에, 라라벨은 자동으로 request URI 로 부터 일치하는 ID 값을 가진 모델 인스턴스를 주입할것입니다. 만약 데이터베이스에서 매칭되는 모델 인스턴스를 찾을 수 없으면, 자동으로 404 HTTP response 생성됩니다.

## 키의 이름을 변경하기
---

주어진 모델을 클래스를 찾을 때 id 와는 다른 데이터베이스 컬럼을 사용하는 모델 바인딩을 하고자 한다면, Eloquent 모델의 getRouteKeyName 메소드를 재지정하면 됩니다:

```php
/**
 * Get the route key for the model.
 *
 * @return string
 */
public function getRouteKeyName()
{
    return 'slug';
}
```

## 명시적 바인딩
---
명시적 바인딩을 등록하기 위해서, 주어진 파라미터에 대한 클래스를 지정하려면 라우터의 model 메소드를 사용하십시오. 여러분은 RouteServiceProvider 클래스의 boot 메소드 안에서 명시적 모델 바인딩을 정의해야 합니다:

```php
public function boot()
{
    parent::boot();

    Route::model('user', App\User::class);
}
```

다음으로, {user} 파라미터를 포함한 라우트를 정의합니다:

```php
Route::get('profile/{user}', function (App\User $user) {
    //
});
```

{user} 파라미터와 App\User 모델이 바인딩되어 있기 때문에 라우트에는 User 인스턴스가 주입 될것입니다. 예를 들어 profile/1으로 요청이 들어오면 데이터베이스로 부터 ID가 1인 User의 인스턴스가 주입됩니다.

만약 데이터베이스에서 일치하는 모델 인스턴스를 찾지 못하는 경우, 404 응답이 자동으로 생성될 것입니다.

## 의존성 해결 로직 커스터마이징하기
---

만약 여러분의 고유한 의존성 해결 로직을 사용하려면 Route::bind 메소드를 사용할 수 있습니다. bind 메소드에 전달되는 클로저에는 URI 세그먼트에 해당하는 값이 전달되고 라우트에 주입되어야 하는 클래스의 인스턴스를 반환해야 합니다:

```php
public function boot()
{
    parent::boot();

    Route::bind('user', function ($value) {
        return App\User::where('name', $value)->first() ?? abort(404);
    });
}
```
