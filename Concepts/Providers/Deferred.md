## 지연(deferred) 프로바이더
만약 여러분의 프로바이더가 단지 서비스 컨테이너에 바인딩을 등록하기만 한다면, 등록된 바인딩이 실제로 필요할때까지 등록 자체를 지연(deferred) 시킬 수 있습니다. 이러한 프로바이더 로딩의 지연(deferred)은 모든 요청에 프로바이더를 파일 시스템에서 로드하지 않으므로 애플리케이션의 성능을 향상시킬 것입니다.

라라벨은 지연된 서비스 프로바이더가 제공하는 모든 서비스 목록과 해당 서비스 프로바이더 클래스 이름을 컴파일하고 저장합니다. 그런 다음 이러한 서비스들 중 하나를 의존성 해결하려고 할 때에만, 라라벨이 서비스 프로바이더를 로드합니다.

프로바이더를 지연(defer) 로딩 하려면 프로바이더의 defer 프로퍼티를 true로 설정하고 provides 메소드를 정의하면 됩니다. provides 메소드는 프로바이더에 의해서 바인딩이 등록된 서비스 컨테이너를 리턴해야 합니다:

```php
<?php

namespace App\Providers;

use Riak\Connection;
use Illuminate\Support\ServiceProvider;

class RiakServiceProvider extends ServiceProvider
{
    /**
     * Indicates if loading of the provider is deferred.
     *
     * @var bool
     */
    protected $defer = true;

    /**
     * Register the service provider.
     *
     * @return void
     */
    public function register()
    {
        $this->app->singleton(Connection::class, function ($app) {
            return new Connection($app['config']['riak']);
        });
    }

    /**
     * Get the services provided by the provider.
     *
     * @return array
     */
    public function provides()
    {
        return [Connection::class];
    }

}
```
