---
layout: laravel
title: Laravel
subtitle: Facades
cate:
    name: "컨셉"
    link: "/Laravel/Facades"
---

## 언제 파사드를 사용하는가?
--- 

파사드는 다양한 장점이 있습니다. 파사드는 의존성으로 주입되거나 직접 설정해야 하는 긴 이름의 클래스들을 일일이 기억하지 않고서도 알 수 있는, 간결하고 기억하기 쉬운 문법을 제공합니다. 또한 PHP의 독특한 다이나믹 메소드의 사용 덕분에, 테스트도 쉬워집니다.

그러나 파사드를 사용하는데 몇 가지 주의해야 할 점도 존재합니다. 파사드의 가장 주요한 위험은 클래스의 책임 범위의 잘못된 사용입니다. 파사드는 사용하기가 매우 쉽고, 의존성 주입도 필요하지 않기 때문에, 하나의 클래스 파일 안에서 여러개의 파사드를 사용하는 코드가 자꾸 늘어나도록 클래스를 구성하기가 매우 쉽습니다. 의존성 주입을 사용하면 클래스가 커짐에 따라서 생성자가 비대해진다는 시각적인 피드백을 주게 되어 이러한 잠재적 위험이 억제됩니다. 따라서 파사드를 사용할 때에는, 클래스의 책임 범위를 작게 유지하도록 클래스의 사이즈를 구성하는데 특별한 주의가 필요합니다.

{tip} 라라벨과 연관된 써드파티 패키지를 구성할 때, 파사드 사용하는 대신에 라라벨 contracts를 주입하는 것이 더 나은 방법입니다. 패키지는 라라벨의 외부에서 구성되기 때문에, 라라벨의 파사드 테스팅 헬퍼에 엑세스할 필요는 없습니다.


## 파사드 Vs. 의존성 주입
의존성 주입의 주요한 장점중 하나는 주입된 클래스의 구현체를 변경할 수 있다는 특성입니다. 이는 테스팅을 수행하는 동안 모킹 객체과 스터브를 주입할 수 있게 하고, 다양한 메소드가 호출되는 것을 확인할 수 있게 하여 유용합니다.

일반적으로, 정적 클래스 메소드에 대해서는 모킹(mock) 이나 스터가 사용가능하지 않습니다. 하지만 파사드는 서비스 컨테이너에 의해서 의존성이 해결되는 클래스 객체의 프록시 메소드로 다이나믹 메소드를 사용하기 때문에, 실제 주입된 클래스 인스턴스를 테스트하는 것과 마찬가지로 파사드를 테스트할 수 있습니다. 예를 들어 다음의 주어진 라우트를 보겠습니다:

```php
use Illuminate\Support\Facades\Cache;

Route::get('/cache', function () {
    return Cache::get('key');
});
우리는 Cache::get 메소드가 원하는 인자와 함께 호출되었는지 확인하기 위해서 다음의 테스트를 작성할 수 있습니다.

use Illuminate\Support\Facades\Cache;

/**
 * A basic functional test example.
 *
 * @return void
 */
public function testBasicExample()
{
    Cache::shouldReceive('get')
         ->with('key')
         ->andReturn('value');

    $this->visit('/cache')
         ->see('value');
}
```

## 파사드 Vs. 헬퍼 함수
파사드 뿐만 아니라, 라라벨은 뷰 파일을 생성하거나, 이벤트를 발생시키거나, Job을 실행시키거나 또는 HTTP 응답을 반환하는등의 공통된 작업을 수행하는 다양한 "헬퍼" 함수를 포함하고 있습니다. 이러한 다수의 헬퍼 함수들은 파사드와 일치하는 동일한 동작들을 수행합니다. 예를 들어 다음의 파사드 호출과 헬퍼 함수 호출은 동일합니다:

```php
return View::make('profile');

return view('profile');
```

이 파사드와 헬퍼함수 사이에는 차이점이 전혀 없습니다. 헬퍼함수를 사용하는 경우에도 일치하는 파사드와 같이 동일하게 테스트 할 수 있습니다. 예를 들어 다음의 주어진 라우트를 보겠습니다:

```php
Route::get('/cache', function () {
    return cache('key');
});
```

위의 코드에서 cache 헬퍼 함수는 Cache 파사드의 get 메소드를 호출합니다. 따라서 헬퍼 함수를 사용하더라도, 기대되는 인자와 함께 메소드가 호출되었는지 확인하기 위해서 다음의 테스트를 작성할 수 있습니다.

```php
use Illuminate\Support\Facades\Cache;

/**
 * A basic functional test example.
 *
 * @return void
 */
public function testBasicExample()
{
    Cache::shouldReceive('get')
         ->with('key')
         ->andReturn('value');

    $this->visit('/cache')
         ->see('value');
}
```

