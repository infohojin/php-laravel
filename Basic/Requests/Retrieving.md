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

## Request 입력값 조회하기
---

요청된 `request`의 값을 조회할 수 있습니다. 이와 관련된 몇개의 메소드를 제공합니다.

<br>

### 모든 입력값 조회하기
---

`all()` 메소드는 모든 입력데이터를 배열갑 형태로 조회할 수 있습니다.

```php
$input = $request->all();
```

<br>

## 입력값 조회하기
---

몇 개의 단순한 메소드들을 사용하면 어떤 HTTP verb 가 `request` 에 사용되었는지에 대한 걱정없이 `Illuminate\Http\Request` 인스턴스에서 모든 사용자 입력에 접근할 수 있습니다. HTTP verb에 관계없이 input 메서드는 사용자 입력을 조회하는데 사용됩니다:

```php
$name = $request->input('name');
```

input 메소드에 두번째 인자로 기본값을 전달할 수 있습니다. Request에 요청된 입력 값이 존재하지 않는다면 기본값이 반환됩니다:

```php
$name = $request->input('name', 'Sally');
```

배열 입력을 가진 폼에서 동작할 때에는, 배열에 접근하기 위하여 "점" 표기법을 사용할 수 있습니다:

```php
$name = $request->input('products.0.name');

$names = $request->input('products.*.name');
```

<br>

### 쿼리 스트링에서 입력값 조회하기
---

input 메소드가 요청-request 전체의 payload(쿼리 스트링을 포함하여)에서 값을 조회한다면, query 메소드는 쿼리 스트링에서만 값을 조회합니다:

```php
$name = $request->query('name');
```

찾고자 하는 데이터가 존재하지 않을 때를 위해서 메소드의 두번째 인자로 기본값을 전달할 수 있습니다:

```php
$name = $request->query('name', 'Helen');
```
query 메소드를 호출할 때 아무런 인자를 전달하지 않는다면, 전체 쿼리 스트링의 값들을 배열 형태로 반환합니다:

```php
$query = $request->query();
```

<br>

### 동적 속성을 통한 입력값 조회하기
---

`Illuminate\Http\Request` 인스턴스의 동적 속성을 사용하여 사용자 입력에 엑세스할 수도 있습니다. 
예를 들어 애플리케이션의 form 중에 하나가 name 필드를 가지고 있다면, 다음과 같이 필드의 값에 엑세스 할 수 있습니다:

```php
$name = $request->name;
```

동적 속성을 사용할 때, 라라벨은 먼저 request payload 안에 있는 파라미터의 값을 찾습니다. 만약 값이 없다면 라라벨은 라우트 파라미터 안에 있는 필드를 찾을 것입니다.

<br>

### JSON 입력 값 조회하기
---

애플리케이션에 JSON 요청이 전달되어 Content-Type 헤더 속성이 application/json 으로 지정되어 있다면 input 메소드를 통해서 JSON 데이터에 접근할 수 있습니다. 

또한 "점" 문법을 통해서 JSON 배열에 접근할 수도 있습니다.

```php
$name = $request->input('user.name');
```

<br>

###입력 데이터의 한 부분 조회하기
---

입력 데이터의 일부분만 조회하기 위해서 only와 except 메소드를 사용할 수 있습니다. 이 두 메소드 모두 하나의 배열 또는 동적인 인자의 목록을 받아 들입니다:

```php
$input = $request->only(['username', 'password']);

$input = $request->only('username', 'password');

$input = $request->except(['credit_card']);

$input = $request->except('credit_card');
```

{tip} only 메소드는 유입되는 request-요청에서 입력한 키 / 값 쌍을 반환합니다. 그렇지만 현재 request 에서 존재하지 않는 키/값은 반환하지 않습니다.

```php
$input = $request->intersect(['username', 'password']);
```

<br>

### 입력값이 존재하는지 확인하기
---

Request에 어떤 값이 존재하는지 확인하기 위해서 has 메소드를 사용해야 합니다. has 메소드는 현재 request 에서 값이 존재할 때 true를 반환합니다:

```php
if ($request->has('name')) {
    //
}
```

has 메소드에 배열이 주어지면, 지정된 모든 값이 존재하는지 확인하게 됩니다:

```php
if ($request->has(['name', 'email'])) {
    //
}
```

주어진 변수값이 현재 request 에 존재하고 비어 있지 않은 것을 확인하려면 filled 메소드를 사용하면 됩니다:

```php
if ($request->filled('name')) {
    //
}
```

<br>

## 이전 입력값 확인하기
---

라라벨은 한 request의 입력을 다음 request 중에도 유지할 수 있도록 해줍니다. 이 기능은 특히 유효성 검사 오류를 감지한 후 폼을 다시 채워 넣을 때 유용합니다. 하지만 라라벨에 포함된 유효성 검사 기능를 이용한다면 몇몇 유효성 검사 기능들이 자동으로 이 기능을 호출하기 때문에 수동으로 이 메소드들을 사용해야 할 가능성은 낮습니다.

입력값을 세션에 임시 저장하기
Illuminate\Http\Request 클래스의 flash 메소드는 현재의 입력들을 세션에 저장하여, 사용자의 다음 request 동안에도 값을 사용할 수 있게 해줍니다:

```php
$request->flash();
```

flashOnly와 flashExcept 메소드를 이용하여 request 데이터의 일부분을 세션에 임시 저장(flash)할 수 있습니다. 이 메소드들은 패스워드와 같은 민감한 정보들을 세션에 포함하지 않도록 하는데 유용합니다:

```php
$request->flashOnly(['username', 'email']);

$request->flashExcept('password');
```

<br>

## 입력값을 임시저장한 후 리다이렉트하기
---

대부분 입력값을 세션에 임시 저장 하고 이전 페이지로 리다이렉트 하기를 원하기 때문에, 이를 위해 리다이렉트와 함께 입력값 임시 저장을 메소드 체이닝으로 사용할 수 있습니다.

```php
return redirect('form')->withInput();

return redirect('form')->withInput(
    $request->except('password')
);
```

<br>

## 이전 입력값 조회하기
---

이전 request 에서 저장된 입력값을 조회하기 위해서는 Request 인스턴스의 old 메소드를 사용하면 됩니다. old 메소드는 세션에 저장된 입력 데이터를 꺼낼 것입니다:

```php
$username = $request->old('username');
```

라라벨은 글로벌 old 헬퍼 함수도 제공합니다. 블레이드 템플릿 안에서 지난 입력값을 보여주려면 old 헬퍼 함수를 사용하는 것이 보다 편리합니다. 주어진 필드에 대한 이전 입력값이 존재하지 않는다면, null 이 반환될 것입니다:

```php
<input type="text" name="username" value="{{ old('username') }}">
```

<br>




## 쿠키
---

### Request 에서 쿠키 조회하기

라라벨 프레임워크에서 생성된 모든 쿠키는 인증 코드와 함께 암호화 됩니다. 
이 것은 클라이언트가 변경되었을 때는, 쿠키가 유효하지 않다는 것을 의미합니다. 
Request에서 쿠키 값을 가져오기 위해서는 Illuminate\Http\Request 인스턴스에서 cookie 메소드를 사용하십시오:

```php
$value = $request->cookie('name');
```

또한, 쿠키에 엑세스 하기 위해서 Cookie 파사드를 사용할 수 있습니다:

```php
$value = Cookie::get('name');
```

<br>

## 쿠키를 Response 에 추가하기
---
외부로 나가는 Illuminate\Http\Response인스턴스에 cookie 메소드를 사용하여 쿠키를 추가할 수 있습니다. 메소드에는 이름과 값 그리고 쿠키가 얼마나 유효한지 결정하는 분단위의 값이 전달되어야 합니다:

```php
return response('Hello World')->cookie(
    'name', 'value', $minutes
);
```

cookie 메소드는 또한 자주 사용되지 않는 몇가지 인자를 더 받아들입니다. 일반적으로 이 인자들은 PHP의 내장된 setcookie 메소드에 제공되는 인자들과 동일한 목적과 의미가 있습니다:

```php
return response('Hello World')->cookie(
    'name', 'value', $minutes, $path, $domain, $secure, $httpOnly
);
```

또는, Cookie 파사드를 애플리케이션의 응답에 쿠키를 추가하기 위해서 "queue"할 수 있습니다. queue 메소드는 Cookie 인스턴스나 Cookie 인스턴스를 생성하는데 필요한 인자를 받습니다. 이 쿠키는 응답이 브라우저로 보내지기 전에 추가됩니다:

```php
Cookie::queue(Cookie::make('name', 'value', $minutes));

Cookie::queue('name', 'value', $minutes);
```

<br>

## 쿠키 인스턴스 생성하기
---

나중에 response 인스턴스에 넣을 수 있는 Symfony\Component\HttpFoundation\Cookie인스턴스를 생성하려면, 글로벌 cookie 헬퍼 함수를 사용할 수 있습니다. 이 쿠키는 response 인스턴스에 첨부하지 않는 한 클라이언트에게 다시 보내지지 않습니다:

```php
$cookie = cookie('name', 'value', $minutes);

return response('Hello World')->cookie($cookie);
```

