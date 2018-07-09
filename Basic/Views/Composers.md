---
layout: laravel
title: Laravel
subtitle: View
submenus:
    -
        name: 뷰-Views
        link: /Laravel/Basic/Views
    -
        name: 뷰 생성하기
        link: /Laravel/Basic/Views/Creating
    -
        name: 데이터 전달하기
        link: /Laravel/Basic/Views/DataViews
    -
        name: 뷰 컴포저
        link: /Laravel/Basic/Views/Composers
---

## 뷰 컴포저

뷰 컴포저는 뷰가 렌더링 될 때 호출되는 콜백 또는 클래스 메소드입니다. 

서비스 프로바디이더의 `boot`메소드를 통하여 모든 뷰에 전달되는 내부값을 정의할 수 있었습니다. 또는 뷰가 블레이드 템플릿 언어를 통하여 화면을 직접 렌더링 될 때마다 생성된 데이터가 존재하고, 이를 뷰에게 다시 전달해야 하는 데이터도 있을 수 있습니다. 뷰 컴포저는 이러한 작업들을 한곳에서 로직을 작성하여 관리할 수 있도록 도와 줍니다.

예를 들어 뷰 컴포저를 서비스 프로바이더에 구성해 봅시다. `Illuminate\Contracts\View\Factory` contract 구현체에 엑세스 하기 위해서 View 파사드를 사용할 것입니다. 

여기서 우리가 좀더 알아야 할 것은 라라벨이 뷰 컴포저를 위한 어떠한 기본적인 디렉토리도 포함하고 있지 않다는 것입니다. 이 의미는 개발자가 직접 자유롭게 뷰 컴포저를 구성할 수 있습니다. 

예를 들어 `app/Http/ViewComposers` 디렉토리를 새롭게 생성할 수 있습니다.

```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\View;
use Illuminate\Support\ServiceProvider;

class ComposerServiceProvider extends ServiceProvider
{
    /**
     * Register bindings in the container.
     *
     * @return void
     */
    public function boot()
    {
        // Using class based composers...
        View::composer(
            'profile', 'App\Http\ViewComposers\ProfileComposer'
        );

        // Using Closure based composers...
        View::composer('dashboard', function ($view) {
            //
        });
    }

    /**
     * Register the service provider.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}
```

{note} 만일 여러분이 뷰 컴포저를 등록하기 위한 새로운 서비스 프로바이더를 생성후에는 설정파일을 수정해 주어야만 합니다. config/app.php의 설정 파일에 있는 providers 배열값에 이 서비스 프로바이더를 추가해 주어야 합니다.

이제 뷰 컴포저를 생성 등록했다면 `profile` 화면 뷰를 렌더링 할때마다 `ProfileComposer@compose` 메소드를 추가로 실행하게 됩니다.


그럼 `ViewComposers`클래스를 만들어 보도록 합니다.

```php
<?php

namespace App\Http\ViewComposers;

use Illuminate\View\View;
use App\Repositories\UserRepository;

class ProfileComposer
{
    /**
     * The user repository implementation.
     *
     * @var UserRepository
     */
    protected $users;

    /**
     * Create a new profile composer.
     *
     * @param  UserRepository  $users
     * @return void
     */
    public function __construct(UserRepository $users)
    {
        // Dependencies automatically resolved by service container...
        $this->users = $users;
    }

    /**
     * Bind data to the view.
     *
     * @param  View  $view
     * @return void
     */
    public function compose(View $view)
    {
        $view->with('count', $this->users->count());
    }
}
```

위의 코드를 보면 `compose` 메소드가 새롭게 추가 되었습니다. 화면 뷰가 렌더링 하기 전에 `ViewComposers`의 compose 메소드가 Illuminate\View\View 인스턴스와 함께 호출하게 됩니다.

컴포저 메소드는 `with` 메소드를 통하여 데이터 값을 추가로 설정을 합니다.

{tip} 모든 뷰 컴포저의 의존성 주입은 service container 를 통해서 이루어 집니다. 그렇기 때문에 필요한 객체의 경우 뷰 컴포저의 생성자에서 타입힌트를 지정한 형태로 지정하면 됩니다.

<br>

## 컴포저를 다수의 뷰에 적용하기
---

이전과 비슷한 방법으로 컴포저 에게도 여러개의 뷰화면을 전달 할 수 있습니다. 첫번째 인자는 뷰파일 목록의 배열입니다. 

```php
View::composer(
    ['profile', 'dashboard'],
    'App\Http\ViewComposers\MyViewComposer'
);
```

직접적인 뷰파일 목록을 지정해 주는 것 외에도 `*` 와일드 기호를 통하여 모든 뷰를 컴포저로 지정할 수도 있습니다.

```php
View::composer('*', function ($view) {
    //
});
```

<br>

## 뷰 크리에이터
---

뷰 크리에이터는 뷰 컴포저와 거의 비슷하게 동작합니다. 컴포저와의 차이점은 실행 시점입니다. 

뷰 크리에이터는 뷰가 렌더링 되기를 기다리지 않고 인스턴스후 다음에 바로 실행됩니다. 

```php
View::creator('profile', 'App\Http\ViewCreators\ProfileCreator');
```

뷰 크리에이터를 등록하기 위해서는 `creator` 메소드를 사용하면 됩니다.

<br>