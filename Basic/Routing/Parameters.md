---
layout: laravel
title: Laravel
subtitle: 라우팅 파라미터
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

# 라우트 파라미터
---
라우트 파라미터는 라우트 URL 주소의 일부분을 파라미터 인자값으로 전달처리하는 방법을 말합니다. 이는 url 이 동적으로 처리를 요청하는데 매우 유용합니다.

<br>

## 필수 파라미터
필수 파라미터는 URI의 일부 세크먼트를 필수 파라미터로 설정을 하는 것입니다.

다음 예제와 같이 URL 에서 사용자의 ID를 확인하고자 하는 요청 입니다. 이때 `/user/`다음에 오는 구분자를 사용자 ID로 사용을 하겠다는 의미 입니다.
`{id}`로 설정된 치환키를 사용하면, 콜백함수의 `$id`로 인자값이 전달됩니다.
```php
Route::get('user/{id}', function ($id) {
    return 'User '.$id;
});
```

필수파라미터에서는 값을 같이 입력하지 않은 경우 오류를 발생합니다.

다음과 같이 여러개의 라우트 파라미터를 정의할 수도 있습니다:

```php
Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
    //
});
```

라우트 파라미터는 항상 "{}"(중괄호)로 쌓여져 있습니다.
- 문자를 포함하지 않은 알파벳 문자로 구성되어 있어야합니다. 
- 문자는 사용하기 보다는 대신 (_) 언어스코어를 사용하십시오. 

라우트 파라미터는 라우트 콜백 / 컨트롤러에 주입되는데 이때 사용되는 콜백 / 컨트롤러 인자에서 문제가 되지 않는 이름이어야 합니다.

<br>

## 선택적 파라미터
---
필수 파라미터는 URI와 파라미터 값을 같이 입력요청을 해야 합니다. 만일 URI가 파라미터 부분이 같지 입력이 되지 않는 경우 오류를 발생합니다.

하지만, 동작중에 보면 파라미터 값이 지정이 되지 않는 경우도 필요합니다. 이를 지원하기 위해서 선택적 파라미터를 설정할 수 있습니다.

파라미터 이름뒤에 ? 를 표시하면 됩니다. 라우트 파라미터와 일치하는 변수가 기본값을 가지는지 확인하십시오:

```php
Route::get('user/{name?}', function ($name = null) {
    return $name;
});

Route::get('user/{name?}', function ($name = 'John') {
    return $name;
});
```

<br>

## 정규표현식 제약
---
파라미터 입력을 받을때 특정한 포맷을 필터링하여 라우트를 받아 들일 수 있습니다. 파라미터 값이 정수 또는 문자열, 또다른 패턴등을 필터링 합니다.

라우트를 설정하고 `->where()` 메소드 체인을 연결하여 포맷을 설정할 수 있습니다. 포맷은 정규표현식 형태로 작성을 하여 입력하면 됩니다.

```php
Route::get('user/{name}', function ($name) {
    //
})->where('name', '[A-Za-z]+');

Route::get('user/{id}', function ($id) {
    //
})->where('id', '[0-9]+');

Route::get('user/{id}/{name}', function ($id, $name) {
    //
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
```
<br>

## 글로벌 제약
---
`->where()`를 통한 정규표현식 제약은 각각의 라우트에 대한 패턴을 필터링 하는 것입니다. 하지만, 파라미터값으로 사용하는 이름이 여러 곳에서 사용을 하고 있고, 이에 대한 패턴 필터링을 동일한 경우 이를 미리 정의하여 재사용할 수 있습니다.

이 설정 패턴들은 RouteServiceProvider의 boot 메소드 안에 작성을 합니다.

```php
/**
 * Define your route model bindings, pattern filters, etc.
 *
 * @return void
 */
public function boot()
{
    Route::pattern('id', '[0-9]+');

    parent::boot();
}
```

위와 같이 패턴을 한번 정의하면, 정의된 값이 전체 라우트에 자동으로 적용됩니다:

```php
Route::get('user/{id}', function ($id) {
    // Only executed if {id} is numeric...
});
```

별도의 개별 패턴 정의를 하지 않아도 됩니다.

<br>