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

## 설정하기
---
애플리케이션의 로깅 시스템 설정은 config/logging.php 파일에 지정되어 있습니다. 이 파일을 통해서 애플리케이션의 로그 채널을 설정하기 때문에, 사용가능한 채널과 옵션을 살펴보도록 하십시오. 물란 다음의 몇가지 일반적인 옵션을 살펴보도록 하겠습니다.

기본적으로 라라벨은 로그를 stack 채널을 통해서 생성합니다. stack 채널은 여러개의 로그 채널을 하나의 채널로 묶어서 사용한다는 의미입니다. 스택을 구성하는 보다 자세한 내용은 아래의 문서를 참고하십시오.

### 채널 이름 설정하기
---
기본적으로 Monolog는 현재의 환경과 일치하는 production 또는 local 과 같은 이름의 "채널 이름"으로 인스턴스가 생성됩니다. 이 값을 변경하려면, 채널 설정에서 name 옵션을 추가하면 됩니다:

```php
'stack' => [
    'driver' => 'stack',
    'name' => 'channel-name',
    'channels' => ['single', 'slack'],
],
```

### 사용가능한 채널 드라이버
---
이름	설명
stack	"다중 채널"을 생성할 수 있는 채널
single	하나의 파일이나 경로 기반 로거 채널(StreamHandler)
daily	Monolog 드라이버를 기반으로 한 일별 로테이션을 하는 RotatingFileHandler
slack	Monolog 드라이버를 기반으로 한 SlackWebhookHandler
syslog	Monolog 드라이버를 기반으로 한 SyslogHandler
errorlog	Monolog 드라이버를 기반으로한 ErrorLogHandler
monolog	지원가능한 Monolog 핸들러를 사용하는 Molog 팩토리 드라이버
custom	지정된 팩토리를 호출해서 채널을 생성하는 커스텀 드라이버

{tip} 고급 채널 커스터마이징에 대한 매뉴얼을 확인하여 monolog 와 custom 드라이버에 대해서 자세히 알아보십시오.

### 슬랙 채널 설정하기
---
slack 채널은 url 옵션을 필요로 합니다. URL은 팀에서 지정한 슬랙 incoming webhook URL과 일치해야 합니다.


## 로그 스택 구성하기
---
앞서 말한바와 같이, stack 드라이버는 여러개의 채널을 하나의 로그 채널로 묶어 줍니다. 로그 스택을 사용하는 방법을 설명하기 위해 프로덕션 애플리케이션에서 확인할 수 있는 설정 예를 살펴 보겠습니다:

```php
'channels' => [
    'stack' => [
        'driver' => 'stack',
        'channels' => ['syslog', 'slack'],
    ],

    'syslog' => [
        'driver' => 'syslog',
        'level' => 'debug',
    ],

    'slack' => [
        'driver' => 'slack',
        'url' => env('LOG_SLACK_WEBHOOK_URL'),
        'username' => 'Laravel Log',
        'emoji' => ':boom:',
        'level' => 'critical',
    ],
],
```

위의 설정을 자세히 확인해보겠습니다. 먼저, stack 채널이 channels 옵션의 syslog 와 slack 두개의 다른 채널을 묶어 줍니다. 따라서 메세지를 로깅할 때에는 두개의 채널 모두에게 로그를 기록합니다.

### 로그 레벨
---
앞의 예제에서 확인한 syslog 와 slack 채널 설정에 있는 level 옵션을 확인해 보십시오. 이 옵션은 채널에서 로그가 기록되어야 하는 최소 "레벨"을 결정합니다. 라라벨의 로그 서비스를 제공하는 Monolog는 RFC 5424 표준 스펙에 정의된 모든 emergency, alert, critical, error, warning, notice, info, debug 레벨을 지원합니다.

따라서, debug 메소드를 사용하여 로그를 기록하는 것을 생각해 보겠습니다: :

```php
Log::debug('An informational message.');
```

주어진 설정에 따라서 syslog 채널은 전달된 메세지를 시스템 로그파일에 기록합니다. 그렇지만 슬랙 채널에는 에러 메세지가 critical 이상으로 설정되어 있기 때문에, 메세지가 전달되지 않습니다. emergency 메세지를 로깅 하려고 할때에는, 두 채널에 설정된 최소 로그 레벨이 emergency 레벨 보다 낮기 때문에, 두 채널 모두에서 메세지가 전달됩니다:

```php
Log::emergency('The system is down!');
```

