
## 문자열


### __()
__ 함수는 주어진 다국어 문자열 또는 다국어 키를 다국어 파일을 사용하여 변환합니다:

```php
echo __('Welcome to our application');

echo __('messages.welcome');
```

지정된 다국어 문자열이나 키가 존재하지 않는 경우, __ 함수는 주어진 값을 그대로 반환합니다. 따라서 예제와 같이 __ 함수는 다국어 키가 존재하지 않는다면 messages.welcome를 그대로 반환합니다.


### camel_case()
camel_case 함수는 주어진 문자열을 camelCase 형태로 변환합니다:

```php
$converted = camel_case('foo_bar');

// fooBar
```

### class_basename()
class_basename은 클래스의 네임스페이스를 제거한 클래스의 클래스 명을 반환합니다:

```php
$class = class_basename('Foo\Bar\Baz');

// Baz
```

### e()
e 함수는 주어진 문자열에 PHP의 htmlspecialchars 함수를 duuble_encode 옵션이 기본적으로 true 인 형태로 실행한 결과를 반환합니다:

```php
echo e('<html>foo</html>');

// &lt;html&gt;foo&lt;/html&gt;
```

### ends_with()
ends_with 함수는 주어진 문자열이 특정 값으로 끝나는지 알아냅니다:

```php
$result = ends_with('This is my name', 'name');

// true
```

### kebab_case()
kebab_case 함수는 주어진 문자열을 kebab-case로 변환합니다: (역자주 : 단어와 단어를 '-'로 연결한 형태)

```php
$converted = kebab_case('fooBar');

// foo-bar
```

### preg_replace_array()
preg_replace_array 함수는 주어진 패턴에 맞는 문자열을 순차적으로 배열으로 교체합니다:

```php
$string = 'The event will take place between :start and :end';

$replaced = preg_replace_array('/:[a-z_]+/', ['8:30', '9:00'], $string);

// The event will take place between 8:30 and 9:00
```

### snake_case()
snake_case 함수는 주어진 문자열을 snake_case 형태로 변환합니다:

```php
$converted = snake_case('fooBar');

// foo_bar
```

### starts_with()
starts_with 함수는 문자열이 주어진 문자열으로 시작하는지 판별합니다:

```php
$result = starts_with('This is my name', 'This');

// true
```

### str_after()
str_after 함수는 문자열에서 주어진 문자열 다음의 모든 값을 반환합니다:

```php
$slice = str_after('This is my name', 'This is');

// ' my name'
```

### str_before()
str_before 함수는 문자열에 주어진 문자열 이전의 모든 값을 반환합니다:

```php
$slice = str_before('This is my name', 'my name');

// 'This is '
```

### str_contains()
str_contains 함수는 주어진 문자열이 특정 문자열을 포함하는지 판별합니다 (대소문자를 구분합니다):

```php
$contains = str_contains('This is my name', 'my');

// true
```

또한 주어진 문자열이 특정 문자열을 포함하고 있는지 판별하기 위한 배열을 전달할 수도 있습니다:

```php
$contains = str_contains('This is my name', ['my', 'foo']);

// true
```

### str_finish()
str_finish 함수는 문자열이 주어진 값으로 끝나지 않는다면 해당 값을 추가합니다:

```php
$adjusted = str_finish('this/string', '/');

// this/string/

$adjusted = str_finish('this/string/', '/');

// this/string/
```

### str_is()
str_is 함수는 주어진 문자열이 주어진 패턴과 대응되는지 확인합니다. 와일드카드를 표시하기 위해 별표를 사용할 수 있습니다:

```php
$matches = str_is('foo*', 'foobar');

// true

$matches = str_is('baz*', 'foobar');

// false
```

### str_limit()
str_limit 함수는 주어진 문자열을 지정된 길이로 제한합니다:

```php
$truncated = str_limit('The quick brown fox jumps over the lazy dog', 20);

// The quick brown fox...
```

변경될 문자열의 마지막에 덧붙일 문자열을 세번째 인자로 전달할 수도 있습니다:

```php
$truncated = str_limit('The quick brown fox jumps over the lazy dog', 20, ' (...)');

// The quick brown fox (...)

Str::orderedUuid()
```

Str::orderedUuid 메소드는 인덱싱된 데이터베이스 컬럼에 효과적으로 저장될 수 있도록 "타임스탬프와 같은 정렬이 가능한" UUID를 생성합니다:

(역자주 : 이 헬퍼함수의 결과로 생성되는 UUID의 첫번째 부분은 시간에 따라서 증감하는 형태를 보입니다)

```php
use Illuminate\Support\Str;

return (string) Str::orderedUuid();
```

### str_plural()
str_plural 함수는 문자열을 복수형태로 변환합니다. 이 함수는 현재 영어에만 적용 가능합니다:

```php
$plural = str_plural('car');

// cars

$plural = str_plural('child');

// children
```

문자열의 단일 혹은 복수 형태를 조회하기 위해서, 함수의 두번째 인자로 정수를 전달할 수 있습니다:

```php
$plural = str_plural('child', 2);

// children

$plural = str_plural('child', 1);

// child
```

### str_random()
str_random 함수는 지정된 길이의 문자열을 무작위로 생성합니다. 이 함수는 PHP의 random_bytes 함수를 사용합니다:

```php
$random = str_random(40);
```

### str_replace_array()
str_replace_array 함수는 주어진 값을 순차적으로 배열값으로 치환합니다:

```php
$string = 'The event will take place between ? and ?';

$replaced = str_replace_array('?', ['8:30', '9:00'], $string);

// The event will take place between 8:30 and 9:00
```

### str_replace_first()
str_replace_first 함수는 문자열에서 주어진 값이 발견된 첫번째 부분을 교체합니다:

```php
$replaced = str_replace_first('the', 'a', 'the quick brown fox jumps over the lazy dog');

// a quick brown fox jumps over the lazy dog
```

### str_replace_last()
str_replace_last 함수는 문자열에서 주어진 값이 발견된 마지막 부분을 교체합니다:

```php
$replaced = str_replace_last('the', 'a', 'the quick brown fox jumps over the lazy dog');

// the quick brown fox jumps over a lazy dog
```

### str_singular()
str_singular 함수는 문자열을 단수 형태로 변환합니다. 이 함수는 현재 영어에만 적용 가능합니다:

```php
$singular = str_singular('cars');

// car

$singular = str_singular('children');

// child
```

### str_slug()
str_slug 함수는 주어진 문자열로부터 URL에 알맞은 "slug"를 생성합니다:

```php
$slug = str_slug('Laravel 5 Framework', '-');

// laravel-5-framework
```

### str_start()
str_start 함수는 문자열이 주어진 값으로 시작하지 않은 경우에 이를 추가합니다:

```php
$adjusted = str_start('this/string', '/');

// /this/string

$adjusted = str_start('/this/string/', '/');

// /this/string
```

### studly_case()
studly_case 함수는 주어진 문자열을 StudlyCase 형태로 변환합니다:

```php
$converted = studly_case('foo_bar');

// FooBar
```

### title_case()
title_case 함수는 주어진 문자열을 Title Case 로 변환합니다(단어별로 앞글자를 대문자, 단어 사이를 공백이 포함되는 형태):

```php
$converted = title_case('a nice title uses the correct case');

// A Nice Title Uses The Correct Case
```

### trans()
trans 함수는 다국어 파일을 이용하여 주어진 다국어 키를 변환합니다:

```php
echo trans('messages.welcome');
```

지정된 다국어 키가 존재하지 않는다면, trans 함수는 주어진 키를 반환합니다. 따라서 예제에서 다국어 키가 존재하지 않는다면 trans 함수는 messages.welcome를 그대로 반환합니다.


### trans_choice()
trans_choice 함수는 주어진, 수량(단수, 복수 처리)을 이용하여 주어진 다국어 키를 번역합니다:

```php
echo trans_choice('messages.notifications', $unreadCount);
```

지정된 다국어 키가 존재하지 않는다면, trans_choice 함수는 주어진 키를 반환합니다. 따라서 예제에서 다국어 키가 존재하지 않는다면 trans_choice 함수는 messages.notifications를 그대로 반환합니다.


### Str::uuid()
Str::uuid 메소드는 UUID (version 4)를 생성합니다:

```php
use Illuminate\Support\Str;

return (string) Str::uuid();
```
