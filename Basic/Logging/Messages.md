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

## 로그 메세지 작성하기
---
Log 파사드를 사용하여 로그 메세지를 작성할 수 있습니다. 앞에서 언급했듯이, 로그는 RFC 5424 스펙에 정의된 8가지 로그 레벨 emergency, alert, critical, error, warning, notice, info, debug 을 제공합니다:

```php
Log::emergency($message);
Log::alert($message);
Log::critical($message);
Log::error($message);
Log::warning($message);
Log::notice($message);
Log::info($message);
Log::debug($message);
```

따라서 로그 메세지 기록할 때 일치하는 레벨에 맞는 메소드를 호출하면 됩니다. 기본적으로 메세지는 config/logging.php 설정 파일에 정의된 로그 채널로 기록됩니다:

```php
<?php

namespace App\Http\Controllers;

use App\User;
use Illuminate\Support\Facades\Log;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return Response
     */
    public function showProfile($id)
    {
        Log::info('Showing user profile for user: '.$id);

        return view('user.profile', ['user' => User::findOrFail($id)]);
    }
}
```

### 상태 정보를 추가하기
---
로그 메소드에 원하는 상태 정보를 배열로 전달할 수 있습니다. 이 데이터는 로그 메세지와 함께 출력됩니다:

```php
Log::info('User failed to login.', ['id' => $user->id]);
```

### 채널을 지정하여 로그 기록하기
---
때로는 애플리케이션의 기본 채널이 아닌, 다른 채널을 지정하여 로그를 남기길 원할 수도 있습니다. Log 파사드의 `channel 메소드를 사용하면, 설정 파일에 정의된 채널을 찾아서 로그를 작성합니다:

```php
Log::channel('slack')->info('Something happened!');
```

로그를 작성할 때 임시로 여러 채널을 묶은 로그 스택을 구성하려면 다음과 같이 stack 메소드를 사용하면 됩니다:

```php
Log::stack(['single', 'slack'])->info('Something happened!');
```

