## 경로

### app_path()
app_path 함수는 app 디렉토리에 대한 절대 경로를 반환합니다. app_path 함수를 사용하면 애플리케이션 디렉토리에서 특정 파일의 절대 경로를 생성할 수도 있습니다:

```php
$path = app_path();

$path = app_path('Http/Controllers/Controller.php');
```

### base_path()
base_path 함수는 프로젝트의 루트 디렉토리에 대한 절대 경로를 반환합니다. base_path 함수를 사용하여 프로젝트 루트 디렉토리에 대한 특정 파일의 절대 경로를 생성할 수도 있습니다:

```php
$path = base_path();

$path = base_path('vendor/bin');
```

### config_path()
config_path 함수는 애플리케이션의 config 디렉토리에 대한 절대 경로를 반환합니다. config_path 함수를 사용하여, 애플리케이션의 설정 디렉토리 안에 있는 파일에 대한 절대 경로를 생성할 수도 있습니다:

```php
$path = config_path();

$path = config_path('app.php');
```

### database_path()
database_path 함수는 애플리케이션의 database 디렉토리에 대한 절대 경로를 반환합니다. database_path 함수를 사용하여, 애플리케이션의 데이터베이스 디렉토리 안에 있는 파일에 대한 절대 경로를 생성할 수도 있습니다:

```php
$path = database_path();

$path = database_path('factories/UserFactory.php');
```

### mix()
mix 함수는 버전이 지정된 Mix 파일에 대한 경로를 반환합니다:

```php
$path = mix('css/app.css');
```

### public_path()
public_path 함수는 public 디렉토리에 대한 절대경로를 반환합니다. public_path 함수를 사용하여, 애플리케이션의 public 디렉토리 안에 있는 파일에 대한 절대 경로를 생성할 수도 있습니다:

```php
$path = public_path();

$path = public_path('css/app.css');
```

### resource_path()
resource_path 함수는 resources 디렉토리에 대한 절대경로를 반환합니다. resource_path 함수를 사용하여 리소스 디렉토리 안에 있는 파일에 대한 절대 경로를 생성할 수도 있습니다:

```php
$path = resource_path();

$path = resource_path('assets/sass/app.scss');
```

### storage_path()
storage_path 함수는 storage 디렉토리에 대한 절대경로를 반환합니다. storage_path 함수를 사용하여 스토리지 디렉토리 안에 있는 파일에 대한 절대 경로를 생성할 수도 있습니다:

```php
$path = storage_path();

$path = storage_path('app/file.txt');
```
