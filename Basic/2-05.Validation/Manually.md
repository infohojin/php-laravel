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

## Validator 수동으로 생성하기
---
request 의 validate 메소드를 사용하고 싶지 않다면, Validator 파사드를 사용하여 validator 인스턴스를 수동으로 생성할 수 있습니다. 파사드에 make 메소드를 사용하면 새로운 validator 인스턴스가 생성됩니다:

```php
<?php

namespace App\Http\Controllers;

use Validator;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class PostController extends Controller
{
    /**
     * Store a new blog post.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        $validator = Validator::make($request->all(), [
            'title' => 'required|unique:posts|max:255',
            'body' => 'required',
        ]);

        if ($validator->fails()) {
            return redirect('post/create')
                        ->withErrors($validator)
                        ->withInput();
        }

        // Store the blog post...
    }
}
```

make 메소드로 전달되는 첫번째 인자는 유효성 검사를 받을 데이터입니다. 두번째 인자는 데이터에 적용되어야 하는 유효성 검사 규칙들입니다.

request-요청이 유효성 검사에 실패하였는지 확인한 후에 withErrors 메소드로 세션에 에러 메세지를 임시저장-flash 할 수 있습니다. 이 메소드를 사용하면 리다이렉트 후에 $errors 변수가 자동으로 뷰에서 공유되어 손쉽게 사용자에게 보여질 수 있습니다. withErrors 메소드는 validator, MessageBag, 혹은 PHP array를 전달 받습니다.


## 자동으로 리다이렉트하기
---
수동으로 validator 인스턴스를 생성하더라도, request의 validate 메소드에 의해서 자동으로 리다이렉트 되는 이점을 유지하고 싶다면, validator 인스턴스에 validate 메소드를 호출하면 됩니다. 유효성 검사가 실패하는 경우, 사용자는 자동으로 리다이렉트 되거나, 또는 AJAX 요청인 경우, JSON이 반환됩니다:

```php
Validator::make($request->all(), [
    'title' => 'required|unique:posts|max:255',
    'body' => 'required',
])->validate();
```

## 이름이 지정된 Error Bags
---
한 페이지 안에서 여러개의 form을 가지고 있다면 에러들의 MessageBag에 이름을 붙여 지정한 form에 맞는 에러 메세지를 조회할 수 있도록 할 수 있습니다. withErrors에 이름을 두번째 인자로 전달하면 됩니다:

```php
return redirect('register')
            ->withErrors($validator, 'login');
```

그러면 $errors 변수에서 지정된 MessageBag 인스턴스에 접근할 수 있습니다:

```php
{{ $errors->login->first('email') }}
```

## 유효성 검사 이후에 후킹하기
---
유효성 검사가 완료된 후에 실행하고자 하는 콜백함수를 validator에 추가할 수 있습니다. 이를 통하면, 손쉽게 더 많은 유효성 검사를 실행할 수 있도록 하고, 에러 메시지 컬렉션에 에러 메시지를 더 추가할 수도 있습니다. 다음처럼 validator 인스턴스의 after 메소드를 사용하면 됩니다:

```php
$validator = Validator::make(...);

$validator->after(function ($validator) {
    if ($this->somethingElseIsInvalid()) {
        $validator->errors()->add('field', 'Something is wrong with this field!');
    }
});

if ($validator->fails()) {
    //
}
```

