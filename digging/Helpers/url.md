
## URLs

### action()
action 함수는 주어진 컨트롤러 메소드로 URL을 생성합니다. 컨트롤러의 전체 네임스페이스를 전달하지 않아도 됩니다. 대신, App\Http\Controllers 네임스페이스에 따른 컨트롤러 클래스 이름을 전달하면 됩니다.:

```php
$url = action('HomeController@index');
```

메소드가 라우트 파라미터를 받아들인다면, 두번째 인자로 메소드에 전달하십시오:

```php
$url = action('UserController@profile', ['id' => 1]);
```

### asset()
asset 함수는 HTTP요청의 현재 scheme(HTTP나 HTTPS)을 이용하여 asset을 사용하기 위한 URL을 생성합니다:

```php
$url = asset('img/photo.jpg');
```

### secure_asset()
secure_asset 함수는 HTTPS를 이용하여 asset을 사용하기 위한 URL을 생성합니다:

```php
$url = secure_asset('img/photo.jpg');
```

### route()
route 함수는 주어진 라우트 이름으로 URL을 생성합니다:

```php
$url = route('routeName');
```

라우트가 파라미터를 가진다면 파라미터를 두번째 인자로 메소드에 전달하세요:

```php
$url = route('routeName', ['id' => 1]);
```
기본적으로 route 함수는 절대경로 URL을 생성합니다. 만약 상태경로의 URL을 생성하려면 세번째 인자를 false로 전달하면 됩니다:

```php
$url = route('routeName', ['id' => 1], false);
```

### secure_url()
secure_url 함수는 주어진 경로에 대한 전체 HTTPS URL을 생성합니다:

```php
$url = secure_url('user/profile');

$url = secure_url('user/profile', [1]);
```

### url()
url 함수는 주어진 경로에 대한 전체 URL을 생성합니다:

```php
$url = url('user/profile');

$url = url('user/profile', [1]);
```

경로를 전달하지 않으면, Illuminate\Routing\UrlGenerator 인스턴스가 반환됩니다:

```php
$current = url()->current();

$full = url()->full();

$previous = url()->previous();
```