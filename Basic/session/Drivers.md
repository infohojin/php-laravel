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

## 사용자 정의 세션 드라이버 추가하기
---

## 드라이버 구현하기
여러분의 사용자 정의 세션 드라이버는 SessionHandlerInterface를 구현해야 합니다. 이 인터페이스는 구현해야할 필요가 있는 몇가지 간단한 메소드를 포함하고 있습니다. MongoDB를 사용하는 구현체라면 다음과 같이 될 것입니다:

```php
<?php

namespace App\Extensions;

class MongoSessionHandler implements \SessionHandlerInterface
{
    public function open($savePath, $sessionName) {}
    public function close() {}
    public function read($sessionId) {}
    public function write($sessionId, $data) {}
    public function destroy($sessionId) {}
    public function gc($lifetime) {}
}
```

{tip} 라라벨은 이러한 확장 기능을 담아둘 디렉토리를 제공하지는 않습니다. 원하는 곳 어디에든 자유롭게 구성할 수 있습니다. 이 예제에서는, MongoSessionHandler를 저장하기 위해서 Extensions 디렉토리를 만들었습니다.

이 메소드들의 목적을 쉽게 이해하기 어렵기 때문에, 각각의 메소드를 빠르게 살펴보겠습니다:

open 메소드는 일반적으로 파일 기반의 세션 저장 시스템에서 사용됩니다. 라라벨은 file 세션 드라이버를 제공하고 있기 때문에, 여러분은 거의 해당 메소드에 추가할 것이 없습니다. 이 메소드는 비어 있는 형태로 구성해도 됩니다. 이것은 좋지않은 인터페이스 디자인의 경우가 됩니다만 (나중에 설명합니다), PHP가 이 메소드를 구현하게끔 요구하고 있습니다.
close 메소드역시 open 메소드와 마찬가지로 무시할 수 있습니다. 대부분의 드라이버에서는 필요가 없습니다.
read 메소드는 주어진 $sessionId 에 해당하는 문자열의 세션 데이터의 문자열 버전을 반환해야합니다. 라라벨이 시리얼라이즈(직렬화)를 수행해주기 때문에 여러분이 작성한 드라이버에서 세션 데이터를 탐색하거나 저장하는데 시리얼라이즈나 다른 인코딩을 처리할 필요는 없습니다.
write 메소드는 $sessionId 에 해당하는 $data 문자열을 MongoDB, Dynamo 등과 같은 시스템에 저장해야 합니다. 다시 말하지만, 라라벨이 이미 처리하기 때문에, 여러분은 어떠한 시리얼라이제이션-직렬화도 수행하지 말아야 합니다.
destory 메소드는 저장소에서 주어진 $sessionId 에 해당하는 데이터를 삭제해야 합니다.
gc 메소드는 UNIX 타임스탬프로 주어진 $lifetime 보다 오래된 모든 세션 데이터들을 제거해야합니다. Memcached와 Redis처럼 스스로 오래된 데이터를 삭제하는 시스템에서는, 이 메소드는 비워 둡니다.

## 드라이버 등록하기
---

드라이버를 구현하였다면, 프레임워크에 등록할 준비가 되었습니다. 라라벨의 세션 벡엔드에 추가적인 드라이버를 추가하려면 Session 파사드의 extens 메소드를 사용할 수 있습니다. 서비스 프로바이더의 boot 메소드에서 extend 메소드를 호출해야 합니다. 이미 존재하는 AppServiceProvider 또는 완전히 새로운 프로바이더를 생성하여 수행할 수 있습니다:

```php
<?php

namespace App\Providers;

use App\Extensions\MongoSessionHandler;
use Illuminate\Support\Facades\Session;
use Illuminate\Support\ServiceProvider;

class SessionServiceProvider extends ServiceProvider
{
    /**
     * Perform post-registration booting of services.
     *
     * @return void
     */
    public function boot()
    {
        Session::extend('mongo', function ($app) {
            // Return implementation of SessionHandlerInterface...
            return new MongoSessionHandler;
        });
    }

    /**
     * Register bindings in the container.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}
```

세션 드라이버를 등록했으면 mongo 드라이버를 config/session.php 설정 파일에서 사용할 수 있습니다.