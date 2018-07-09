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

## 다른 Response 타입들
---
response 헬퍼 함수는 다른 타입의 response 인스턴스를 생성하는데 사용될 수 있습니다. response 헬퍼 함수가 인자 없이 호출되면, Illuminate\Contracts\Routing\ResponseFactory contract의 구현체가 반환됩니다. 이 contract response를 생성하는데 몇가지 유용한 메소드를 제공합니다.


## View Responses
---
response 의 status 와 header 뿐만 아니라, response 내용으로 view를 반환하고자 한다면, view 메소드를 사용해야 합니다:

```php
return response()
            ->view('hello', $data, 200)
            ->header('Content-Type', $type);
```

사용자가 직접 HTTP 상태 코드나, 특정 헤더 값을 전달할 필요는 없다면, 글로벌 view 헬퍼 함수를 사용해야 합니다.


## JSON Responses
---
json 메소드는 자동으로 Content-Type 헤더를 application/json 으로 설정하고, PHP json_encode 함수를 사용하여 주어진 배열을 JSON으로 변환할 것입니다:

```php
return response()->json([
    'name' => 'Abigail',
    'state' => 'CA'
]);
```

JSONP response 를 생성하고자 한다면, json 메소드와 setCallback를 조합하면 됩니다:

```php
return response()
            ->json(['name' => 'Abigail', 'state' => 'CA'])
            ->withCallback($request->input('callback'));
```            

## 파일 다운로드
---
download 메소드는 사용자의 브라우저가 주어진 경로에 해당하는 파일을 다운로드 하게 하는 response를 생성하는데 사용됩니다. download 메소드는 사용자가 다운로드 하는 파일의 이름을 두번째 인자로 받습니다. 마지막으로 HTTP 헤더의 배열을 세번째 인자로 전달할 수 있습니다:

```php
return response()->download($pathToFile);

return response()->download($pathToFile, $name, $headers);

return response()->download($pathToFile)->deleteFileAfterSend(true);
```

{note} 파일 다운로드를 관리하는 Symfony HttpFoundation 클래스는 ASCII 형식의 파일 이름을 지정해야 합니다.

## 스트리밍 다운로드
---

때로는, 특정 동작의 결과 내용을 디스크에 저장하지 않고 바로 다운로드 가능한 응답으로 반환하고자 할 수 있습니다. 이러한 경우 streamDownload 메소드를 사용할 수 있습니다. 이 메소드는 콜백과, 파일이름 그리고 옵션값으로 헤더 배열을 인자로 전달받습니다.

```php
return response()->streamDownload(function () {
    echo GitHub::api('repo')
                ->contents()
                ->readme('laravel', 'laravel')['contents'];
}, 'laravel-readme.md');
```


## 파일 Responses
---
file 메소드는 파일을 다운로드 하는 대신에, 브라우저에 이미지나 PDF 와 같은 파일을 표시하는데 사용되어 집니다. 이 메소드는 첫번째 인자로 파일의 경로를, 두번째 인자롣 헤더의 배열을 전달 받습니다:

```php
return response()->file($pathToFile);

return response()->file($pathToFile, $headers);
```

