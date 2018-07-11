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

## 뷰에 데이터 전달하기
---

뷰를 생성 처리하는 `view` 핼퍼 함수는 2개의 매개변수를 전달 받습니다. 첫번째는 뷰파일명 이고, 두번째는 데이터 배열 입니다.
배열 데이터는 키/값 으로 구성된 연상배열 형태의 타입입니다. 
```php
return view('greetings', ['name' => 'Victoria']);
```

이렇게 전달받은 데이터는 뷰에서 화면을 구성하는 변수값으로 이용하게 됩니다. 

뷰 템플릿 파일 안에서 <?php echo $key; ?> 와 같이 각각의 키에 해당하는 값에 엑세스 하여 확인을 할 수 있습니다. 직접적인 인자를 통하여 view 헬퍼 함수에 데이터를 전달하는 대신에 with 메소드를 사용하여 개별적으로 데이터를 추가 할 수도 있습니다.

다음은 위의 예제를 with 메소드를 통하여 개별 선택추가하는 예 입니다:
```php
return view('greeting')->with('name', 'Victoria');
```

<br>

## 모든 뷰파일에서 데이터 공유하기
---

때로는 모든 뷰에서 애플리케이션에서 사용되는 변수를 접근하여 사용할 필요가 있을때도 있습니다. 이런경우 뷰 파사드의 share 메소드를 사용하면 됩니다. 

그러기 위해서는 이 코드를 서비스 프로바이더의 `boot` 메소드 안에 미리 작성해 놓으셔야 합니다. 

```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\View;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        View::share('key', 'value');
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

또는 AppServiceProvider에 이 코드를 구성해 놓거나, 다른 분리된 서비스 프로바이더를 생성할 수도 있습니다.
