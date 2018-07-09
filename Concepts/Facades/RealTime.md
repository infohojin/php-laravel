---
layout: laravel
title: Laravel
subtitle: Facades
cate:
    name: "컨셉"
    link: "/Laravel/Facades"
---

## 실시간 파사드
---
실시간 파사드를 사용하여 애플리케이션의 모든 클래스를 파사드처럼 취급 할 수 있습니다. 어떻게 이를 사용할 수 있는지 알기 위해서, 다음의 경우를 살펴보겠습니다. 예를 들어 Podcast 모델이 publish 메소드를 가지고 있다고 가정해보겠습니다. 이때, podcast를 publish 하기 위해서는 Publisher 인스턴스를 주입해야 합니다:

```php
<?php

namespace App;

use App\Contracts\Publisher;
use Illuminate\Database\Eloquent\Model;

class Podcast extends Model
{
    /**
     * Publish the podcast.
     *
     * @param  Publisher  $publisher
     * @return void
     */
    public function publish(Publisher $publisher)
    {
        $this->update(['publishing' => now()]);

        $publisher->publish($this);
    }
}
```

메소드에 publisher 구현체를 주입하면 주입된 publisher 는 mock 할 수 있기 때문에, 메소드를 분리시켜 테스트를 용이하게 합니다. 그렇지만, publish 메소드를 호출할 때마다 매번 publisher 인스턴스를 전달할 필요가 있습니다. 리얼타임 파사드를 사용하면, 동일한 테스트 유효성을 유지하면서도, 명시적으로 Publisher 인스턴스를 전달하지 않아도 됩니다. 리얼타임 파사드를 생성하기 위해서는 Import 한 클래스 이름 앞에 Facades 를 붙이면 됩니다:

```php
<?php

namespace App;

use Facades\App\Contracts\Publisher;
use Illuminate\Database\Eloquent\Model;

class Podcast extends Model
{
    /**
     * Publish the podcast.
     *
     * @return void
     */
    public function publish()
    {
        $this->update(['publishing' => now()]);

        Publisher::publish($this);
    }
}
```

리얼타임 파사드가 사용되면, publisher 구현체는 Facades 클래스네임 뒤에 정의된 인터페이스 또는 클래스 네임을 사용하여 서비스 컨테이너에서 의존성이 해결됩니다. 테스트를 진행할 때에는, 이 메소드 호출을 mock 하기 위해서 라라벨에 내장된 파사드 테스팅 헬퍼를 사용할 수 있습니다:

```php
<?php

namespace Tests\Feature;

use App\Podcast;
use Tests\TestCase;
use Facades\App\Contracts\Publisher;
use Illuminate\Foundation\Testing\RefreshDatabase;

class PodcastTest extends TestCase
{
    use RefreshDatabase;

    /**
     * A test example.
     *
     * @return void
     */
    public function test_podcast_can_be_published()
    {
        $podcast = factory(Podcast::class)->create();

        Publisher::shouldReceive('publish')->once()->with($podcast);

        $podcast->publish();
    }
}
```

