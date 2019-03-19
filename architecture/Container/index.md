---
layout: laravel
title: Laravel
subtitle: Concepts
cate:
    name: "서비스 컨테이너"
    link: "/Laravel/Concepts"


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

<br><br><br>


## 바인딩
---

라라벨 컨테이너의 2번째 기능으로는 바인딩이 있습니다. 바인딩은 컨테이너에 클래스를 생성하여 저장하는 역할 을 합니다.

<br>

### 기본적인 바인딩
---
대부분의 컨테이버 바인딩 작업들은 서비스 프로바이터 내에서 등록처리 됩니다. 이러한 모든 예제들은 해당 컨텍스트에서 컨테이너를 사용하는 데모가 될것입니다.

{tip} 특정 인터페이스에 대한 의존성이 없을 때에는 컨테이너에 클래스를 바인딩 할 필요는 없습니다. 이러한 객체들은 리플랙션에 의해서 자동으로 의존성이 해결되기 때문에, 컨테이너가 각각의 객체들이 어떻게 생성될지 알 필요는 없습니다.

<br>

### 간단한 바인딩
---

서비스 프로바이더 클래스 안에서는 $this->app 형태로 컨테이너 인스턴스에 접근을 할 수 있습니다. 
또한 bind 메소드를 사용하여 클래스나 인터페이스 이름에 대한 의존성을 우리가 원하는 클래스의 인스턴스를 반환하는 Closure를 등록하여 바인딩 할 수 있습니다.

```php
$this->app->bind('HelpSpot\API', function ($app) {
    return new HelpSpot\API($app->make('HttpClient'));
});
```

바인딩 처리를 할때 의존성 주입을 위해서 클로저 함수의 인자로 컨테이너 자신을 전달 합니다. 이는 의존성을 주입하는데 매우 중요 합니다. 
이를 통해서 연결된 객체의 의존성 문제를 위해서 컨테이너 자신을 사용할 수 있습니다.

<br>

## 싱글톤으로 바인딩하기
---

싱글톤은 클래스의 인스턴스 생성을 제한하는 디자인 패턴중의 하나 입니다.
singleton 메소드 방식으로 클래스, 인터페이스를 바인딩 처리 하면 한번에 컨테이너는 해당 의존성 처리를 해결할 수 있습니다. 
의존성이 싱글톤 방식으로 바인딩으로 처리되면, 컨테이너는 다른 곳에서도 호출 될 경우에 동일한 객체 인스턴스가 반환될 것입니다.

```php
$this->app->singleton('HelpSpot\API', function ($app) {
    return new HelpSpot\API($app->make('HttpClient'));
});
```

<br>

## 인스턴스를 바인딩하기
---

새롭게 인스턴스를 생성하여 바인딩 하는것과 달리, 기존에 생성된 인스턴스를 컨테이너에 바인딩할 수도 있습니다. 이때 `instance` 메소드를 사용하면 됩니다.
이처럼 사용하고 있는 인스턴스를 컨테이너에 바인딩을 처리하면, 이후 다른 곳에서 컨테이너를 통하여 호출될때  동일한 인스턴스가 반환됩니다.

```php
$api = new HelpSpot\API(new HttpClient);

$this->app->instance('HelpSpot\API', $api);
```

<br>

### 기본 타입 바인딩
---

때로는, 클래스가 주입되는 클래스들을 받아 들일 수도 있지만, 정수형과 같은 기본타입의 값들을 주입할 필요가 있을 수도 있습니다. 
여러분은 손쉽게 문맥에 따라 조건적 바인딩을 통해서 클래스가 필요한 값을 주입할 수 있습니다:

```php
$this->app->when('App\Http\Controllers\UserController')
          ->needs('$variableName')
          ->give($value);
```

<br>

### 인터페이스에 구현객체 바인딩하기
---

서비스 컨테이너의 강력한 기능중 하나는 주어진 구현 객체에 인터페이스를 바인딩 할 수 있다는 것입니다. 

예를 들어 EventPusher 인터페이스와 RedisEventPusher 구현 클래스가 있다고 가정해 보겠습니다. 
이 인터페이스를 구현한 RedisEventPusher 객채를 구성한 뒤에 이 객체를 다음과 같이 서비스 컨테이너에 등록할 수 있습니다:

```php
$this->app->bind(
    'App\Contracts\EventPusher',
    'App\Services\RedisEventPusher'
);
```

이것은 구문은 EventPusher 인터페이스의 구현 객체가 필요할 때 컨테이너가 RedisEventPusher 을 주입해준다는 것을 말합니다. 
이제 우리는 EventPusher 인터페이스에 대한 타입을 생성자에 지정하면 어디에서라도 서비스 컨테이너가 의존성을 주입해줍니다.

```php
use App\Contracts\EventPusher;

/**
 * Create a new class instance.
 *
 * @param  EventPusher  $pusher
 * @return void
 */
public function __construct(EventPusher $pusher)
{
    $this->pusher = $pusher;
}
```
<br>

### 문맥에 따른 조건적 바인딩
---

때때로 동일한 인터페이스에 대한 2가지 구현 객체가 있고, 각각의 클래스마다 다른 구현 객체를 전달하고자 할 수도 있습니다. 
예를 들어 각각의 컨트롤러가 다른 Illuminate\Contracts\Filesystem\Filesystem contract 구현체에 의존한다면, 라라벨은 간단하고 유연한 인터페이스를 통해서 다음 행동을 정의합니다.

```php
use Illuminate\Support\Facades\Storage;
use App\Http\Controllers\PhotoController;
use App\Http\Controllers\VideoController;
use Illuminate\Contracts\Filesystem\Filesystem;

$this->app->when(PhotoController::class)
          ->needs(Filesystem::class)
          ->give(function () {
              return Storage::disk('local');
          });

$this->app->when(VideoController::class)
          ->needs(Filesystem::class)
          ->give(function () {
              return Storage::disk('s3');
          });
```

<br>

## 태깅
---

가끔은, 바인딩의 특정 "카테고리" 전체에 대한 의존성 해결을 해야 할 때도 있습니다. 
예를 들어, 서로 다른 Report 인터페이스의 구현 객체를 포함하는 배열을 전달 받는 보고서 수집기를 개발하고 있다고 해봅시다. 
Report 구현 객체를 등록한 뒤에, tag 메소드를 사용하여 태그를 달 수 있습니다:

```php
$this->app->bind('SpeedReport', function () {
    //
});

$this->app->bind('MemoryReport', function () {
    //
});

$this->app->tag(['SpeedReport', 'MemoryReport'], 'reports');
```

서비스에 태그가 붙으면 tagged 메소드를 사용하여 손쉽게 의존성을 해결할 수 있습니다.

```php
$this->app->bind('ReportAggregator', function ($app) {
    return new ReportAggregator($app->tagged('reports'));
});
```

<br>

## 바인딩 확장
---

extend 메서드로 서비스의 의존성을 수정할 수 있습니다. 예를 들어, 서비스의 의존성이 해결되었을때, 서비스를 꾸미거나(decorate) 혹은 설정하는 위한 추가 코드를 실행할 수 있습니다.

```php
$this->app->extend(Service::class, function($service) {
    return new DecoratedService($service);
});
```

<br><br><br>


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



<br><br><br>


## 컨테이너 이벤트
---

서비스 컨테이너는 객체의 의존성 해결을 수행할 때 마다 이벤트를 발생시킵니다. `resolving` 메소드를 사용하여 이 이벤트들에 대응할 수 있습니다.

```php
$this->app->resolving(function ($object, $app) {
    // Called when container resolves object of any type...
});

$this->app->resolving(HelpSpot\API::class, function ($api, $app) {
    // Called when container resolves objects of type "HelpSpot\API"...
});
```

위의 예제를 통하여 의존성이 해결된 객체가 콜백에 전달되어 질때 최종적으로 객체를 필요로 하는 대상에 전달하기 전에 추가적으로 객체의 속성을 설정 할 수 있습니다.


<br><br><br>


## PSR-11
---

라라벨의 서비스 컨테이너는 PSR-11 인터페이스를 따르고 있습니다. 따라서, PSR-11 인터페이스를 통하여  라라벨의 컨테이너에 접근이 가능합니다.

```php
use Psr\Container\ContainerInterface;

Route::get('/', function (ContainerInterface $container) {
    $service = $container->get('Service');

    //
});
```

{note} 만약 컨테이너에 명시적으로 선언되지 않은 서비스를 get 메서드로 호출 시 예외가 발생됩니다.


