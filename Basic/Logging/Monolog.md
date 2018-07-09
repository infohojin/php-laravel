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

## 확장된 Monolog 채널 커스터마이징하기
---
채널에서 사용하는 Monolog 커스터마이징하기
때로는 채널에서 사용하는 Monolog를 제어할 필요가 있을 수도 있습니다. 예를 들어, 주어진 채널 핸들러에 대해서 FormatterInterface 구현을 커스터마이징 하기를 원할 수도 있습니다.

이렇게 하기 위해서 먼저 채널 설정 배열에 tap 속성을 정의하면 됩니다. tap 배열은 생성 된 Monolog 인스턴스를 커스터마이징 할 수 있는 클래스의 리스트로 구성되어 있어야 합니다:

```php
'single' => [
    'driver' => 'single',
    'tap' => [App\Logging\CustomizeFormatter::class],
    'path' => storage_path('logs/laravel.log'),
    'level' => 'debug',
],
```

채널에 tap 옵션을 설정했다면, Monolog 인스턴스를 커스터마이징 하는 클래스를 정의하면 됩니다. 이 클래스에서는 Monolog 인스턴스를 전달받는 __invoke 메소드를 정의할 필요가 있습니다.

```php
<?php

namespace App\Logging;

class CustomizeFormatter
{
    /**
     * Customize the given Monolog instance.
     *
     * @param  \Monolog\Logger
     * @return void
     */
    public function __invoke($monolog)
    {
        foreach ($monolog->getHandlers() as $handler) {
            $handler->setFormatter(...);
        }
    }
}
```

{tip} 모든 "tap" 클래스는 서비스 컨테이너에 의해서 의존성이 해결되기 때문에, 생성자에 정의된 의존성은 자동으로 주입됩니다.


## Monolog 핸들러 채널 생성하기
---
Monolog 는 다양한 핸들러를 가지고 있습니다. 몇 가지 경우에서, 생성하고자 하는 로거의 타입은 특정 핸들러 인스턴스를 가지는 Monolog 드라이버일 뿐입니다.

monolog 드라이버를 사용할 때, handler 설정 옵션은 어떤 핸들러가 인스턴스화 되어야 하는지 지정하는데 사용합니다. 옵션으로, 특정 핸들러의 생성자 파라미터가 필요한 경우 handler_with 설정을 사용하여 필요한 옵션을 지정할 수 있습니다:

```php
'logentries' => [
    'driver'  => 'monolog',
    'handler' => Monolog\Handler\SyslogUdpHandler::class,
    'handler_with' => [
        'host' => 'my.logentries.internal.datahubhost.company.com',
        'port' => '10000',
    ],
],
```

### Monolog Formatters
---
monolog 드라이버를 사용할 때, Monolog 의 LineFormatter 가 기본적인 포맷터로 사용됩니다. 그렇지만, 핸들러가 사용할 formatter 와 formatter_with 설정 옵션을 사용하여 포맷터의 타입을 커스터마이징 할 수 있습니다:

```php
'browser' => [
    'driver' => 'monolog',
    'handler' => Monolog\Handler\BrowserConsoleHandler::class,
    'formatter' => Monolog\Formatter\HtmlFormatter::class,
    'formatter_with' => [
        'dateFormat' => 'Y-m-d',
    ],
],
```

고유한 formatter를 제공하는 Monolog 핸들러를 사용한다면, formatter 설정 옵션을 default 설정을 사용하면 됩니다:

```php
'newrelic' => [
    'driver' => 'monolog',
    'handler' => Monolog\Handler\NewRelicHandler::class,
    'formatter' => 'default',
],
```

## 팩토리를 사용하여 채널 생성하기
---
Monolog 인스턴스와 설정을 완벽하게 제어할 수 있는 커스텀 채널을 정의하려면, config/logging.php 설정 파일에 custom 드라이버를 지정할 수 있습니다. 설정에서는 팩토리에서 Monolog 인스턴스를 생성하기 위해서 호출해야 하는 클래스를 지정하는 via 옵션을 포함해야 합니다:

```php
'channels' => [
    'custom' => [
        'driver' => 'custom',
        'via' => App\Logging\CreateCustomLogger::class,
    ],
],
```
custom 채널을 설정한 다음에, Monolog 인스턴스를 생성하는 클래스를 정의하면 됩니다. 이 클래스에서는 Monolog 인스턴스를 반환하는 __invoke 메소드를 정의할 필요가 있습니다:

```php
<?php

namespace App\Logging;

use Monolog\Logger;

class CreateCustomLogger
{
    /**
     * Create a custom Monolog instance.
     *
     * @param  array  $config
     * @return \Monolog\Logger
     */
    public function __invoke(array $config)
    {
        return new Logger(...);
    }
}
```