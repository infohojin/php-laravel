---
layout: laravel
title: Laravel
subtitle: Contracts
cate:
    name: "컨셉"
    link: "/Laravel/Contracts"
---

### Contract 레퍼런스
그럼 어떻게 Contract의 구현체를 얻을 수 있을까요? 사실 매우 간단합니다.

라라벨에 있는 여러 종류의 클래스들은 컨트롤러, 이벤트리스너, 미들웨어, 큐 작업, 라우트 클로저들을 관리하는 서비스 컨테이너를 통해 의존성 해결(resolve) 되고 있습니다. 따라서 의존성이 해결되는 어떤 클래스가 특정 Contract의 구현체를 얻으려면 그 클래스의 생성자에 그 인터페이스를 "type-hint"로 지정해놓으면 됩니다

그 예로 아래의 이벤트 리스너를 보겠습니다.

```php
<?php

namespace App\Listeners;

use App\User;
use App\Events\OrderWasPlaced;
use Illuminate\Contracts\Redis\Database;

class CacheOrderInformation
{
    /**
     * The Redis database implementation.
     */
    protected $redis;

    /**
     * Create a new event handler instance.
     *
     * @param  Database  $redis
     * @return void
     */
    public function __construct(Database $redis)
    {
        $this->redis = $redis;
    }

    /**
     * Handle the event.
     *
     * @param  OrderWasPlaced  $event
     * @return void
     */
    public function handle(OrderWasPlaced $event)
    {
        //
    }
}
```

이벤트리스너가 의존성 해결될 때, 서비스 컨테이너는 클래스의 생성자에 있는 타입힌트를 읽고, 그에 적합한 값을 주입해 줍니다. 서비스 컨테이너에 무언가를 등록하는 것에 대하여 더 알고싶다면, 이 문서를 보시기 바랍니다.


