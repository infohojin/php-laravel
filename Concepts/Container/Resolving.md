---
layout: laravel
title: Laravel
subtitle: Concepts
cate:
    name: "서비스 컨테이너"
    link: "/Laravel/Concepts"

submenus:
    -
        name: 설계컨셉
        link: /Laravel/Concepts

    -
        name: 서비스 컨테이너
        link: /Laravel/Concepts/Container

    -
        name: 바인딩
        link: /Laravel/Concepts/Container/Binding
    -
        name: 의존성 해결
        link: /Laravel/Concepts/Container/Resolving

    -
        name: 이벤트
        link: /Laravel/Concepts/Container/Events
    -
        name: PSR-11
        link: /Laravel/Concepts/Container/PSR-11
---

## 의존성 해결
---

<br>

### make 메소드
---

컨테이너 밖에서 클래스 인스턴스에 대한 의존성을 해결하기 위해서 make 메소드를 사용할 수 있습니다. 
`make()` 메소드는 의존성 해결을 위하여 클래스나 인터페이스 이름을 전달 받습니다. 사용방법은 다음과 같습니다:

```php
$api = $this->app->make('HelpSpot\API');
```

만일 $app 변수에 접근권한이 없는 경우에는 별로도 제공되는 `resolve` 헬퍼 함수를 통하여 처리 할 수 있습니다. 다음은 렐퍼함수의 예 입니다.

```php
$api = resolve('HelpSpot\API');
```

위의 두가지 방법으로도 클래스의 의존성이 컨테이너를 통해서 해결될 수 없을때는, `makeWith` 메소드에 관련된 인자를 배열로 전달 할 수도 있습니다:

```php
$api = $this->app->makeWith('HelpSpot\API', ['id' => 1]);
```

<br>

### 자동 주입
---

앞서 이야기한 방법과 다르게, 그리고 가장 중요한 방법은 
컨트롤러, 이벤트 리스너, queue jobs, 미들웨어 그리고 다른 곳에서도 클래스의 생성자에 "타입-힌트" 를 선언함으로써 컨테이너가 의존성을 해결할 수 있도록 하는 것입니다. 
실제로는 이 방법이 개발에서 컨테이너에 의해서 객체의 의존성을 해결되어야 하는 데 가장 많이 사용되는 방법입니다.

예를 들어 컨트롤러의 생성자에서 타입 힌트로 지정된 Repository를 정의했다고 가정해 보겠습니다. 
해당 Repository는 자동으로 의존성이 해결되어 클래스에 주입될 것입니다.

```php
<?php

namespace App\Http\Controllers;

use App\Users\Repository as UserRepository;

class UserController extends Controller
{
    /**
     * The user repository instance.
     */
    protected $users;

    /**
     * Create a new controller instance.
     *
     * @param  UserRepository  $users
     * @return void
     */
    public function __construct(UserRepository $users)
    {
        $this->users = $users;
    }

    /**
     * Show the user with the given ID.
     *
     * @param  int  $id
     * @return Response
     */
    public function show($id)
    {
        //
    }
}
```


