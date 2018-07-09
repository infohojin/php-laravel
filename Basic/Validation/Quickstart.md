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

## 빠르게 유효성 검사 살펴보기
---
라라벨의 강력한 유효성 검사 기능에 대해 알아보기 위해서, form을 유효성 검사한 뒤 사용자에게 에러 메세지를 보여주는 예제를 살펴보도록 하겠습니다.


## 라우트 정의하기
---
우선 다음의 라우트들이 routes/web.php 파일에 정의되어 있다고 가정해 보겠습니다:

```php
Route::get('post/create', 'PostController@create');

Route::post('post', 'PostController@store');
```

당연히, GET 라우트는 사용자가 새로운 블로그 포스트를 생성하기 위한 form을 나타낼 것이고, POST 라우트는 데이터베이스에 새로운 블로그 포스트를 저장할 것입니다.


## 컨트롤러 생성하기
---
다음으로, 이 라우트들을 다루는 간단한 컨트롤러를 살펴보겠습니다. 지금은 store 메소드를 비어있는 채로 둘 것입니다:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class PostController extends Controller
{
    /**
     * Show the form to create a new blog post.
     *
     * @return Response
     */
    public function create()
    {
        return view('post.create');
    }

    /**
     * Store a new blog post.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        // Validate and store the blog post...
    }
}
```

### 유효성 검사 로직 작성하기
---
이제 새로운 블로그 포스트에 대해 유효성을 검사하는 로직을 store 메소드에 채워넣을 준비가 되었습니다. 이를 위해서 Illuminate\Http\Request 객체에 제공되는 validate 메소드를 사용할 것입니다. 유효성 검사 룰들을 통과하게되면 코드는 계속해서 정상적으로 실행됩니다. 하지만 유효성 검사를 통과하지 못할 경우, 예외-exception가 던져지고 적절한 오류 응답이 사용자에게 자동으로 보내질 것입니다. 전통적인 HTTP 요청의 경우, 리다이렉트 응답이 생성될 것이며 AJAX 요청에는 JSON 응답이 보내질 것입니다.

validate 메소드에 대해 더 잘 이해하기 위해, 다시 store 메소드로 돌아가 보겠습니다:

```php
/**
 * Store a new blog post.
 *
 * @param  Request  $request
 * @return Response
 */
public function store(Request $request)
{
    $validatedData = $request->validate([
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
    ]);

    // The blog post is valid...
}
```

보시다시피, 원하는 유효성 검사 룰을 메소드에 전달하기만 하면 됩니다. 다시말해, 유효성 검사가 실패하면, 적절한 응답이 자동으로 생성됩니다. 유효성 검사를 통과하면 컨트롤러가 계속해서 정상적으로 로직을 수행합니다.

## 첫번째 유효성 검사가 실패하면 중지하기
---
경우에 따라서 첫번째 유효성 감사가 실패하면 속성값에 대한 검사를 중단하기를 원할수도 있습니다. 이러한 경우 bail 규칙을 지정하면 됩니다:

```php
$request->validate([
    'title' => 'bail|required|unique:posts|max:255',
    'body' => 'required',
]);
```

이 예제에서는, unique 규칙으로 지정된 title 속성의 유효성 검사가 실패하면 max 규칙은 확인하지 않습니다. 유효성 검사 규칙은 선언된 순서대로 검사될 것입니다.

## 중첩된 속성에 대한 유의사항
---
HTTP 요청이 "중첩된" 파라미터를 가지고 있다면 ".(점)" 문법을 사용하여 유효성 확인 규칙을 지정할 수 있습니다:

```php
$request->validate([
    'title' => 'required|unique:posts|max:255',
    'author.name' => 'required',
    'author.description' => 'required',
]);
```

## 유효성 검사 에러 표시하기
---
그럼 입력되는 요청-request의 파라미터들이 유효성 검사를 통과하지 못하는 경우에는 어떻게 될까요? 앞서 언급한대로, 라라벨은 사용자를 자동으로 이전의 위치로 리다이렉트합니다. 또한 모든 유효성 확인 에러는 자동으로 세션에 임시 저장될 것입니다.

다시 말하지만, GET 라우트에서 에러 메세지를 뷰와 명시적으로 연결하지 않아도 됩니다. 왜냐하면, 라라벨이 세션 데이터에서 에러가 있는지 확인하고, 에러가 있다면 뷰에 자동으로 연결해 주기 때문입니다. $errors 변수는 Illuminate\Support\MessageBag의 인스턴스일 것입니다. 이 객체를 다루는 법에 대해 더 알아보고 싶다면 문서을 확인해보시기 바랍니다.

{tip} $errors 변수는 web 미들웨어 그룹에 의해서 제공되는 Illuminate\View\Middleware\ShareErrorsFromSession 미들웨어에 의해서 뷰와 연결됩니다. 이 미들웨어가 지정되었을 때 $errors 변수는 뷰 안에서 항상 사용가능 할 것이므로, 편하게 생각하면 $errors 변수는 항상 선언되어 있다고 생각하고 안정하게 사용할 수 있습니다.

따라서, 이 예제에서, 유효성 검사를 통과하지 못할 경우 사용자는 컨트롤러의 create 메소드로 리다이렉트 될것이고, 뷰에서는 에러 메세지가 표시됩니다:

```php
<!-- /resources/views/post/create.blade.php -->

<h1>Create Post</h1>

@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif

<!-- Create Post Form -->
```

## 옵션 필드에 대한 주의사항
---
기본적으로 라라벨은 애플리케이션의 글로벌 미들웨어 스택에 TrimStrings 그리고 ConvertEmptyStringsToNull 미들웨어를 포함하고 있습니다. 이 미들웨어는 App\Http\Kernel 클래스의 미들웨어 스택에 나열되어 있습니다. 이때문에, 유효성 검사에서 null이 유효하지 않은것으로 간주하지 않으려면 "선택적-optional" request-요청 필드를 nullable로 표시할 필요도 있습니다. 예를들면:

```php
$request->validate([
    'title' => 'required|unique:posts|max:255',
    'body' => 'required',
    'publish_at' => 'nullable|date',
]);
```

이 예제에서는 publish_at 필드가 null이거나 유효한 날짜 형식이라고 지정했습니다. 만약 nullable 규칙이 추가되지 않은 경우 null값은 유효하지 않다고 결정됩니다.


## AJAX 요청과 유효성 검사
---
이 예제에서는, 애플리케이션에 전통적인 form을 이용하여 데이터를 보냈습니다. 하지만 많은 애플리케이션이 AJAX 요청을 사용합니다. AJAX reqeust 중에서 validate 메소드를 사용한다면 라라벨은 리다이렉트 응답을 생성하지 않을 것입니다. 대신 라라벨은 유효성 검사의 모든 실패 에러들을 포함하는 JSON 응답을 생성할 것입니다. 이 JSON 응답은 422 HTTP 상태 코드와 함께 보내질 것입니다.


