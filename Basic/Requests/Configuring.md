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

## 신뢰할 수 있는 프록시 설정하기
---
TLS / SSL 인증서가 적용된 로드 밸런서 뒤에서 애플리케이션을 실행할 때 애플리케이션에서 HTTPS 링크가 생성되지 않는 경우가 있습니다. 일반적으로 이는 애플리케이션이 포트 80의 로드 밸런서에서 전송되는 트래픽뒤에 위치해서, HTTPS 링크를 생성해야 한다는 것을 알지 못하기 때문입니다.

이 문제를 해결하기 위해서, 라라벨 애플리케이션에는 App\Http\Middleware\TrustProxies 미들웨어를 사용하여 손쉽게 애플리케이션에서 신뢰할 수 있는 로드밸런서 또는 프록시를 설정할 수 있습니다. 신뢰할 수 있는 프록시들은 이 미들웨어의 $proxies 속성 배열에 지정해놓으면 됩니다. 신뢰할 수 있는 프록시를 구성하는 것 이외에도 신뢰해야 하는 프록시 헤더를 설정 할 수 있습니다:

```php
<?php

namespace App\Http\Middleware;

use Illuminate\Http\Request;
use Fideloper\Proxy\TrustProxies as Middleware;

class TrustProxies extends Middleware
{
    /**
     * The trusted proxies for this application.
     *
     * @var array
     */
    protected $proxies = [
        '192.168.1.1',
        '192.168.1.2',
    ];

    /**
     * The headers that should be used to detect proxies.
     *
     * @var string
     */
    protected $headers = Request::HEADER_X_FORWARDED_ALL;
}
```

{팁} 만약 여러분이 AWS Elastic 로드 발란싱을 사용하고 있다면, 여러분의 $headers 값은 Request::HEADER_X_FORWARDED_AWS_ELB이 되어야 합니다. $headers에 사용될 수 있는 상수에 대한 다른 정보는, Symfony 문서의 trusting proxies를 참고하십시오.

## 모든 프록시 신뢰하기
---
아마존 AWS 또는 다른 "클라우드" 로드밸런서를 사용하는 경우에는, 실제 로드밸런서의 IP를 알 수가 없습니다. 이 경우, 모든 프록시를 신뢰할 수 있도록 하기 위해서 * 를 사용할 수 있습니다:

```php
/**
 * The trusted proxies for this application.
 *
 * @var array
 */
protected $proxies = '*';
```