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

## 사용가능한 유효성 검사 규칙
---
다음은 사용 가능한 모든 유효성 검사 규칙과 그 기능의 목록입니다:

Accepted
Active URL
After (Date)
After Or Equal (Date)
Alpha
Alpha Dash
Alpha Numeric
Array
Bail
Before (Date)
Before Or Equal (Date)
Between
Boolean
Confirmed
Date
Date Equals
Date Format
Different
Digits
Digits Between
Dimensions (Image Files)
Distinct
E-Mail
Exists (Database)
File
Filled
Greater Than
Greater Than Or Equal
Image (File)
In
In Array
Integer
IP Address
JSON
Less Than
Less Than Or Equal
Max
MIME Types
MIME Type By File Extension
Min
Not In
Not Regex
Nullable
Numeric
Present
Regular Expression
Required
Required If
Required Unless
Required With
Required With All
Required Without
Required Without All
Same
Size
String
Timezone
Unique (Database)
URL

accepted
필드의 값이 yes, on, 1, 또는 _true_이어야 합니다. 이 것은 "이용약관" 동의와 같은 필드의 검사에 유용합니다.


active_url
필드의 값이 PHP 함수 dns_get_record에서 확인 가능한 올바른 A 또는 AAAA 레코드여야 합니다.


after:date
필드의 값이 주어진 날짜 이후여야 합니다. 이때 날짜는 strtotime PHP 함수를 통해 생성된 값입니다.

'start_date' => 'required|date|after:tomorrow'
strtotime에 의해 계산될 날짜 문자열을 전달하는 대신 날짜와 비교할 다른 필드를 명시할 수 있습니다:

'finish_date' => 'required|date|after:start_date'

after_or_equal:date
필드의 값이 주어진 날짜와 동일하거나, 이후여야 합니다. 보다 자세한 사항은 after 규칙을 확인하십시오.


alpha
필드의 값이 완벽하게 (숫자나 기호가 아닌) 알파벳[자음과 모음] 문자로 이루어져야 합니다.

(역자주: 영문 알파벳만을 의미하지 않고, 숫자나 기호가 아닌경우에 해당하여, 한글도 허용합니다.)


alpha_dash
필드의 값이 (숫자나 기호가 아닌) 알파벳[자음과 모음] 문자 및 숫자와 dash(-), underscore(_)로 이루어져야 합니다.


alpha_num
필드의 값이 완벽하게 (숫자나 기호가 아닌) 알파벳[자음과 모음] 문자 및 숫자로 이루어져야 합니다.


array
필드의 값이 반드시 PHP 배열 형태이어야 합니다.


bail
첫번째 유효성 검사 룰이 실패하면, 유효성 검사를 중단합니다.


before:date
필드의 값이 반드시 주어진 날짜보다 앞서야 합니다. 날짜는 strtotime PHP 함수를 통해 비교됩니다.


before_or_equal:date
필드의 값이 주어진 날짜보다 앞서거나, 같아야 합니다. 날짜는 strtotime PHP 함수를 통해서 비교됩니다.


between:min,max
필드의 값이, 주어진 min 과 _max_의 사이의 값이어야 합니다. 문자열, 숫자, 그리고 파일이 size 룰에 의해 같은 방식으로 계산될 수 있습니다.


boolean
필드의 값이 반드시 boolean으로 캐스팅될 수 있어야 합니다. 허용되는 값은 true, false, 1, 0, "1", "0" 입니다.


confirmed
필드의 값이 foo_confirmation의 매칭되는 필드를 가져야 합니다. 예를 들어 만약 필드가 password라면, password_confirmation라는 필드가 입력값 중에 있어야 합니다.


date
필드의 값이 strtotime PHP 함수에서 인식할 수 있는 올바른 날짜여야 합니다.


date_equals:date
필드의 값이 주어진 날짜와 일치해야 합니다. 날짜값은 PHP의 strtotime 함수로 전달됩니다.


date_format:format
필드의 값이 반드시 주어진 _format_과 일지해야 합니다. 필드의 유효성을 검사할 때에는 date와 date_format 중 하나만 사용해야 합니다


different:field
필드의 값이 주어진 _field_의 값과 달라야 합니다.


digits:value
필드의 값이 반드시 _숫자_여야 하고, 길이가 _value_이어야 합니다.


digits_between:min,max
필드의 값이 주어진 _min_과 max 사이의 길이를 가져야 합니다.


dimensions
필드의 값이 룰에 지정된 파라미터들을 만족하는 이미지이어야 합니다.

'avatar' => 'dimensions:min_width=100,min_height=200'
사용가능한 제약은: min_width, max_width, min_height, max_height, width, height, ratio 입니다.

ratio 제약은 가로를 세로로 나눈 비율을 표현해야합니다. 이는 3/2 또는 소수점 1.5 처럼 지정될 수 있습니다:

'avatar' => 'dimensions:ratio=3/2'
이 룰은 여러개의 인자를 필요로 하는데, 다음처럼 Rule:dimensions 메소드를 사용하여 유연하게 룰을 생성할 수 있습니다:

```php
use Illuminate\Validation\Rule;

Validator::make($data, [
    'avatar' => [
        'required',
        Rule::dimensions()->maxWidth(1000)->maxHeight(500)->ratio(3 / 2),
    ],
]);
```

distinct
배열에서 동작하며, 필드의 값이 배열안의 다른 값과 중복되지 않아야 합니다.

```php
'foo.*.id' => 'distinct'
```

email
필드의 값이 이메일 주소 형식이어야 합니다.


exists:table,column
필드의 값이 주어진 데이터베이스 테이블에 존재하는 값이어야 합니다.

exists 룰의 기본 사용법

```php
'state' => 'exists:states'
```

특정 컬럼명 지정하기

```php
'state' => 'exists:states,abbreviation'
```

때때로, exists 쿼리에서 사용할 데이터베이스 커넥션을 지정하고자 할 수도 있습니다. 이경우 커넥션의 이름을 테이블 이름 앞에 "점" 문법을 사용하여 표기할 수도 있습니다.

```php
'email' => 'exists:connection.staff,email'
```

유효성 검사 규칙에 의해서 실행되는 쿼리를 커스터마이징 하고자 한다면, 규칙에 Rule 클래스를 정의해서 사용할 수 있습니다. 다음 예제에서 | 문자를 구분자로 사용하는 대신에 유효성 검사 규칙을 배열로 지정하고 있습니다:

```php
use Illuminate\Validation\Rule;

Validator::make($data, [
    'email' => [
        'required',
        Rule::exists('staff')->where(function ($query) {
            $query->where('account_id', 1);
        }),
    ],
]);
```

file
필드의 값이 완전히 업로드된 파일이어야 합니다.


filled
필드가 존재하는 경우 값이 비어있으면 안됩니다.


gt:field
필드의 값이 주어진 다른 필드의 값보다 커야합니다. 두개의 필드는 동일한 타입이어야 하며, 문자열, 숫자형, 배열 그리고 파일 타입은 size 룰에 따라서 계산됩니다.


gte:field
필드의 값이 주어진 다른 필드의 값보다 크거나 같아야합니다. 두개의 필드는 동일한 타입이어야 하며, 문자열, 숫자형, 배열 그리고 파일 타입은 size 룰에 따라서 계산됩니다.


image
이미지 파일(jpeg, png, bmp, gif, svg)이어야 합니다.


in:foo,bar,...
필드의 값이 주어진 목록에 포함돼 있어야 합니다. 이 룰은 자주 배열을 implode 하는 것을 필요로 하며, 다음처럼 Rule:in 메소드를 사용하여 편리하게 생성할 수 있습니다:

```php
use Illuminate\Validation\Rule;

Validator::make($data, [
    'zones' => [
        'required',
        Rule::in(['first-zone', 'second-zone']),
    ],
]);
```

in_array:anotherfield
필드의 값이 주어진 다른 필드의 값안에 존재해야만 합니다.


integer
필드의 값이 정수여야 합니다.


ip
필드의 값이 IP 주소여야 합니다.

ipv4
필드의 값이 IPv4 주소여야 합니다.

ipv6
필드의 값이 IPv6 주소여야 합니다.


json
필드의 값이 유효한 JSON 문자열이어야 합니다.


lt:field
The field under validation must be less than the given field. The two fields must be of the same type. Strings, numerics, arrays, and files are evaluated using the same conventions as the size rule.

필드의 값이 주어진 다른 필드의 값보다 작아야 합니다. 두개의 필드는 동일한 타입이어야 하며, 문자열, 숫자형, 배열 그리고 파일 타입은 size 룰에 따라서 계산됩니다.


lte:field
The field under validation must be less than or equal to the given field. The two fields must be of the same type. Strings, numerics, arrays, and files are evaluated using the same conventions as the size rule.

필드의 값이 주어진 다른 필드의 값보다 적거나 같아야 합니다. 두개의 필드는 동일한 타입이어야 하며, 문자열, 숫자형, 배열 그리고 파일 타입은 size 룰에 따라서 계산됩니다.


max:value
필드의 값이 반드시 _value_보다 작거나 같아야 합니다. 문자열, 숫자, 그리고 파일이 size 룰에 의해 같은 방식으로 평가될 수 있습니다.


mimetypes:text/plain,...
파일이 주어진 MIME 타입중 하나와 일치해야 합니다:

```php
'video' => 'mimetypes:video/avi,video/mpeg,video/quicktime'
```

업로드 파일의 MIME 타입을 지정하고자 한다면, 프레임 워크는 파일의 내용을 읽어 들여 MIME 타입을 추측하게 되며 클라이언트가 제공하는 MIME 타입과 달라질 수 있습니다.


mimes:foo,bar,...
파일의 MIME 타입이 주어진 확장자 리스트 중에 하나와 일치해야 합니다.

MIME 룰의 기본 사용법

```php
'photo' => 'mimes:jpeg,bmp,png'
```
여러분은 확장자를 지정하기만 하면 되지만, 이 경우 파일의 컨텐트를 읽고 MIME 타입을 추정함으로써 이 파일의 MIME의 유효성을 검사합니다.

MIME 타입과 그에 상응하는 확장의 전체 목록은 다음의 위치에서 확인하실 수 있습니다: https://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types


min:value
필드의 값이 반드시 value 보다 크거나 같아야 합니다. 문자열, 숫자, 그리고 파일이 size 룰에 의해 같은 방식으로 평가될 수 있습니다.


nullable
필드의 값은 null 일 수 있습니다. 이는 특히 null 을 포함한 문자열이나 정수형과 같은 기본 타입에 대해서 유효성검사를 할 때 유용합니다.


not_in:foo,bar,...
필드의 값이 주어진 목록에 존재하지 않아야 합니다. Rule::notIn 메소드는 검사룰을 보다 유연하게 구성하는데 사용할 수 있습니다:

```php
use Illuminate\Validation\Rule;

Validator::make($data, [
    'toppings' => [
        'required',
        Rule::notIn(['sprinkles', 'cherries']),
    ],
]);
```

not_regex:pattern
필드의 값이 주어진 정규표현식과 매칭되지 않는 것을 확인합니다.

Note: regex / not_regex 패턴을 사용할 때, 특히 정규표현식에 파이프 문자가 포함 된 경우, 파이프 구분자를 사용하는 대신에 배열에 룰을 지정해야 할 수 있습니다.


nullable
필드의 값이 null 일 수 있습니다. 이것은 특히 null 값을 포함 할 수 있는 문자열 및 정수형과 같은 프리미티브 타입의 유효성을 검사 할 때 유용합니다.


numeric
필드의 값이 숫자여야 합니다.


present
필드가 존재하고 있는지 확인하지만, 값이 비어있을 수 있습니다.


regex:pattern
필드의 값이 주어진 정규식 표현과 일치해야 합니다.

참고: regex / not_regex 패턴을 사용할 때, 특히 정규 표현식에 파이프 문자열이 있다면, 파이프 구분자를 사용하는 대신 배열 형식을 사용하여 룰을 지정할 필요가 있습니다.


required
입력 값 중에 해당 필드가 존재해야 하며 비어 있어서는 안됩니다. 필드는 다음의 조건 중 하나를 충족하면 "빈(empty)" 것으로 간주됩니다:

값이 null인 경우.
값이 비어있는 문자열인 경우.
값이 비어있는 배열이거나, 비어있는 Countable 객체인경우
값이 경로없이 업로드된 파일인 경우

required_if:anotherfield,value,...
만약 _anotherfield_의 값이 _value_중의 하나와 일치한다면, 해당 필드는 존재하고 비어있지 않아야 합니다.


required_unless:anotherfield,value,...
_anotherfield_가 어떤 _value_와도 값이 일치하지 않다면 해당 필드는 존재하고 비어있지 않아야 합니다.


required_with:foo,bar,...
지정된 다른 필드중 하나라도 존재한다면, 해당 필드가 반드시 존재하고 비어있지 않아야 합니다.


required_with_all:foo,bar,...
지정된 다른 필드가 모두 존재한다면, 해당 필드가 반드시 존재하고 비어있지 않아야 합니다.


required_without:foo,bar,...
지정된 다른 필드중 하나라도 존재하지 않으면, 해당 필드가 반드시 존재하고 비어있지 않아야 합니다.


required_without_all:foo,bar,...
지정된 다른 필드들이 모두 존재하지 않으면, 해당 필드가 존재하고 비어있지 않아야 합니다.


same:field
필드의 값이 주어진 _field_의 값과 일치해야 합니다.


size:value
필드의 값이 주어진 _value_와 일치하는 크기를 가져야 합니다. 문자열 데이터에서는 문자의 개수가 _value_와 일치해야 합니다. 숫자형식의 데이터에서는 주어진 정수값이 _value_와 일치해야 합니다. 배열에서는 배열의 count 와 일치해야 합니다. 파일에서는 킬로바이트 형식의 파일 사이즈가 _size_와 일치해야 합니다.


string
필드의 값이 반드시 문자열이어야 합니다. 필드가 null 인것을 허용하려면 규칙에 nullable 을 할당해야만 합니다.


timezone
필드의 값이 timezone_identifiers_list PHP 함수에서 인식 가능한 유효한 timezone 식별자여야 합니다.


unique:table,column,except,idColumn
필드의 값이 주어진 데이터베이스 테이블에서 고유한 값이어야 합니다. 만약 column이 지정돼 있지 않다면 필드의 이름이 사용됩니다.

특정 컬럼명 지정하기:

```
'email' => 'unique:users,email_address'
```

특정 데이터베이스 커넥션

때때로, 여러분은 Validator에 의해서 생성되는 데이터베이스 쿼리에 사용자가 지정한 커넥션을 필요로 할지도 모릅니다. 위에서의 검증 규칙 unique:users 에서는 데이터베이스를 쿼리하기 위해 기본 데이터 베이스 커넥션이 사용됩니다. 이를 오버라이드 하려면 테이블 이름 후에 "." 표기법으로 커넥션을 지정하십시오:

'email' => 'unique:connection.users,email_address'
주어진 ID에 대해서 유니크 규칙을 무시하도록 강제하기:

때때로 유니크 검사를 할 때 특정 ID를 무시하고자 할 수 있습니다. 예를 들어 사용자 이름, 이메일 주소 그리고 위치를 포함하는 "프로필 업데이트" 화면이 있습니다. 물론 이메일 주소가 고유하다는 것을 확인하고 싶을 것입니다. 하지만 사용자가 이름 필드만 바꾸고 이메일 필드를 바꾸지 않는다면 사용자가 이미 이메일 주소의 주인이기 때문에 유효 검사 오류가 던져지지 않아야 합니다.

사용자 ID를 무시하도록 지시하려면, 규칙을 유연하게 정의할 수 있는 Rule 클래스를 사용하면 됩니다. 다음 예제에서 규칙을 | 문자를 구분자로 사용하는 대신에 유효성 검사 규칙을 배열로 지정하고 있습니다:

```php
use Illuminate\Validation\Rule;

Validator::make($data, [
    'email' => [
        'required',
        Rule::unique('users')->ignore($user->id),
    ],
]);
```

테이블이 id가 아닌 primary 키 컬럼 이름을 사용한다면, ignore 메소드를 호출할 때 컬럼의 이름을 지정하면 됩니다:

```php
'email' => Rule::unique('users')->ignore($user->id, 'user_id')
```

추가적인 Where 구문 추가하기:

where 메소드를 사용하여 쿼리를 커스터마이징하는 추가 제약을 지정할 수 있습니다. 예를 들어, account_id이 1인지 확인하는 제약 조건을 추가해 보겠습니다:

```php
'email' => Rule::unique('users')->where(function ($query) {
    return $query->where('account_id', 1);
})
```

url
필드는 반드시 유효한 URL이어야 합니다.


