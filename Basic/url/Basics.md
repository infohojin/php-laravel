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

##기본적인 내용
---
### 기본 URL 생성하기
url 헬퍼는 애플리케이션에서 사용하는 임의의 URL을 생성하는데 사용합니다. 생성된 URL은 자동으로 현재 요청-request의 (HTTP 또는 HTTPS) 스키마와 호스트를 사용합니다:

```php
$post = App\Post::find(1);

echo url("/posts/{$post->id}");

// http://example.com/posts/1
```

### 현재 URL 엑세스하기
---
url 헬퍼에 아무런 경로를 지정하지 않는다면, Illuminate\Routing\UrlGenerator 인스턴스가 반환되며, 현재 URL에 대한 정보에 엑세스 할 수 있습니다:

```php
// Get the current URL without the query string...
echo url()->current();

// Get the current URL including the query string...
echo url()->full();

// Get the full URL for the previous request...
echo url()->previous();
```

각각의 메소드는 URL 파사드를 통해서도 엑세스 할 수 있습니다:

```php
use Illuminate\Support\Facades\URL;

echo URL::current();
```

