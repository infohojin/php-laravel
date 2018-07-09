---
layout: laravel
title: Laravel
subtitle: 미들웨어
cate:
    name: "미들웨어"
    link: "/Laravel/Basic"
submenus:
    -
        name: "미들웨어"
        link: "/Laravel/Basic/Middleware"
    -
        name: 정의
        link: "/Laravel/Basic/Middleware/Defining"
    -
        name: 등록하기
        link: "/Laravel/Basic/Middleware/Registering"
    -
        name: 파라미터
        link: "/Laravel/Basic/Middleware/Parameters"
    -
        name: 종료동작
        link: "/Laravel/Basic/Middleware/Terminable"
---

## 종료시 동작하는 미들웨어 (Terminable Middleware)
---
때로는 미들웨어는 HTTP response-응답이 준비된 이 후에 어떤 작업을 수행할 필요가 있을지도 모릅니다. 예를 들어, 라라벨에 내장된 "session" 미들웨어는 response-응답이 완전히 준비된 이후에 세션데이터를 저장소에 저장합니다. 만약 미들웨어에 terminate 메소드를 정의했다면, response-응답이 준비된 뒤에 자동으로 호출될 것입니다.

```php
<?php

namespace Illuminate\Session\Middleware;

use Closure;

class StartSession
{
    public function handle($request, Closure $next)
    {
        return $next($request);
    }

    public function terminate($request, $response)
    {
        // Store the session data...
    }
}
```

terminate 메소드는 Http 요청과 응답의 두가지를 전달 받는 구조여야 합니다. 종료시 동작하는 미들웨어를 정의하고나면, 이 미들웨어를 app/Http/Kernel.php 파일의 라우트 또는 글로벌 미들웨어 목록에 추가해 주어야 합니다.

미들웨어의 terminate 메소드가 호출될 때, 라라벨은 서비스 컨테이너에서 미들웨어의 새로운 인스턴스를 의존성 해결 하여 획득 할 것입니다. handle 과 terminate 메소드를 호출할 때, 동일한 미들웨어 인스턴스를 사용하려면 컨테이너의 singleton 메소드를 사용하여 미들웨어를 등록하십시오.