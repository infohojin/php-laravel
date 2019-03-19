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

## 에러 메시지 작업하기
---
Validator 인스턴스의 errors 메소드를 호출하면, 에러 메시지를 편하게 사용할 수 있는 다양한 메소드를 가진 MessageBag 인스턴스를 받을 수 있습니다, 이것은 오류메세지를 처리하기 위한 다양하고 편리한 메소드들을 가집니다. $errors 변수는 자동으로 모든 뷰에서 MessageBag 클래스 인스턴스로써 사용가능합니다.

하나의 필드에 대한 첫번째 에러 메시지 조회하기
특정 필드에 대한 첫번째 에러 메세지를 조회하려면 first 메소드를 사용하면 됩니다:

```php
$errors = $validator->errors();

echo $errors->first('email');
```

하나의 필드에 대한 모든 에러 메세지 조회하기
특정 필드에 대한 모든 메세지에 대한 배열을 조회하고자 한다면, get 메소드를 사용하면 됩니다:

```php
foreach ($errors->get('email') as $message) {
    //
}
```

배열형태의 form 필드에 대해서 유효성 검사를 하려면, 배열의 각각의 요소는 * 문자열을 사용하여 모든 메세지를 조회할 수 있습니다:

```php
foreach ($errors->get('attachments.*') as $message) {
    //
}
```

모든 필드에 대한 모든 에러 메세지 조회하기
모든 필드에 대한 모든 에러 메세지를 조회하기 위해서는 all 메소들 사용하면 됩니다:

```php
foreach ($errors->all() as $message) {
    //
}
```

하나의 필드에 대하여 에러 메시지가 존재하는지 검사하기
has 메소드는 특정 필드에 대한 에러 메세지가 존재하는지 확인하는데 사용합니다:

```php
if ($errors->has('email')) {
    //
}
```


## 사용자 지정(커스텀) 에러 메세지
---
필요하다면 기본적인 에러 메세지 대신에 커스텀 에러 메세지를 유효성 검사에 사용할 수 있습니다. 커스텀 메세지를 지정하는 데에는 여러가지 방법이 있습니다. 먼저 Validator::make 메소드에 커스텀 메세지를 세번째 인자로 전달할 수 있습니다:

```php
$messages = [
    'required' => 'The :attribute field is required.',
];

$validator = Validator::make($input, $rules, $messages);
```

다음의 예에서 :attribute 플레이스 홀더는 유효성 검사를 받는 필드의 실제 이름으로 대체됩니다. 유효성 검사 메세지에서 다른 플레이스 홀더들 또한 활용할 수 있습니다. 예를 들어:

```php
$messages = [
    'same'    => 'The :attribute and :other must match.',
    'size'    => 'The :attribute must be exactly :size.',
    'between' => 'The :attribute value :input is not between :min - :max.',
    'in'      => 'The :attribute must be one of the following types: :values',
];
```

## 주어진 속성에 대해 커스텀 메세지 지정하기
---
종종 하나의 특정 필드에 대해서만 커스텀 오류 메세지를 지정해야 하는 경우가 있습니다. 이것은 ".(점)" 표기법을 통해서 할 수 있습니다. 속성의 이름을 먼저 지정하고, 규칙을 명시하면됩니다:

```php
$messages = [
    'email.required' => 'We need to know your e-mail address!',
];
```

## 언어 파일에 커스텀 메세지 지정하기
---
대부분의 경우에서, Validator에 직접 메세지를 전달하는 대신, 언어 파일의 커스텀 메세지를 지정하기 원할 수 있습니다. 이렇게 하기 위해서는 resources/lang/xx/validation.php 언어 파일의 custom 배열에 메제지를 추가하면 됩니다.

```php
'custom' => [
    'email' => [
        'required' => 'We need to know your e-mail address!',
    ],
],
```

## 언어파일에 커스텀 속성 지정하기
---
유효성 검사 메세지의 :attribute 부분을 사용자 정의 속성 이름으로 교체하려면 resources/lang/xx/validation.php언어 파일의 attributes 배열에 사용자 정의 이름을 지정하면 됩니다:

```php
'attributes' => [
    'email' => 'email address',
],
```

