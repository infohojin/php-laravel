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

## 서비스 컨테이너
---

디자인 패턴중에서 어플리케이션 클래스의 인스턴스를 관리하는 패턴동작이 있습니다. `instance pool`은 클래스의 인스턴스를 배열로 저장후, 필요할 때마다 이를 재사용 하는 패턴 입니다.
라라벨은 이러한 인스턴스풀, 다른말로 `register`라고도 합니다. 이를 `컨테이너`라는 용어로 사용을 합니다.  

또한 이러한 클래스의 인스턴스를 관리하고, 상호 접근을 위하여 의존성을 주입합니다. 

라라벨의 내부 코드컨셉을 이해하기 위해서는 클래스의 의존성 및 주입에 대한 지식이 필요로 합니다. 그리고 라라벨은 클래스의 의존성을 관리하고, 의존성 주입을 위하여 컨테이너 개념을 사용합니다.

의존성 주입이란? 클래스간의 의존성은 클래스 생성될 때 또는 경우에 따라 "setter" 메소드에 의해서 "주입" 된다는 의미입니다.

간단한 예제코드를 통하여 자세히 알아 봅니다:

```php
<?php

namespace App\Http\Controllers;

use App\User;
use App\Repositories\UserRepository;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * The user repository implementation.
     *
     * @var UserRepository
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
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return Response
     */
    public function show($id)
    {
        $user = $this->users->find($id);

        return view('user.profile', ['user' => $user]);
    }
}
```

위의 예제 코드는 `UserController` 클래스의 소스 일부 입니다. `show()` 메소드는 DB에 있는 사용자 정보를 조회하는 코드들이 같이 들어가 있습니다.
즉 `show()` 메소드는 `user`클래스를 접근할 수 있는 인스턴스가 필요로 합니다.

`user` 인스턴스는 `UserRepository`타입으로 `Eloquent`를 통하여 데이터베이스에 접속하여 사용자 정보를 조회합니다. 이렇게 가능한 것은 처음 `__construct()` 생성자에서
인스턴스를 인자로 전달받아 내부 프로퍼티에 저장을 해두었기 때문입니다.

이렇게 전달받은 인스턴스 인자를 통하여 접근을 할 수 있도록 하는 것이 의존성 주입입니다.




