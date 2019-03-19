---
layout: laravel
title: Laravel
subtitle: Contracts
cate:
    name: "컨셉"
    link: "/Laravel/Contracts"
---

### Contracts 사용 시기
---

다른 곳에서 논의 된 것처럼, contract나 facade를 사용하기로 한 결정의 대부분은 개인적인 취향과 개발 팀의 취향에 달려 있습니다. contract와 facades 모두 강력하고 잘 테스트 된 Laravel 응용 프로그램을 작성하는 데 사용할 수 있습니다. 클래스가 제 역할을 하는데에 contract와 facades를 사용하는 데는 실제적인 차이점이 거의 없습니다.

그러나 계약(contracts)과 관련하여 몇 가지 의문을 떠올릴 수도 있습니다. 예를 들어, 인터페이스를 사용하는 이유는 무엇인가? 인터페이스를 더 복잡하게 사용하고 있지는 않은가? 입니다. 다음의 느슨한 결합 및 단순성 부분에서 인터페이스를 사용하는 이유를 설명합니다.

<br>

## 느슨한 결합
---

우선, 한 캐시 구현체에 강하게 결합돼 있는 코드를 살펴봅시다.

```php
<?php

namespace App\Orders;

class Repository
{
    /**
     * The cache instance.
     */
    protected $cache;

    /**
     * Create a new repository instance.
     *
     * @param  \SomePackage\Cache\Memcached  $cache
     * @return void
     */
    public function __construct(\SomePackage\Cache\Memcached $cache)
    {
        $this->cache = $cache;
    }

    /**
     * Retrieve an Order by ID.
     *
     * @param  int  $id
     * @return Order
     */
    public function find($id)
    {
        if ($this->cache->has($id))    {
            //
        }
    }
}
```

이 클래스의 코드는 주어진 캐시 구현체와 밀접하게 결합돼 있습니다. 특정 패키지 벤더의 캐시 구상클래스에 의존하기 때문에 이 코드는 캐시 클래스와 밀접하게 결합돼 있는 것입니다. 만약 이 패키지의 API가 변경되면 예시로든 이 코드 또한 변경되어야 합니다.

또한, 코드가 사용하는 캐시(Memcached)를 다른 것(Redis)으로 변경하고자 하는 경우, 역시나 Repository 클래스를 다시 수정해야만 할 것입니다. 저장소 클래스는 누가 어떻게 데이터를 제공하는지에 대한 정보를 너무 많이 가지고 있어서는 안 됩니다.

이렇게 접근하는 대신, 특정 벤더에 구속되지 않고 단순한 인터페이스에 의존하도록 하여 코드를 개선할 수 있습니다:

```php
<?php

namespace App\Orders;

use Illuminate\Contracts\Cache\Repository as Cache;

class Repository
{
    /**
     * The cache instance.
     */
    protected $cache;

    /**
     * Create a new repository instance.
     *
     * @param  Cache  $cache
     * @return void
     */
    public function __construct(Cache $cache)
    {
        $this->cache = $cache;
    }
}
```

이제 코드는 어떤 특정 벤더, 심지어 라라벨과도 결합되지 않습니다. Contract는 구현체를 가지지 않고, 의존성도 없기 때문에, 주어진 Contract의 다른 구현체를 쉽게 작성할 수 있습니다. 캐시를 사용하는 코드를 수정하지 않고도 캐시 구현체를 대체할 수 있습니다.

<br>

### 단순함
---

라라벨의 모든 서비스들이 단순한 인터페이스로 보기좋게 정의돼 있기 때문에, 그 서비스들에 의해 제공되는 기능을 알아내는 것이 매우 쉽습니다. Contract들이 프레임워크의 기능들에 대한 간결한 도큐먼트의 역할을 하는 것입니다.

또한, 여러분이 간단한 인터페이스에 의존하게 되면, 여러분의 코드는 이해하거나 유지보수하기가 더 쉬워집니다. 크고 복잡한 클래스에서 사용할 수 있는 메소드들을 훑어보는 대신, 단순하고 깨끗한 인터페이스를 참고할 수 있습니다.


