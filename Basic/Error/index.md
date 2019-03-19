---
layout: laravel
title: Laravel
subtitle: Basic
cate:
    name: "기본사항"
    link: "/Laravel/Basic"


---

에러 핸들링
소개하기
설정하기
Exception 핸들러
Report 메소드
Render 메소드
Reportable & Renderable Exceptions
HTTP Exceptions
커스텀-사용자 지정 HTTP 에러 페이지

## 소개하기
---



라라벨은 기본적으로 오류 및 예외처리에 관한 모든 설정이 되어 있습니다. 라라벨이 동작을 하면서 발생되는 모든 Exception 은 `App\Exceptions\Handler` 클래스에 의해 로그를 남기게 됩니다. 그리고 이렇게 저장된 로그기록을 reponse 로 화면에 출력합니다.

라라벨은 로그 기록을 생성하기 위해서 `Monolog` 라이브러리를 사용합니다.
모노로그는 다양한 로그 핸들러를 생성할 수 있는 인기 있는 외부 패키지 입니다.

로그는 파일하나 또는 일정 주기, 시스템 로그에 기록등의 다양한 핸들러를 설정할 수 있습니다.

<br><br><br>

## 설정하기
---

디버그의 옵션은 `config/app.php` 파일애 설정을 합니다. 얼마나 많은 에러애 대한 정보를 사용자에게 노출할 것인지 결정합니다. 

디버그 옵션은 .env 파일에서 `APP_DEBUG` 환경 변수에 따라서 결정됩니다.

보통 로컬 개발 환경에서는 APP_DEBUG 환경 변수 값을 true 로 설정하여 개발을 하지만, 실제 서비스 프로덕션 환경에서는 이 값은 false 로 설정되어야 합니다. 

실 서버 환경에서 이 값을 true 로 설정한면 오류가 발생시 애플리케이션의 민감한 환경 설정 값이 사용자에게 표시되는 위험이 발생될 수 있습니다.

<br><br><br>

## Exception 핸들러
---

### Report 메소드
---

라라벨의 모든 `Exception`는 `App\Exceptions\Handler` 클래스에 의해서 동작합니다. 

그중에서 `report` 메소드는 `Exceptions`을 로깅 처리하거나 Bugsnag 또는 Sentry 같은 별도의 외부 서비스로 보내는데 사용되어 집니다. 
기본적으로 report 메소드는 예외-exception가 기록되는 경우, 베이스 클래스로 예외-exception을 전달합니다.

예를 들어 각기 다른 유형의 예외-exception을 다른 방법을 처리해야 할때는 PHP의 instanceof 비교 연산자로 구분할 수 있습니다.

```php
/**
 * Report or log an exception.
 *
 * This is a great spot to send exceptions to Sentry, Bugsnag, etc.
 *
 * @param  \Exception  $exception
 * @return void
 */
public function report(Exception $exception)
{
    if ($exception instanceof CustomException) {
        //
    }

    return parent::report($exception);
}
```

{tip} report 메소드 안에서 수많은 instanceof 체크를 하는 대신에 reportable exceptions을 사용하도록 하십시오.

<br>

### report 헬퍼 함수
---

라라벨이 동작중에 Exception이 발생되면 `report` 처리를 해야 합니다. 하지만 요청된 `request`를 계속처리를 할 수 있습니다.
 
이때 `report` 헬퍼 함수는 에러 페이지를 렌더링 하지 않습니다. 

exception 핸들러의 report 메소드를 사용하여 보다 빠르게 예외사항을 보고합니다:

```php
public function isValid($value)
{
    try {
        // Validate the value...
    } catch (Exception $e) {
        report($e);

        return false;
    }
}
```
<br>

### 유형에 따라서 예외-exception 무시하기
---

Exception 핸들러의 `$dontReport`의  배열값에 정의된 Exception은 로그 기록이 되지 않습니다.

예를 들어 404 에러뿐만 아니라 다른 유형의 몇몇 에러들은 로그파일에 기록되지 않습니다. 

필요한 경우에 다른 예외-exception 유형들도 이 배열에 추가할 수 있습니다.

```php
/**
 * A list of the exception types that should not be reported.
 *
 * @var array
 */
protected $dontReport = [
    \Illuminate\Auth\AuthenticationException::class,
    \Illuminate\Auth\Access\AuthorizationException::class,
    \Symfony\Component\HttpKernel\Exception\HttpException::class,
    \Illuminate\Database\Eloquent\ModelNotFoundException::class,
    \Illuminate\Validation\ValidationException::class,
];
```

<br>

## Render 메소드
---

render 메소드는 주어진 예외-exception를 HTTP 응답으로 변환하고 브라우저에게 보내는 역할을 담당합니다. 기본적으로 예외-exception는 응답을 생성하는 기본 클래스로 전달됩니다. 하지만 예외-exception의 유형을 파악하고 사용자 지정된 응답을 반환 것도 여러분의 자유입니다.

```php
/**
 * Render an exception into an HTTP response.
 *
 * @param  \Illuminate\Http\Request  $request
 * @param  \Exception  $exception
 * @return \Illuminate\Http\Response
 */
public function render($request, Exception $exception)
{
    if ($exception instanceof CustomException) {
        return response()->view('errors.custom', [], 500);
    }

    return parent::render($request, $exception);
}
```
<br>

## Reportable & Renderable Exceptions
---

Exception 핸들러의 report 그리고 render 메소드에서 exception의 타입을 확인하는 대신에 여러분이 정의한 고유한 exception에 report 와 render 메소드르르 정의할 수 있습니다. 이 메소드가 존재한다면 프레임워크는 자동으로 이 메소드를 호출합니다:

```php
<?php

namespace App\Exceptions;

use Exception;

class RenderException extends Exception
{
    /**
     * Report the exception.
     *
     * @return void
     */
    public function report()
    {
        //
    }

    /**
     * Render the exception into an HTTP response.
     *
     * @param  \Illuminate\Http\Request
     * @return \Illuminate\Http\Response
     */
    public function render($request)
    {
        return response(...);
    }
}
```


<br><br><br>


## HTTP 예외-exception
---
어떤 예외들-exceptions은 서버에서 발생하는 HTTP 에러 코드를 나타냅니다. 예를 들어, "페이지를 찾을 수 없습니다." 에러(404)나, 인증 오류 (401) 또는 개발자가 유발하는 500 오류 같은 것들입니다. 애플리케이션의 어느 곳에서든 이 같은 응답을 반환하려면 abort 헬퍼 함수를 사용하면 됩니다:

```php
abort(404);
```

abort 헬퍼함수는 즉시 예외-exception을 발생시키고, 그 예외는 Exception 핸들러에 의해 렌더링될 것입니다. 선택적으로 응답의 텍스트를 지정할 수도 있습니다.

```php
abort(403, 'Unauthorized action.');
```

## 사용자 지정 HTTP 에러 페이지
---
라라벨에서는 HTTP 응답 코드에 따른 다양한 사용자 지정 에러 페이지를 표시할 수 있습니다. 예를 들어 404 HTTP 응답 코드에 대한 에러 페이지를 수정하고자 한다면, resources/views/errors/404.blade.php 파일을 생성하면 됩니다. 이 파일은 애플리케이션에서 404 에러가 발생했을 때 사용자에게 보여질 것입니다. 이 디렉토리안에 있는 뷰의 이름은 해당 HTTP 상태 코드와 일치해야 합니다. abort 함수에 의해서 발생한 HttpException 인스턴스는 뷰에 $exception변수로 전달될 것입니다.

```php
<h2>{{ $exception->getMessage() }}</h2>
```
