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
HTTP 세션
소개하기
설정하기
드라이버 사전준비사항
세션 사용하기
데이터 조회하기
데이터 저장하기
데이터 임시저장하기
데이터 삭제하기
세션 ID 다시 생성하기
사용자 정의 세션 드라이버 추가하기
드라이버 구현하기
드라이버 등록하기

## 소개하기
---

HTTP 기반의 애플리케이션은 상태를 저장할수 없기 때문에, HTTP 여러 요청들에 관계없이 사용자의 정보를 저장하기위해서 세션이 사용됩니다. 라라벨은 풍부한 표현이 가능하며 일관된 API를 통해서 엑세스 되는 다양한 세션 백엔드를 제공합니다. 별다른 설정 없이도, 많이 알려진 Memcached, Redis 그리고 데이터베이스를 지원합니다.


## 설정하기
---
세션의 설정파일은 config/session.php로 저장되어 있습니다. 이 파일에서 사용 가능한 옵션을 잘 살펴보십시오. 기본적으로 라라벨은 대부분의 애플리케이션에서 잘 작동 할 수 있도록 file 세션 드라이버를 사용하도록 설정되어 있습니다. 실서비스용 애플리케이션에서는 보다 나은 세션 처리 성능을 위해서 memcached 또는 redis 드라이버를 사용하는 것을 고려하십시오.

세션 driver 설정 옵션은 각각의 요청-request에 따른 세션을 어디에 저장할 것인지 정의합니다. 라라벨은 별다른 설정 없이도 다양한 드라이버를 제공합니다.

file - storage/framework/sessions 디렉토리에 세션을 저장합니다.
cookie - 암호화된 쿠키를 사용하여 안전하게 세션을 저장할 것입니다.
database - 세션이 관계형 데이터베이스에 저장된다.
memcached / redis - 빠르고, 캐시를 기반으로한 memcached, redis 에 저장합니다.
array - 세션은 PHP 배열에 저장되며 지속되지 않습니다.
{tip} array 드라이버는 테스트가 진행되는 동안 사용되고, 세션이 지속적으로 유지되지 않습니다.


## 드라이버의 사전준비사항
---
### 데이터베이스
database 세션 드라이버를 사용하는 경우, 세션을 저장할 수 있는 테이블을 생성할 필요가 있습니다. 다음의 Schema 예제를 통해서 테이블을 생성할 수 있습니다:

```php
Schema::create('sessions', function ($table) {
    $table->string('id')->unique();
    $table->unsignedInteger('user_id')->nullable();
    $table->string('ip_address', 45)->nullable();
    $table->text('user_agent')->nullable();
    $table->text('payload');
    $table->integer('last_activity');
});
```
session:table 아티즌 명령어를 통해서 이 마이그레이션을 생성할 수 있습니다:

```php
php artisan session:table

php artisan migrate
```

## Redis
---

라라벨에서 Redis 세션을 사용하기 전에, Composer 를 통해서 predis/predis 패키지 (~1.0)을 설치할 필요가 있습니다. database 설정 파일 안에서 Redis 커넥션을 설정할 수 있습니다. session 설정 파일에서 connection 옵션은 세션이 어떤 Redis 커넥션을 사용할지 지정하는데 사용될 수 있습니다.



