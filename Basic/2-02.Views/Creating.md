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

## 뷰 생성하기
---

라라벨은 뷰의 화면을 블레이드 템플릿을 이용하며 처리를 합니다. 블레이드에 대한 보다 자세한 것은 `블레이드 템플릿을 작성하는 방법`에 대한 문서를 확인하십시오.

기본적으로 사용자 웹브라우저로 출력되는 뷰는 HTML로 구성이 되어 있습니다.

다음은 간단한 뷰파일의 내용입니다:

```php
<!-- View stored in resources/views/greeting.blade.php -->

<html>
    <body>
        <h1>Hello, {{ $name }}</h1>
    </body>
</html>
``` 

이렇게 작성한 뷰 파일은 확장자가 `.blade.php`로 저장을 합니다. 블레이드로 저장된 view파일은 라라벨의 뷰를 생성하고 호출하는 `view()` 헬퍼함수를 통하여 읽어 들일 수 있습니다.

다음은 라라벨 라우터에서 직접 뷰를 호출하는 내용입니다: 

```php
Route::get('/', function () {
    return view('greeting', ['name' => 'James']);
});
```

사용자가 루트`/`로 접속시 콜백함수를 처리합니다. 콜백함수는 다시 view 헬퍼 함수를 실행하고 반환값을 전달합니다.

view 헬퍼는 두개의 인자값을 전달 받습니다. 첫번째는 뷰 파일의 이름입니다. 확장자 `.blade.php`는 생략을 합니다. 이 뷰 파일들은 라라벨의 `resources/views` 디렉토리에 있는 파일의 이름이 됩니다.

두번째 인자는 뷰에서 사용되는 데이터 배열입니다. 이 예제에서는 뷰에서 블레이드 문법에서 처리할 name 변수를 전달하고 있습니다.

뷰의 파일은 `resources/views` 안에 또라른 서브 디렉토리를 생성하여 구성할 수도 있습니다. 이는 뷰의 파일이 많아지고, 계층형으로 관리하는데 매우 유용합니다. 서브디렉토리로 구성된 뷰 파일은 디렉토리 구분자 `/` 대신에 `.`을 사용합니다. 예를 들어 뷰파일이 resources/views/admin/profile.blade.php 처럼 저장이 되었 있다면 아랭허 같이 호출을 해야 합니다. 

```php
return view('admin.profile', $data);
```

<br>

## 뷰파일이 존재하는지 확인하기
---

뷰는 `resources/views` 안에 있는 뷰파일을 기반으로 화면을 구성합니다. 만일 뷰 파일이 없는 경우에는 파일읽기 오류로 정상적인 뷰 화면을 생성할 수 없습니다.
이런경우 뷰를 호출하기 전에 뷰 파일이 있는지를 확인할 수 있는 메서드를 제공합니다.

컨트롤러에서 뷰를 호출하기 전에 사용을 하면 매우 유용합니다. 개발자, 협력자와의 협업으로 인하여 발생될 수 있는 다양한 문제들을 미연에 방지할 수 있습니다.

```php
use Illuminate\Support\Facades\View;

if (View::exists('emails.customer')) {
    //
}
```

exist 메소드는 뷰 파일이 존재한다면 true 를 반환할 것입니다. exist는 View 의 파사드 기능 입니다. 

<br>


## 먼저 사용가능한 뷰 파일 사용하기
---

뷰 헬퍼함수를 호출시 인자값을 전달하지 않는 경우에는 다음과 같이 메소드 체인을 통하여 인자값을 추가 설정을 할 수 있습니다.

```php
return view()->first(['custom.admin', 'admin'], $data);
```

위의 예제는 `first()` 메소드는 두개의 뷰 파일을 지정한 배열로 인자값을 전달 합니다. 이중 첫번째 뷰를 사용하겠다는 의미 입니다. 
이는 애플리케이션 또는 패키지에서 뷰파일을 커스마이징 하거나, 덮어 쓸 필요가 있는 경우 유용합니다:


당연하게도, View 파사드 패턴을 이용하여 메소드를 호출할 수 있습니다:

```php
use Illuminate\Support\Facades\View;

return View::first(['custom.admin', 'admin'], $data);
```

