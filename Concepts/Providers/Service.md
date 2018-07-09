## 서비스 프로바이더 작성하기
---

모든 서비스 프로바이더는 Illuminate\Support\ServiceProvider 클래스를 상속받습니다. 대부분의 서비스 프로바이더는 register 와 boot 메소드를 가지고 있습니다. register 메소드 안에서는 서비스 컨테이너에 등록만을 하도록 합니다. register 메소드안에서는 다른 이벤트 리스너나 라우트 또는 기타 기능의 일부등을 등록하지 말아야 합니다.

아티즌 CLI에서는 make:provider 명령어를 통해서 새로운 프로바이더를 생성할 수 있습니다:

```php
php artisan make:provider RiakServiceProvider
```

### Register 메소드
---

이미 설명한 바와 같이, register 메소드 안에서는, 서비스 컨테이너에 무언가를 바인딩 하는 작업만을 수행 해야 합니다. 여러분은 register 메소드 안에서 어떠한 이벤트 리스너, 라우트, 또는 다른 기능들에 대한 코드를 넣어서는 절대 안됩니다. 그렇지 않으면, 서비스 프로바이더가 아직 로드 하지 않은 서비스를 의도치 않게 사용해 버리고 말것입니다.

기본적인 서비스 프로바이더를 살펴보겠습니다. 서비스 프로바이더의 메소드 안에서 언제든지 $app 속성을 사용할 수 있으며, 이를 통해 서비스 컨테이너에 접근할 수 있습니다:

```php
<?php

namespace App\Providers;

use Riak\Connection;
use Illuminate\Support\ServiceProvider;

class RiakServiceProvider extends ServiceProvider
{
    /**
     * Register bindings in the container.
     *
     * @return void
     */
    public function register()
    {
        $this->app->singleton(Connection::class, function ($app) {
            return new Connection(config('riak'));
        });
    }
}
```

이 서비스 프로바이더는 register 메소드만 정의되어 있습니다. 그리고 이 메소드를 통해서 서비스 컨테이너에 Riak\Connection 구현 객체를 정의하고 있습니다. 서비스 컨테이너가 어떻게 작동하는지 이해하지 못하겠다면, 컨테이너 문서를 확인하십시오.

<br>

## bindings 과 singletons 속성
---

서비스 프로바이더가 동일한 (simple) 바인딩 여러개를 등록한다면, 각각의 컨테이너 바인딩을 일일이 등록하는 대신에 bindings 와 singletons 속성을 사용할 수 있습니다. 프레임워크에서 서비스 프로바이더가 로드 될 때, 이 속성들을 자동으로 체크하고 바인딩을 등록합니다:

```php
<?php

namespace App\Providers;

use App\Contracts\ServerProvider;
use App\Contracts\DowntimeNotifier;
use Illuminate\Support\ServiceProvider;
use App\Services\PingdomDowntimeNotifier;
use App\Services\DigitalOceanServerProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * All of the container bindings that should be registered.
     *
     * @var array
     */
    public $bindings = [
        ServerProvider::class => DigitalOceanServerProvider::class,
    ];

    /**
     * All of the container singletons that should be registered.
     *
     * @var array
     */
    public $singletons = [
        DowntimeNotifier::class => PingdomDowntimeNotifier::class,
    ];
}
```

<br>

## Boot 메소드
---

그럼 이제 서비스 프로바이더 안에서 뷰 컴포저를 등록할 필요가 있다면 어떻게 해야 할까요? 그런 작업은 boot 메소드 안에서 해야합니다. 이 메소드는 모든 다른 서비스 프로바이더들이 등록된 이후에 호출됩니다 즉, 프레임 워크에 의해 등록된 다른 모든 서비스들에 액세스 할 수 있다는 것을 의미합니다:

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;

class ComposerServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        view()->composer('view', function () {
            //
        });
    }
}
```

### Boot 메소드의 의존성 주입
---

여러분은 서비스 프로바이더의 boot 메소드에서 의존성 주입을 위해서 타입 힌트를 사용할 수 있습니다. 서비스 컨테이너는 자동으로 필요한 의존 객체를 주입할 것입니다:

```php
use Illuminate\Contracts\Routing\ResponseFactory;

public function boot(ResponseFactory $response)
{
    $response->macro('caps', function ($value) {
        //
    });
}
```

<br>