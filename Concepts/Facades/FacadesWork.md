---
layout: laravel
title: Laravel
subtitle: Facades
cate:
    name: "컨셉"
    link: "/Laravel/Facades"
---

## 파사드는 어떻게 동작하는가
---
라라벨 애플리케이션에서, 파사드는 컨테이너의 객체에 엑세스하는 방법을 제공하는 클래스라고 할 수 있습니다. 이 작업을 수행하는 주요 매커니즘이 파사드 클래스안에 있습니다. 라라벨의 파사드들과 여러분이 작성한 파사드들은 기본 Illuminate\Support\Facades\Facade 클래스를 상속받습니다.

Facade 기본 클래스는 __callStatic() 매직 매소드를 사용하여 여러분이 작성한 파사드에 대한 호출을 컨테이너에서 의존성이 해결된 객체로 전달합니다. 다음의 예제에서 라라벨의 캐시 시스템을 호출합니다. 이 코드를 보자면, 아마 Cache 클래스의 get static 메소드를 호출한다고 생각할 수 있습니다.

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\Cache;

class UserController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return Response
     */
    public function showProfile($id)
    {
        $user = Cache::get('user:'.$id);

        return view('profile', ['user' => $user]);
    }
}
```

파일의 상단에 Cache 파사드를 사용하고 있는 부분에 주목해 주십시오. 이 파사드는 Illuminate\Contracts\Cache\Factory 인터페이스의 구현체에 접속할 수 있는 프록시로 동작합니다. 파사드를 사용한 어떠한 호출이라도 라라벨의 캐시 서비스의 구현체에 전달됩니다.

실제로 Illuminate\Support\Facades\Cache를 살펴보면 get이라는 static 메소드는 찾을 수가 없습니다:

```php
class Cache extends Facade
{
    /**
     * Get the registered name of the component.
     *
     * @return string
     */
    protected static function getFacadeAccessor() { return 'cache'; }
}
```

대신에, Cache 파사드는 기본 Facade 클래스를 상속하고 getFacadeAccessor() 메소드를 정의하고 있습니다. 이 메소드의 역할이 서비스 컨테이너의 바인딩 이름을 반환한다는 것입니다. 사용자가 Cache 파사드의 어떤 스태틱 메소드를 참조하려고 할 때 라라벨은 [서비스 컨테이너]로 부터 cache 로 이름지어진 바인딩 객체를 찾아 메소드 호출을 요청할 것입니다(이 경우 get 메소드)


