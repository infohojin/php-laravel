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

## Form Request 유효성 검사
---

Form Requests 생성하기
보다 복잡한 유효성 검사 시나리오의 경우, "form request"를 작성하는 것이 좋습니다. Form request는 request 클래스의 사용자 정의(커스텀) 클래스로, 유효성 검사 로직을 가지고 있습니다. form request 클래스를 생성하기 위해서는 make:request 아티즌 CLI 명령어를 사용하십시오:

php artisan make:request StoreBlogPost
생성된 클래스는 app/Http/Requests 디렉토리에 저장됩니다. 이 디렉토리가 존재하지 않는다면 make:request 명령어가 실행될 때 생성됩니다. rules 메소드에 몇가지 유효성 검사 규칙을 추가해 보겠습니다:

```php
/**
 * Get the validation rules that apply to the request.
 *
 * @return array
 */
public function rules()
{
    return [
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
    ];
}
```

그렇다면 유효성 검사 규칙은 어떻게 실행할까요? 여러분이 해야할일은 컨트롤러 메소드에 request 를 타입-힌트 하는 것입니다. 유입된 form request 는 컨트롤러 메소드가 호출되기 전에 유효성 검사를 수행합니다. 즉 컨트롤러에 유효성 검사 로직을 포함시키지 않아도 됩니다.

```php
/**
 * Store the incoming blog post.
 *
 * @param  StoreBlogPost  $request
 * @return Response
 */
public function store(StoreBlogPost $request)
{
    // The incoming request is valid...

    // Retrieve the validated input data...
    $validated = $request->validated();
}
```

유효성 검사가 실패하면 리다이렉션 응답-response이 생성되어 사용자를 이전 위치로 되돌려 보냅니다. 오류를 세션에 임시저장하여 화면에 표시 할 수도 있습니다. AJAX 요청인 경우 유효성 검사 오류가 JSON 형식으로 재구성되어 422 상태 코드가있는 HTTP 응답이 사용자에게 반환됩니다.

## Form Request 에 After 후킹 추가하기
---
form request 에 "after" 후킹을 추가하려면, withValidator 메소드를 사용하면 됩니다. 이 메소드는 생성된 완전한 validator를 전달 받는데, 유효성 검사 규칙이 실제로 수행되기 전에 메소드 중 하나를 호출할 수 있도록 해줍니다:

```php
/**
 * Configure the validator instance.
 *
 * @param  \Illuminate\Validation\Validator  $validator
 * @return void
 */
public function withValidator($validator)
{
    $validator->after(function ($validator) {
        if ($this->somethingElseIsInvalid()) {
            $validator->errors()->add('field', 'Something is wrong with this field!');
        }
    });
}
```

## Form Requests 사용자 승인
---
form request 클래스는 또한 authorize 메소드를 가지고 있습니다. 이 메소드 안에서 여러분은 인증된 사용자가 주어진 리소스에 대해서 수정할 수 있는 권한이 있는지 확인할 수 있습니다. 예를 들어, 사용자가 블로그 포스트의 탯글을 수정하려고 시도할 때, 그 본인의 코멘트인지 확인할 수 있습니다:

```php
/**
 * Determine if the user is authorized to make this request.
 *
 * @return bool
 */
public function authorize()
{
    $comment = Comment::find($this->route('comment'));

    return $comment && $this->user()->can('update', $comment);
}
```

모든 form request는 라라벨의 베이스 request 클래스를 확장하고 있기 때문에, 현재 인증된 사용자에 엑세스 하기 위해서 user 메소드를 사용할 수 있습니다. 다음 예제에서 route 메소드를 호출하는것에 주목하십시오. 예제에서 이 메소드는 {comment} 파라미터와 같이 파라미터가 정의된 라우트의 URI 에 여러분이 엑세스 접근할 수 있는지 권한을 확인합니다:

```php
Route::post('comment/{comment}');
```

만약 authorize 메소드가 false를 리턴하면, 403 HTTP 응답이 자동적으로 반환되고 컨트롤러 메소드는 실행되지 않을 것입니다.

여러분이 애플리케이션의 다른 부분에 있는 인증로직을 사용할 계획이라면, authorize 메소드에서 true를 리턴하면 됩니다.

```php
/**
 * Determine if the user is authorized to make this request.
 *
 * @return bool
 */
public function authorize()
{
    return true;
}
```

## 에러 메세지를 사용자 정의하기(커스터마이징하기)
---
messages 메소드를 대체하면 form request-요청이 사용하는 에러 메세지를 커스터마이즈할 수 있습니다. 이 메소드는 속성 / 규칙의 쌍으로된 배열과 그에 상응하는 오류 메세지를 반환합니다:

```php
/**
 * Get the error messages for the defined validation rules.
 *
 * @return array
 */
public function messages()
{
    return [
        'title.required' => 'A title is required',
        'body.required'  => 'A message is required',
    ];
}
```

