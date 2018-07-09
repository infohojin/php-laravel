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

## Rate 제한
---
사용자가 url 라우트를 통하여 접속시 일부 접속을 제한할 수 있는 기능을 미들웨어로 제공합니다. 이 기능을 사용하기 위해서는 throttle 미들웨어를 사용하면 됩니다.
throttle 미들웨어를 라우트 또는 라우트 그룹으로 지정해야만 합니다. 

throttle 미들웨어는 2개의 파라메터를 받습니다. 개발자가 지정한 분 동안에 받아들일 수 있는 최대 리퀘스트 수를 정할 수 있습니다. 

```php
Route::middleware('auth:api', 'throttle:60,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});
```

예를 들어, 인증된 유저가 아래의 라우트 그룹에 1분 당 60번까지 접속을 제한할 수 습니다. 
<br><br>

## 동적인 Rate 제한
---
인증된 User 에 대해서 attribute 를 기반으로 동적으로 요청되는 Request 최대치를 정할 수 있습니다. 

```php
Route::middleware('auth:api', 'throttle:rate_limit,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});
```

예를 들어, User 모델이 rate_limit 라는 attribute 를 가지고 있다고 할 때, 최대 request-요청 수를 계산하기 위해 attribute 의 이름을 throttle 미들웨어에 전달 할 수 있습니다.