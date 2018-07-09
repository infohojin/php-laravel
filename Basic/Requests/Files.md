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

## 파일처리
---
### 업로드된 파일 조회하기
Illuminate\Http\Request 인스턴스에서 file 메소드를 사용하거나 동적 속성을 사용하여 업로드된 파일에 엑세스 할 수 있습니다. file 메소드는 PHP SplFileInfo클래스를 상속한 Illuminate\Http\UploadedFile 클래스의 인스턴스를 반환하고 파일과 상호작용할 수 있는 다양한 메소드를 제공합니다:

```php
$file = $request->file('photo');

$file = $request->photo;
```

hasFile 메소드를 사용하면 request에 파일을 가지고 있는지 확인할 수 있습니다:

```php
if ($request->hasFile('photo')) {
    //
}
```

## 성공적으로 업로드 되었는지 확인하기
---
파일이 현재 존재하는지 확인하는데 더하여, isValid 메소드를 사용하여 업로드된 파일에 아무런 문제가 없는지 확인할 수 있습니다:

```php
if ($request->file('photo')->isValid()) {
    //
}
```

## 파일 경로 & 확장자
---
UploadedFile 클래스는 파일의 전체 경로와 확장자에 엑세스 할 수 있는 메소드를 가지고 있습니다. extension 메소드는 그 내용에 따라 파일의 확장자를 추측하려고 시도합니다. 이 확장자는 클라이언트가 제공 한 확장자와는 다를 수 있습니다:

```php
$path = $request->photo->path();

$extension = $request->photo->extension();
```

## 기타 파일 관련 메소드들
---
UploadedFile 인스턴스에 다양한 다른 메소드들이 제공되어 있습니다. 이 메소드들에 대해 더 많은 정보를 얻으려면 클래스의 API documentation을 확인해보십시오.


## 업로드된 파일 저장하기
---
업로드된 파일을 저장하려면, 일반적으로 설정된 파일시스템중 하나를 사용합니다. UploadedFile 클래스는 업로드된 파일을 로컬 파일 시스템이나 아마존 S3 와 같은 클라우드 스토리지 디스크 중에 하나로 이동 시킬 수 있는 store 메소드를 가지고 있습니다.

store 메소드는 파일 시스템에 구성된 루트 디렉토리로 부터 파일이 어디에 저장되어야 할지에 대한 경로를 전달 받습니다. 파일의 이름은 자동으로 고유한 ID로 생성되므로 이 경로에는 파일 이름을 포함하지 않아야 합니다.

store 메소드는 선택적으로 파일이 저장되는데 사용될 디스크 이름을 두번째 인자로 전달 받습니다. 메소드는 디스크의 루트를 기준으로 파일의 경로를 반환합니다:

```php
$path = $request->photo->store('images');

$path = $request->photo->store('images', 's3');
```

파일 이름이 자동으로 생성되지 않기를 원한다면, 경로와, 파일이름 그리고 디스크 이름을 인자로 받아 들이는 storeAs 메소드를 사용할 수 있습니다:

```php
$path = $request->photo->storeAs('images', 'filename.jpg');

$path = $request->photo->storeAs('images', 'filename.jpg', 's3');
```

