---
layout: laravel
title: Laravel
subtitle: 소개
cate:
    name: "라라벨"
    link: "/Laravel"
---
# 해퍼 메소드




## 기타 함수들

### abort()
abort 함수는 Exception 핸들러에 의해서 렌더링 될 수 있는 HTTP exception을 발생시킵니다:

abort(403);
exception 의 응답 텍스트를 제공하거나, 커스텀 헤더를 지정할 수도 있습니다:

abort(403, 'Unauthorized.', $headers);

### abort_if()
abort_if 함수는 주어진 조건식이 true 일때 HTTP exception을 발생시킵니다:

abort_if(! Auth::user()->isAdmin(), 403);
abort 메소드와 같이, exception 의 응답 텍스트를 세번째 인자로 전달할 수 있으며, 네번째 인자로 커스텀 응답 헤더를 전달할 수도 있습니다.


### abort_unless()
abort_unless 함수는 주어진 조건식이 false 일때 HTTP exception 을 발생시킵니다:

abort_unless(Auth::user()->isAdmin(), 403);
abort 메소드와 같이, exception 의 응답 텍스트를 세번째 인자로 전달할 수 있으며, 네번째 인자로 커스텀 응답 헤더를 전달할 수도 있습니다.


### app()
app 함수는 서비스 컨테이너 인스턴스를 반환합니다:

```php
$container = app();
```

컨테이너에 의존성을 해결하고자 하는 클래스나 인터페이스의 이름을 전달할 수도 있습니다:

```php
$api = app('HelpSpot\API');
```

### auth()
auth 함수는 authenticator 인스턴스를 반환합니다. 편의를 위하여 Auth 파사드 대신 이용할 수 있습니다:

```php
$user = auth()->user();
```

필요한 경우, 어떤 guard 인스턴스를 사용할지 지정할 수 있습니다:

```php
$user = auth('admin')->user();
```

### back()
back() 함수는 사용자의 이전 위치로 리다이렉트 HTTP response-응답을 생성합니다:

```php
return back($status = 302, $headers = [], $fallback = false);

return back();
```

### bcrypt()
bcrypt 함수는 Bcrypt를 이용하여 주어진 값을 해시 처리합니다. Hash 파사드 대신 사용할 수 있습니다:

```php
$password = bcrypt('my-secret-password');
```

### broadcast()
broadcast 함수는 주어진 이벤트를 리스너들에게 broadcasts 합니다:

```php
broadcast(new UserRegistered($user));
```

### blank()
blank 함수는 주어진 값이 "빈값"인지 판단하여 결과를 반환합니다:

```php
blank('');
blank('   ');
blank(null);
blank(collect());

// true

blank(0);
blank(true);
blank(false);

// false
```

blank의 반대는, filled 메소드를 참고하십시오.


### cache()
cache 함수는 캐시로 부터 값을 가져오는데 사용할 수 있습니다. 캐시에서 주어진 키가 존재하지 않는 경우, 옵션으로 전달된 기본값(두번째인자)가 반환됩니다

```php
$value = cache('key');

$value = cache('key', 'default');
```

함수에 키 / 값으로 된 배열을 전달하여 캐시에 아이템을 추가할 수 있습니다. 또한 캐시에 값이 얼마나 유지되어야 하는지에 대한 시간(분)을 숫자로 전달할 수도 있습니다.

```php
cache(['key' => 'value'], 5);

cache(['key' => 'value'], now()->addSeconds(10));
```

### class_uses_recursive()
class_uses_recursive 함수는 모든 부모 클래스가 사용하는 trait를 포함하여 클래스가 사용하는 모든 trait을 반환합니다:

```php
$traits = class_uses_recursive(App\User::class);
```

### collect()
collect 함수는 주어진 값으로부터 collection 인스턴스를 생성합니다:

```php
$collection = collect(['taylor', 'abigail']);
```

### config()
config 함수는 설정 변수의 값을 가져옵니다. 설정 값은 파일명과 접근하고자 하는 옵션을 포함하는 "점(.)" 문법(syntax)를 사용하여 접근할 수 있습니다. 설정 옵션이 존재하지 않는다면 지정된 기본값이 반환됩니다:

```php
$value = config('app.timezone');

$value = config('app.timezone', $default);
```

런타임에 설정 값을 키 / 값 형태로 전달하여 값을 설정할 수도 있습니다:

```php
config(['app.debug' => true]);
```

### cookie()
cookie 함수는 새로운 쿠키 인스턴스를 생성합니다:

```php
$cookie = cookie('name', 'value', $minutes);
```

### csrf_field()
csrf_field 함수는 CSRF 토큰 값을 포함하는 HTML hidden Input 필드를 생성합니다. 예를 들어 Blade syntax에서 사용할 수 있습니다:

```php
{{ csrf_field() }}
```

### csrf_token()
csrf_token 함수는 현재 CSRF 토큰의 값을 조회합니다:

```php
$token = csrf_token();
```

### dd()
dd 함수는 주어진 변수들을 Dump 처리하고 스크립트의 실행을 중단합니다:

```php
dd($value);

dd($value1, $value2, $value3, ...);
```

스크립트 실행을 중단하고 싶지 않다면, dump 함수를 사용하십시오:


### decrypt()
decrypt 함수는 라라벨의 encrypter를 사용하여 주어진 값을 복호화 합니다:

```php
$decrypted = decrypt($encrypted_value);
```

### dispatch()
dispatch 함수는 라라벨의 job queue에 주어진 job을 추가합니다:

```php
dispatch(new App\Jobs\SendEmails);
```

### dispatch_now()
dispatch_now 함수는 주어진 job을 즉시 실행하고 handle 메소드의 값을 반환합니다:

```php
$result = dispatch_now(new App\Jobs\SendEmails);
```

### dump()
dump 함수는 주어진 변수의 값을 덤프하여 표시합니다:

```php
dump($value);

dump($value1, $value2, $value3, ...);
```

변수읙 값을 표시한 다음에 스크립트의 실행을 멈추고자 한다면, dd함수를 사용하십시오.


### encrypt()
encrypt 함수는 라라벨의 encrypter를 사용하여 주어진 값을 암호화합니다:

```php
$encrypted = encrypt($unencrypted_value);
```

### env()
env 함수는 환경변수의 값을 가져오거나 기본값을 조회합니다:

```php
$env = env('APP_ENV');

// Returns 'production' if APP_ENV is not set...
$env = env('APP_ENV', 'production');
```

{note} 배포과정에서 config:cache 명령어를 실행했다면, 설정 파일안에서 env 함수를 호출한 부분이 있는지 확인해야 합니다. 설정이 캐싱되고 나면, .env 파일은 로드하지 않고, 모든 env 함수는 null을 반환합니다.


### event()
event 함수는 주어진 event를 리스너들에게 보냅니다:

```php
event(new UserRegistered($user));
```

### factory()
factory 함수는 주어진 클래스, 이름, 양을 위한 모델 팩토리 빌더를 생성합니다. testing이나 seeding 중에 이용할 수 있습니다:

```php
$user = factory(App\User::class)->make();
```

### filled()
filled 함수는 주어진 값이 "빈값"이 아닌지 판별하여 결과를 반환합니다:

```php
filled(0);
filled(true);
filled(false);

// true

filled('');
filled('   ');
filled(null);
filled(collect());

// false
```

filled의 반대는, blank 메소드를 확인하십시오.

### info()
info 함수는 로그에 정보를 기록합니다:

```php
info('Some helpful information!');
```

문맥에대한 데이터를 함수에 배열로 전달할 수도 있습니다:

```php
info('User login attempt failed.', ['id' => $user->id]);
```

### logger()
logger 함수는 로그에 debug 로그 레벨을 기록하는데 사용합니다:

```php
logger('Debug message');
```

문맥에대한 데이터를 함수에 배열로 전달할 수도 있습니다:

```php
logger('User has logged in.', ['id' => $user->id]);
```

함수에 아무런 값이 전달되지 않으면 logger 인스턴스가 반환됩니다:

```php
logger()->error('You are not allowed here.');
```

### method_field()
method_field 함수는 HTTP 메소드 형식의 가짜(spoof) 값을 포함하는 HTML hidden Input 필드를 생성합니다. 예를 들어 Blade syntax에서 사용할 수 있습니다:

```php
<form method="POST">
    {{ method_field('DELETE') }}
</form>
```

### now()
now 함수는 현재 시간을 기반으로한 Illuminate\Support\Carbon 인스턴스를 생성합니다:

```php
$now = now();
```

### old()
old 함수는 세션에 저장된 이전 입력값(flashed)을 조회합니다: $value = old('value');

```php
$value = old('value', 'default');
```



### optional()
optional 함수는 인자를 전달받아 해당 객체의 프로퍼티에 엑세스 하거나, 메소드를 호출할 수 있도록 합니다. 지정된 객체가 null 이라면, 프로퍼티와 메소드는 에러를 유발하는 대신에 null 을 반환합니다:

```php
return optional($user->address)->street;

{!! old('name', optional($user)->name) !!}
```

optional 함수는 두번째 인자로 클로저를 받을 수 있습니다. 첫번째 인자가 null이 아닌경우, 클로저가 호출됩니다:

```php
return optional(User::find($id), function ($user) {
    return new DummyUser;
});
```

### policy()
policy 메소도는 주어진 클래스를 위한 policy 인스턴스를 조회합니다:

```php
$policy = policy(App\User::class);
```

### redirect()
redirect 함수는 리다이렉트 HTTP response-응답을 반환하거나, 인자 없이 호출되는 경우 리디렉터 인스턴스를 반환합니다:

```php
return redirect($to = null, $status = 302, $headers = [], $secure = null);

return redirect('/home');

return redirect()->route('route.name');
```

### report()
report 함수는 exception 핸들러의 report 메소드를 사용하여 exception-예외를 보고합니다:

```php
report($e);
```

### request()
request 함수는 현재의 요청 인스턴스를 반환하거나 입력 아이템을 가져옵니다:

```php
$request = request();

$value = request('key', $default);
```

### rescue()
rescue 함수는 주어진 클로저를 실행하고, 실행중 발생하는 예외-exception을 받아냅니다. catch 된 모든 예외-exception은 exception 핸들러의 report 메소드로 전달되지만, request-요청 에 대한 처리는 계속됩니다:

```php
return rescue(function () {
    return $this->method();
});
```

rescue 함수에는 두번째 인자를 전달 할 수 있는데, 이 인자는 클로저를 실행하는 동안 예외-exception가 발생하면 반환된 "기본값" 입니다:

```php
return rescue(function () {
    return $this->method();
}, false);

return rescue(function () {
    return $this->method();
}, function () {
    return $this->failure();
});
```

### resolve()
resolve 함수는 서비스 컨테이너를 사용하여 주어진 클래스 또는 인터페이스를 의존성 해결하여 인스턴스를 반환합니다:

```php
$api = resolve('HelpSpot\API');
```

### response()
response 함수는 응답 인스턴스를 생성하거나 응답 팩토리의 인스턴스를 가져옵니다:

```php
return response('Hello World', 200, $headers);

return response()->json(['foo' => 'bar'], 200, $headers);
```

### retry()
retry 함수는 주어진 최대 횟수가 될 때까지 주어진 콜백을 실행하려고 시도합니다. 콜백이 예외-exception를 던지지 않는다면, 결과 값이 반환됩니다. 콜백이 예외-exception를 던지면 자동으로 다시 시도됩니다. 최대 재시도 횟수를 초과하면 예외가 던져집니다.

```php
return retry(5, function () {
    // Attempt 5 times while resting 100ms in between attempts...
}, 100);
```

### session()
session 함수는 세션 값을 얻거나 지정하는 데에 사용할 수 있습니다.

```php
$value = session('key');
```

키 / 값 쌍들의 배열을 함수로 전달하여 값을 설정할 수 있습니다:

```php
session(['chairs' => 7, 'instruments' => 3]);
```

함수에 아무런 값도 전달되지 않는다면 세션 스토어가 반환됩니다:

```php
$value = session()->get('key');

session()->put('key', $value);
```

### tap()
tap 함수는 임의의 $value 와 클로저 두개의 인자를 받아들입니다. $value 는 클로저에 전달되어 tap 함수에 의해서 반환됩니다. 반환되는 값은 클로저와 무관합니다:

```php
$user = tap(User::first(), function ($user) {
    $user->name = 'taylor';

    $user->save();
});
```

tap 함수에 클로저를 전달하지 않는다면, 주어진 $value 에 모든 메소드를 호출할 수 있습니다. 메소드에서 반환하는 값은 메소드가 실제로 정의해서 반환하는 $value 와는 관계없이 항상 $value 가 됩니다. 예를 들어 Eloquent update 메소드는 일반적으로 정수값을 반환하지만, tap 메소드를 통해서 update 메소드를 호출을 체이닝하면 메소드가 모델 그 자체를 반환하도록 할 수 있습니다:

```php
$user = tap($user)->update([
    'name' => $name,
    'email' => $email,
]);
```

### today()
today 함수는 현재 날짜를 기준으로한 Illuminate\Support\Carbon 인스턴스를 생성합니다:

```php
$today = today();
```

### throw_if()
throw_if 함수는 주어진 결과가 true 인 경우에 주어진 exception-예외를 던집니다:

```php
throw_if(! Auth::user()->isAdmin(), AuthorizationException::class);

throw_if(
    ! Auth::user()->isAdmin(),
    AuthorizationException::class,
    'You are not allowed to access this page'
);
```

### throw_unless()
throw_unless 함수는 주어진 결과가 false 인 경우에 주어진 exception-예외를 던집니다:

```php
throw_unless(Auth::user()->isAdmin(), AuthorizationException::class);

throw_unless(
    Auth::user()->isAdmin(),
    AuthorizationException::class,
    'You are not allowed to access this page'
);

```

### trait_uses_recursive()
trait_uses_recursive 함수는 해당 trait에서 사용된 모든 trait을 반환합니다:

```php
$traits = trait_uses_recursive(\Illuminate\Notifications\Notifiable::class);
```

### transform()
transform 함수는 주어진 값이 빈값이 아닌 경우에 Closure를 실행하고 Closure의 결과를 반환합니다:

```php
$callback = function ($value) {
    return $value * 2;
};

$result = transform(5, $callback);

// 10
```

세번째 인자로 기본값 또는 Closure 가 전달될 수 있습니다. 이 값은 첫번째 인자가 빈값인 경우 반환됩니다:

```php
$result = transform(null, $callback, 'The value is blank');

// The value is blank
```

### validator()
validator 함수는 주어진 인자를 통해서 새로운 validator 인스턴스를 생성합니다. 보다 편리하게 Validator 파사드를 사용할 수도 있습니다:

```php
$validator = validator($data, $rules, $messages);
```

### value()
value 함수는 자신에게 주어진 값을 그대로 반환합니다. 그렇지만 함수에 Closure를 전달하면 Closure가 실행되고 그 결과물이 반환됩니다:

```php
$result = value(true);

// true

$result = value(function () {
    return false;
});

// false
```

### view()
view 함수는 view 인스턴스를 조회합니다:

```php
return view('auth.login');
```

### with()
with 함수는 자신에게 주어진 값을 그대로 반환합니다. 만약 함수에 두번째 인자로 Closure 가 전달되면, Closure 가 실행되어 그 결과를 반환합니다:

```php
$callback = function ($value) {
    return (is_numeric($value)) ? $value * 2 : 0;
};

$result = with(5, $callback);

// 10

$result = with(null, $callback);

// 0

$result = with(5, null);

// 5
```
