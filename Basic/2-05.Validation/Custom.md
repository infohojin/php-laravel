---
layout: laravel
title: Laravel
subtitle: Basic
cate:
    name: "기본사항"
    link: "/Laravel/Basic"

submenus:
    -
        name: 기본사항
        link: /Laravel/Basic
    -
        name: 라우팅
        link: /Laravel/Basic/Routing/Basic
    -
        name: 미들웨어
        link: /Laravel/Basic/Middleware
    -
        name: CSRF 보호
        link: /Laravel/Basic/CSRF
    -
        name: 컨트롤러
        link: /Laravel/Basic/Controllers
    -
        name: Requests-요청
        link: /Laravel/Basic/Requests
    -
        name: Responses-응답
        link: /Laravel/Basic/Responses
    -
        name: Views-뷰
        link: /Laravel/Basic/Views
    -
        name: URL 생성
        link: /Laravel/Basic/url
    -
        name: 세션
        link: /Laravel/Basic/session
    -
        name: Validation-유효성검사
        link: /Laravel/Basic/Validation
    -
        name: 에러처리
        link: /Laravel/Basic/Error
    -
        name: 로깅
        link: /Laravel/Basic/Logging
---

## 사용자 정의 유효성 검사 규칙

### Rule 객체 사용하기
라라벨은 다양하고 유용한 유효성 검사 룰을 제공합니다; 하지만, 여러분은 여러분만의 유효성 검사 룰을 정의하길 바랄수도 있습니다. 커스텀 유효성 검사 룰을 등록하는 방법중 하나는 rule 객체를 사용하는 방법입니다. 새로운 rule 객체를 생성하기 위해서 make:rule 아티즌 명령어를 실행할 수 있습니다. 문자열이 대문자로 구성되었는지 확인하는 rule을 생성하기 위해서 명령어를 사용해보겠습니다. 라라벨은 app/Rules 디렉토리에 새로운 rule 객체를 생성합니다:

```php
php artisan make:rule Uppercase
```

rule 객체가 생성되고나면, 유효성 검사가 동작하는 방식을 정해야 합니다. rule 객체는 두개의 메소드 : passes 와 message 를 가지고 있습니다. passes 메소드는 속성 값과 이름을 전달받아, 속성 값이 유효한지 아닌지에 따라, true 또는 false 를 반환해야 합니다. message 메소드는 유효성 검사가 실패했을 때 사용하는 에러 메세지를 반환해야 합니다:

```php
<?php

namespace App\Rules;

use Illuminate\Contracts\Validation\Rule;

class Uppercase implements Rule
{
    /**
     * Determine if the validation rule passes.
     *
     * @param  string  $attribute
     * @param  mixed  $value
     * @return bool
     */
    public function passes($attribute, $value)
    {
        return strtoupper($value) === $value;
    }

    /**
     * Get the validation error message.
     *
     * @return string
     */
    public function message()
    {
        return 'The :attribute must be uppercase.';
    }
}
```

당연하게도, 여러분이 언어 파일로부터 변환된 에러 메세지를 전달해주고자 한다면, messgae 메소드 안에서 trans 헬퍼 함수를 호출할 수 있습니다:

```php
/**
 * Get the validation error message.
 *
 * @return string
 */
public function message()
{
    return trans('validation.uppercase');
}
```

rule 을 정의하고 나면, 다른 유효성 검사 rule 객체들과 함께, rule 객체의 인스턴스를 전달하여, validator 에 추가할 수 있습니다:

```php
use App\Rules\Uppercase;

$request->validate([
    'name' => ['required', 'string', new Uppercase],
]);
```


## 클로저 사용하기
---

만약 애플리케이션을 통틀어 사용자 정의 규칙의 기능을 한번만 필요로 하는 경우, rule 객체 대신 클로저를 사용할 수 있습니다. 클로저는 속성의 이름, 속성의 값, 그리고 만약 유효성 검사가 실패할 시 호출될 $fail 콜백을 받습니다.

```php
$validator = Validator::make($request->all(), [
    'title' => [
        'required',
        'max:255',
        function($attribute, $value, $fail) {
            if ($value === 'foo') {
                return $fail($attribute.' is invalid.');
            }
        },
    ],
]);
```

## 확장기능 사용하기
---

또다른 사용자 정의 유효성 검사 규칙을 등록하는 방법중 하나는 Validator 파사드에 extend 메소드를 사용하는 것입니다. 이 메소드를 서비스 프로바이더 내에서 사용하여 사용자 정의 유효성 검사 규칙을 등록합니다:

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Validator;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        Validator::extend('foo', function ($attribute, $value, $parameters, $validator) {
            return $value == 'foo';
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

커스텀 유효성 검사 클로저는 4개의 인자를 받습니다: 유효성 검사를 할 필드($attribute)의 이름, 필드의 값($value), 그리고 룰에 전달될 파라미터들($parameters)의 배열, 그리고 Validator 인스턴스 입니다.

또한 클로저 대신 클래스명과 메소드명을 extend 메소드로 전달할 수도 있습니다:

```php
Validator::extend('foo', 'FooValidator@validate');
```

## 에러 메세지 정의하기
---
사용자 정의(커스텀) 규칙을 위한 에러 메세지 또한 정의해야 합니다. 인라인 사용자 정의(커스텀) 메세지 배열을 사용하거나 유효 검사 언어 파일에 엔트리를 추가하면 됩니다. 이 메세지는 custom 배열 안이 아닌 배열의 첫번째 레벨에 놓여야 합니다. custom 배열은 특정 속성에 따른 에러 메세지를 담당합니다:

```php
"foo" => "Your input was invalid!",

"accepted" => "The :attribute must be accepted.",

// The rest of the validation error messages...
```

커스텀 유효 검사 룰을 생성할 때 종종 에러 메세지를 위한 커스텀 플레이스 홀더 대체제를 정의해야 할 수도 있습니다. 이전의 설명에 따라 커스텀 Validator를 생성하고 Validator 파사드에 replacer 메소드를 호출하십시오. 이는 서비스 프로바이더의 boot 메소드 안에서 할 수 있습니다:

```php
/**
 * Bootstrap any application services.
 *
 * @return void
 */
public function boot()
{
    Validator::extend(...);

    Validator::replacer('foo', function ($message, $attribute, $rule, $parameters) {
        return str_replace(...);
    });
}
```

## 묵시적 확장
---

기본적으로 유효성 검사를 받는 속성이 존재하지 않거나 required 규칙의 정의에 따라 빈 값을 가지고 있다면, 사용자 정의(커스텀) 확장을 포함한 정상적인 유효성 검사 규칙은 실행되지 않을 것입니다. 예를 들어 null 값에는 unique 룰이 실행되지 않을 것입니다:

```php
$rules = ['name' => 'unique'];

$input = ['name' => null];

Validator::make($input, $rules)->passes(); // true
```

속성이 비었을 때도 규칙이 실행되기 위해서는 규칙에 속성이 필요하다는 것이 내포되어 있어야 합니다. 이런 "묵시적"인 확장을 만드려면 Validator::extendImplicit() 메소드를 사용하세요:

```php
Validator::extendImplicit('foo', function ($attribute, $value, $parameters, $validator) {
    return $value == 'foo';
});
```

{note} "묵시적" 확장은 단지 속성이 필요하다는 것을 _암시(내포)_합니다. 없거나 빈 속성의 유효성을 실제로 부정하는지는 여러분이 결정합니다.