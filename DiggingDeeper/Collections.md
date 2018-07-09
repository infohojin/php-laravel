---
layout: laravel
title: Laravel
subtitle: Prologue
    
---

컬렉션
소개하기
컬렉션 생성하기
컬렉션 확장하기
사용 가능한 메소드
Higher Order Messages

## 소개하기
Illuminate\Support\Collection 클래스는 배열 데이터를 사용하기위한 유연하고 편리한 래퍼(wrapper)를 제공합니다. 다음의 사용예제를 확인하십시오. collect 헬퍼 함수를 사용하여 배열에서 새로운 collection 인스턴스를 생성하고 모든 요소들을 대문자로 변경한 다음 비어있는 요소들을 제거하는 예제 입니다:

```php
$collection = collect(['taylor', 'abigail', null])->map(function ($name) {
    return strtoupper($name);
})
->reject(function ($name) {
    return empty($name);
});
```

보시는 바와 같이 Collection 클래스는 편리한 맵핑과 배열의 감소를 수행하기 위한 체이닝 방식을 제공하빈다. 일반적으로, 컬렉션은 immutable-불편하고 모든 Collection의 메소드는 Collection의 인스턴스를 반환합니다.


## 컬렉션 생성하기
앞서 설명한 대로 collect 헬퍼 함수는 주어진 배열으로 부터 Illuminate\Support\Collection 인스턴스를 반환합니다. 따라서 컬렉션을 생성하는 것은 보시는 바와 같이 아주 간단합니다.

```php
$collection = collect([1, 2, 3]);
```

{tip} Eloquent쿼리의 결과는 항상 Collection 인스턴스를 반환합니다.


## 컬렉션 확장하기
컬렉션은 "macroable" 하기 때문에, 런타임에 Collection 클래스에 메소드를 추가하라 수 있습니다. 예를 들어 다음의 코드는 Collection 클래스에 toUpper 메소드를 추가합니다:

```php
use Illuminate\Support\Str;

Collection::macro('toUpper', function () {
    return $this->map(function ($value) {
        return Str::upper($value);
    });
});

$collection = collect(['first', 'second']);

$upper = $collection->toUpper();

// ['FIRST', 'SECOND']
```

일반적으로, 서비스 프로바이더에서 컬렉션에 메소드를 추가하게 됩니다.


## 사용가능한 메소드
이 문서의 나머지 부분에서는 Collection 클래스에서 사용할 수 있는 각각의 메소드를 설명합니다. 모든 메소드들은 보다 유연하게 배열을 처리할 수 있도록 체이닝 할 수 있다는 것을 기억하십시오. 또한, 대부분의 모든 메소드는 필요한 경우 컬렉션 원래의 컬렉션을 사용할 수 있도록, 새로운 Collection 인스턴스를 반환합니다.

all
average
avg
chunk
collapse
combine
concat
contains
containsStrict
count
crossJoin
dd
diff
diffAssoc
diffKeys
dump
each
eachSpread
every
except
filter
first
firstWhere
flatMap
flatten
flip
forget
forPage
get
groupBy
has
implode
intersect
intersectByKeys
isEmpty
isNotEmpty
keyBy
keys
last
macro
make
map
mapInto
mapSpread
mapToGroups
mapWithKeys
max
median
merge
min
mode
nth
only
pad
partition
pipe
pluck
pop
prepend
pull
push
put
random
reduce
reject
reverse
search
shift
shuffle
slice
sort
sortBy
sortByDesc
sortKeys
sortKeysDesc
splice
split
sum
take
tap
times
toArray
toJson
transform
union
unique
uniqueStrict
unless
unwrap
values
when
where
whereStrict
whereIn
whereInStrict
whereNotIn
whereNotInStrict
wrap
zip

## 메소드 목록

all() {#collection-method .first-collection-method}
all 메소드는 주어진 배열을 컬렉션으로 되돌려 줍니다:

```php
collect([1, 2, 3])->all();

// [1, 2, 3]
```

average() {#collection-method}
avg 메소드의 별칭입니다.


avg() {#collection-method}
avg 메소드는 주어진 키의 평균값을 반환합니다:

```php
$average = collect([['foo' => 10], ['foo' => 10], ['foo' => 20], ['foo' => 40]])->avg('foo');

// 20

$average = collect([1, 1, 2, 4])->avg();

// 2
```

chunk() {#collection-method}
chunk 메소드는 컬렉션을 주어진 사이즈의 더 작은 여러개로 분할합니다:

```php
$collection = collect([1, 2, 3, 4, 5, 6, 7]);

$chunks = $collection->chunk(4);

$chunks->toArray();

// [[1, 2, 3, 4], [5, 6, 7]]
```

이 메소드는 특히 뷰 안에서 부트스트랩 프레임워크와 같은 그리드(grid) 시스템을 작업할 때 유용합니다. Eloquent 모델 컬렉션을 그리드에 표시한다고 생각해 보십시오:

```php
@foreach ($products->chunk(3) as $chunk)
    <div class="row">
        @foreach ($chunk as $product)
            <div class="col-xs-4">{{ $product->name }}</div>
        @endforeach
    </div>
@endforeach
```

collapse() {#collection-method}
collapse 메소드는 배열의 컬렉션을 하나의 평면적인 컬렉션으로 만듭니다:

```php
$collection = collect([[1, 2, 3], [4, 5, 6], [7, 8, 9]]);

$collapsed = $collection->collapse();

$collapsed->all();

// [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

combine() {#collection-method}
combine 메소드는 컬렉션의 키들과 다른 배열 또는 컬렉션의 값을 결합합니다:

```php
$collection = collect(['name', 'age']);

$combined = $collection->combine(['George', 29]);

$combined->all();

// ['name' => 'George', 'age' => 29]
```

concat() {#collection-method}
concat 메소드는 주어진 배열 또는 컬렉션의 마지막에 값을 추가합니다:

```php
$collection = collect(['John Doe']);

$concatenated = $collection->concat(['Jane Doe'])->concat(['name' => 'Johnny Doe']);

$concatenated->all();

// ['John Doe', 'Jane Doe', 'Johnny Doe']
```

contains() {#collection-method}
contains 메소드는 컬렉션에 주어진 아이템을 포함하고 있는지 알려줍니다:

```php
$collection = collect(['name' => 'Desk', 'price' => 100]);

$collection->contains('Desk');

// true

$collection->contains('New York');

// false
```

또한 주어진 키/값 쌍이 컬렉션 안에 존재하는지 확인할 수 있도록 container 메소드에 키/값 쌍을 인자로 전달할 수도 있습니다:

```php
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
]);

$collection->contains('product', 'Bookcase');

// false
```

마지막으로, contains 메소드에 주어진 조건을 판별할 수 있는 콜백을 인자로 전달할 수도 있습니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$collection->contains(function ($value, $key) {
    return $value > 5;
});

// false
```

contains 메소드는 아이템의 값을 비교할 때 "느슨한" 비교를 수행하기 때문에, 정수값이 문자형일 때에도 정수형 값과 동일하다고 판단합니다. 타입에 대한 "엄격한" 비교를 원한다면 containsStrict 메소드를 사용하십시오.


containsStrict() {#collection-method}
이 메소드는 contains 메소드와 동일하게 사용되지만, "엄격한" 비교를 수행하는 것이 차이점입니다.


count() {#collection-method}
count 메소드는 컬렉션안에 존재하는 아이템의 전체 갯수를 반환합니다:

```php
$collection = collect([1, 2, 3, 4]);

$collection->count();

// 4
```

crossJoin() {#collection-method}
crossJoin 메소드는 컬렉션의 값을 주어진 배열 또는 컬렉션을 사용하여 크로스 조인(cross join)하여 합성 가능한 모든 순열을 가지는 데카르트 곱(직교 값)을 반환합니다:

```php
$collection = collect([1, 2]);

$matrix = $collection->crossJoin(['a', 'b']);

$matrix->all();

/*
    [
        [1, 'a'],
        [1, 'b'],
        [2, 'a'],
        [2, 'b'],
    ]
*/


$collection = collect([1, 2]);

$matrix = $collection->crossJoin(['a', 'b'], ['I', 'II']);

$matrix->all();

/*
    [
        [1, 'a', 'I'],
        [1, 'a', 'II'],
        [1, 'b', 'I'],
        [1, 'b', 'II'],
        [2, 'a', 'I'],
        [2, 'a', 'II'],
        [2, 'b', 'I'],
        [2, 'b', 'II'],
    ]
*/
```

dd() {#collection-method}
dd 메소드는 컬렉션의 아이템을 덤프하여 표시하고 스크립트를 종료합니다:

```php
$collection = collect(['John Doe', 'Jane Doe']);

$collection->dd();

/*
    Collection {
        #items: array:2 [
            0 => "John Doe"
            1 => "Jane Doe"
        ]
    }
*/
```

스크립트가 종료되지 않길 원한다면, dump 메소드를 사용하십시오.


diff() {#collection-method}
diff 메소드는 컬렉션을 다른 컬렉션 또는 일반적인 PHP 배열을 값을 기준으로 비교합니다. 이 메소드는 주어진 컬렉션 안에 들어 있지 않은 원래의 컬렉션 안에 있는 값을 반환합니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$diff = $collection->diff([2, 4, 6, 8]);

$diff->all();

// [1, 3, 5]
```

diffAssoc() {#collection-method}
diffAssoc 메소드는 컬렉션을 키와 값을 기준으로 하여 다른 컬렉션 또는 PHP 배열 비교합니다. 이 메소드는 주어진 컬렉션에는 있지만 원래의 컬렉션에서는 존재하지 않는 키 / 값 쌍을 반환합니다:

```php
$collection = collect([
    'color' => 'orange',
    'type' => 'fruit',
    'remain' => 6
]);

$diff = $collection->diffAssoc([
    'color' => 'yellow',
    'type' => 'fruit',
    'remain' => 3,
    'used' => 6
]);

$diff->all();

// ['color' => 'orange', 'remain' => 6]
```

diffKeys() {#collection-method}
diffKeys 메소드는 컬렉션과 다른 컬렉션의 키을 기준으로 배열을 비교합니다. 이 메소드는 주어진 컬렉션 안에 들어 있지 않은 원래의 컬렉션 안에 있는 키 / 값 쌍을 반환합니다:

```php
$collection = collect([
    'one' => 10,
    'two' => 20,
    'three' => 30,
    'four' => 40,
    'five' => 50,
]);

$diff = $collection->diffKeys([
    'two' => 2,
    'four' => 4,
    'six' => 6,
    'eight' => 8,
]);

$diff->all();

// ['one' => 10, 'three' => 30, 'five' => 50]
```

dump() {#collection-method}
dump 메소드는 컬렉션의 아이템을 덤프하여 표시합니다:

```php
$collection = collect(['John Doe', 'Jane Doe']);

$collection->dump();

/*
    Collection {
        #items: array:2 [
            0 => "John Doe"
            1 => "Jane Doe"
        ]
    }
*/
```

each() {#collection-method}
each 메소드는 컬렉션의 아이템을 반복적으로 처리하여 콜백에 각 아이템을 전달합니다:

```php
$collection->each(function ($item, $key) {
    //
});
```

전체 항목에 대한 반복을 중지하려면, 콜백 안에서 false 를 반환하면 됩니다:

```php
$collection->each(function ($item, $key) {
    if (/* some condition */) {
        return false;
    }
});
```

eachSpread() {#collection-method}
eachSpread 메소드는 컬렉션의 아이템을 돌면서 각각의 중첩된 아이템 값을 주어진 콜백에 전달합니다:

```php
$collection = collect([['John Doe', 35], ['Jane Doe', 33]]);

$collection->eachSpread(function ($name, $age) {
    //
});
```

반복을 멈추고자 한다면, 콜백안에서 false 를 반환하면 됩니다:

```php
$collection->eachSpread(function ($name, $age) {
    return false;
});
```

every() {#collection-method}
every 메소드는 컬렉션의 모든 요소들이 전달된 조건을 충족하는지 확인하는데 사용할 수 있습니다:

```php
collect([1, 2, 3, 4])->every(function ($value, $key) {
    return $value > 2;
});

// false
```

except() {#collection-method}
except 메소드는 지정된 키에 대한 아이템을 제외한 컬렉션의 모든 아이템을 반환합니다:

```php
$collection = collect(['product_id' => 1, 'price' => 100, 'discount' => false]);

$filtered = $collection->except(['price', 'discount']);

$filtered->all();

// ['product_id' => 1]
```

except 의 반대는, only메소드를 확인하십시오.


filter() {#collection-method}
filter 메소드는 주어진 콜백 컬렉션을 사용하여 필터링을 하고, 주어진 테스트에 true를 반환하는 항목 만 남게 됩니다:

```php
$collection = collect([1, 2, 3, 4]);

$filtered = $collection->filter(function ($value, $key) {
    return $value > 2;
});

$filtered->all();

// [3, 4]
```

콜백이 전달되지 않는다면, 모든 컬렉션의 엔트리 중에서 false 로 인식되는 값들은 제거됩니다:

```php
$collection = collect([1, 2, 3, null, false, '', 0, []]);

$collection->filter()->all();

// [1, 2, 3]
```

filter 의 반대는 reject 메소드를 확인하십시오.


first() {#collection-method}
first 메소드는 주어진 테스트를 통과하는 컬렉션의 첫 번째 요소를 반환합니다:

```php
collect([1, 2, 3, 4])->first(function ($value, $key) {
    return $value > 2;
});

// 3
```

first 메소드를 아무런 인자 없이 호출하여 컬렉션의 첫 번째 요소를 획득할 수 있습니다. 만약 컬렉션이 비어 있다면 null 이 반환됩니다:

```php
collect([1, 2, 3, 4])->first();

// 1
```

### firstWhere() {#collection-method}
firstWhere 메소드는 주어진 키 /값에 해당하는 첫번째 요소를 반환합니다:

```php
$collection = collect([
    ['name' => 'Regena', 'age' => 12],
    ['name' => 'Linda', 'age' => 14],
    ['name' => 'Diego', 'age' => 23],
    ['name' => 'Linda', 'age' => 84],
]);

$collection->firstWhere('name', 'Linda');

// ['name' => 'Linda', 'age' => 14]
```

또한 firstWhere 메소드에 연산자를 지정할 수도 있습니다:

```php
$collection->firstWhere('age', '>=', 18);

// ['name' => 'Diego', 'age' => 23]
```

### flatMap() {#collection-method}
flatMap 메소드는 각각의 컬렉션을 반복하며 각가의 값을 주어진 콜백에 전달합니다. 콜백은 자유롭게 이 값을 변경하고 돌려주며 이를 통해서 수정된 값을 기반으로 새로운 컬렉션을 만듭니다. 이 배열은 일차원이 됩니다:

```php
$collection = collect([
    ['name' => 'Sally'],
    ['school' => 'Arkansas'],
    ['age' => 28]
]);

$flattened = $collection->flatMap(function ($values) {
    return array_map('strtoupper', $values);
});

$flattened->all();

// ['name' => 'SALLY', 'school' => 'ARKANSAS', 'age' => '28'];
```

### flatten() {#collection-method}
flatten 메소드는 다차원으로 구성된 컬렉션을 한 차원으로 변경시킵니다:

```php
$collection = collect(['name' => 'taylor', 'languages' => ['php', 'javascript']]);

$flattened = $collection->flatten();

$flattened->all();

// ['taylor', 'php', 'javascript'];
```

여러분은 선택적으로 함수에 "깊이" 인자를 전달할 수 있습니다:

```php
$collection = collect([
    'Apple' => [
        ['name' => 'iPhone 6S', 'brand' => 'Apple'],
    ],
    'Samsung' => [
        ['name' => 'Galaxy S7', 'brand' => 'Samsung']
    ],
]);

$products = $collection->flatten(1);

$products->values()->all();

/*
    [
        ['name' => 'iPhone 6S', 'brand' => 'Apple'],
        ['name' => 'Galaxy S7', 'brand' => 'Samsung'],
    ]
*/
```

이 예제에서, flatten 을 깊이에 대한 인자없이 호출하면, 중첩 배열을 평평하게 하기 때문에 그 결과 ['iPhone 6S', 'Apple', 'Galaxy S7', 'Samsung']이 됩니다.깊이를 전달하게 되면 중첩된 배열의 깊이를 제한하여 정리될것입니다.


### flip() {#collection-method}
flip 메소드는 컬렉션의 키와 대응하는 값을 바꿉니다:

```php
$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

$flipped = $collection->flip();

$flipped->all();

// ['taylor' => 'name', 'laravel' => 'framework']
```

### forget() {#collection-method}
forget 메소드는 키에 해당하는 컬렉션의 아이템을 삭제합니다:

```php
$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

$collection->forget('name');

$collection->all();

// ['framework' => 'laravel']
```

{note} 다른 컬렉션 메소드와는 달리 forget는 수정된 새로운 컬렉션을 반환하지 않습니다; 호출된 컬렉션을 변경합니다.


### forPage() {#collection-method}
forPage 메소드는 주어진 페이지 넘버에 해당하는 아이템을 가지고 있는 새로운 컬렉션을 반환합니다. 이 메소드는 페이지 번호를 첫번째 인자로 페이지별로 보여주고자 하는 아이템의 갯수를 두번째 인자로 받아 들입니다:

```php
$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);

$chunk = $collection->forPage(2, 3);

$chunk->all();

// [4, 5, 6]
```

### get() {#collection-method}
get 메소드는 주어진 키에 대한 아이템을 반환합니다. 키가 존재하지 않느다면, null 이 반환됩니다:

```php
$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

$value = $collection->get('name');

// taylor
```

두번째 인자로 기본값을 전달할 수도 있습니다:

```php
$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

$value = $collection->get('foo', 'default-value');

// default-value
```

기본값을 결정하는 콜백을 전달할 수도 있습니다. 지정된 키가 존재하지 않는 경우에 콜백의 결과가 반환될 것입니다:

```php
$collection->get('email', function () {
    return 'default-value';
});

// default-value
```

### groupBy() {#collection-method}
groupBy 메소드는 주어진 키를 기준으로한 컬렉션의 아이템을 그룹을 지정합니다:

```php
$collection = collect([
    ['account_id' => 'account-x10', 'product' => 'Chair'],
    ['account_id' => 'account-x10', 'product' => 'Bookcase'],
    ['account_id' => 'account-x11', 'product' => 'Desk'],
]);

$grouped = $collection->groupBy('account_id');

$grouped->toArray();

/*
    [
        'account-x10' => [
            ['account_id' => 'account-x10', 'product' => 'Chair'],
            ['account_id' => 'account-x10', 'product' => 'Bookcase'],
        ],
        'account-x11' => [
            ['account_id' => 'account-x11', 'product' => 'Desk'],
        ],
    ]
*/
```

문자로 된 key 를 전달하는 것에 더해서, 콜백을 전달할 수도 있습니다. 콜백은 그룹으로 지정할 키를 반환해야 합니다:

```php
$grouped = $collection->groupBy(function ($item, $key) {
    return substr($item['account_id'], -3);
});

$grouped->toArray();

/*
    [
        'x10' => [
            ['account_id' => 'account-x10', 'product' => 'Chair'],
            ['account_id' => 'account-x10', 'product' => 'Bookcase'],
        ],
        'x11' => [
            ['account_id' => 'account-x11', 'product' => 'Desk'],
        ],
    ]
*/
```

다중 그룹 처리를 위해 배열을 넘길 수 있습니다. 각 배열 요소들은 다차원 배열에 맞는 단계로 그룹 처리 되어 적용됩니다:

```php
$data = new Collection([
    10 => ['user' => 1, 'skill' => 1, 'roles' => ['Role_1', 'Role_3']],
    20 => ['user' => 2, 'skill' => 1, 'roles' => ['Role_1', 'Role_2']],
    30 => ['user' => 3, 'skill' => 2, 'roles' => ['Role_1']],
    40 => ['user' => 4, 'skill' => 2, 'roles' => ['Role_2']],
]);

$result = $data->groupBy([
    'skill',
    function ($item) {
        return $item['roles'];
    },
], $preserveKeys = true);

/*
[
    1 => [
        'Role_1' => [
            10 => ['user' => 1, 'skill' => 1, 'roles' => ['Role_1', 'Role_3']],
            20 => ['user' => 2, 'skill' => 1, 'roles' => ['Role_1', 'Role_2']],
        ],
        'Role_2' => [
            20 => ['user' => 2, 'skill' => 1, 'roles' => ['Role_1', 'Role_2']],
        ],
        'Role_3' => [
            10 => ['user' => 1, 'skill' => 1, 'roles' => ['Role_1', 'Role_3']],
        ],
    ],
    2 => [
        'Role_1' => [
            30 => ['user' => 3, 'skill' => 2, 'roles' => ['Role_1']],
        ],
        'Role_2' => [
            40 => ['user' => 4, 'skill' => 2, 'roles' => ['Role_2']],
        ],
    ],
];
*/
```

### has() {#collection-method}
has 메소드는 주어진 키가 컬렉션 안에 존재하는지 알려줍니다:

```php
$collection = collect(['account_id' => 1, 'product' => 'Desk']);

$collection->has('product');

// true
```

### implode() {#collection-method}
implode 메소드는 컬렉션 안의 아이템들을 합쳐줍니다. 이 메소드의 인자는 컬렉션 안에 있는 아이템들의 타입에 의존합니다. 컬렉션이 배열 또는 객체를 가지고 있다면, 합치고자 하는 속성의 키와 값들 사이에 끼워 넣고자 하는 "glue" 문자열을 전달해야 합니다:

```php
$collection = collect([
    ['account_id' => 1, 'product' => 'Desk'],
    ['account_id' => 2, 'product' => 'Chair'],
]);

$collection->implode('product', ', ');

// Desk, Chair
```

컬렉션이 간단한 문자열 또는 숫자값을 가지고 있다면, 간단하게 "glue" 를 첫번째 인자로 전달하면 됩니다:

```php
collect([1, 2, 3, 4, 5])->implode('-');

// '1-2-3-4-5'
```

## intersect() {#collection-method}
intersect 메소드는 원래의 컬렉션에서 주어진 배열 또는 컬렉션에 존재하지 않는 값을 제거합니다. 메소드를 호출한 뒤에도 원래 컬렉션의 키 값은 보존됩니다:

```php
$collection = collect(['Desk', 'Sofa', 'Chair']);

$intersect = $collection->intersect(['Desk', 'Chair', 'Bookcase']);

$intersect->all();

// [0 => 'Desk', 2 => 'Chair']
```

## intersectByKeys() {#collection-method}
intersectByKeys 메소드는 원래의 컬렉션에서 주어진 배열 또는 컬렉션에 존재하지 않는 키를 제거합니다:

```php
$collection = collect([
    'serial' => 'UX301', 'type' => 'screen', 'year' => 2009
]);

$intersect = $collection->intersectByKeys([
    'reference' => 'UX404', 'type' => 'tab', 'year' => 2011
]);

$intersect->all();

// ['type' => 'screen', 'year' => 2009]
```

## isEmpty() {#collection-method}
isEmpty 메소드는 컬렉션이 비어 있는 경우 true 를 반환하고, 그렇지 않은 경우 false 를 반환합니다:

```php
collect([])->isEmpty();

// true
```

## isNotEmpty() {#collection-method}
isNotEmpty 메소드는 컬렉션이 비어 있지 않은 경우 true 를 반환하고, 그렇지 않은 경우 false 를 반환합니다:

```php
collect([])->isNotEmpty();

// false
```

## keyBy() {#collection-method}
keyBy 메소드는 주어진 키를 기준으로 컬렉션을 다시 구성합니다. 여러 아이템이 같은 키를 가지고 있다면, 새로운 컬렉션에서는 마지막 항목만 나타납니다:

```php
$collection = collect([
    ['product_id' => 'prod-100', 'name' => 'Desk'],
    ['product_id' => 'prod-200', 'name' => 'Chair'],
]);

$keyed = $collection->keyBy('product_id');

$keyed->all();

/*
    [
        'prod-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
        'prod-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
    ]
*/
```

메소드에 콜백은 전달할 수도 있습니다. 콜백은 컬렉션의 키 값을 반환해야합니다:

```php
$keyed = $collection->keyBy(function ($item) {
    return strtoupper($item['product_id']);
});

$keyed->all();

/*
    [
        'PROD-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
        'PROD-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
    ]
*/
```

## keys() {#collection-method}
keys 메소드는 컬렉션의 모든 키를 반환합니다:

```php
$collection = collect([
    'prod-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
    'prod-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
]);

$keys = $collection->keys();

$keys->all();

// ['prod-100', 'prod-200']
```

### last() {#collection-method}
last 메소드는 주어진 콜백에서 참이되는 값중에 가장 마지막 값을 반환합니다:

```php
collect([1, 2, 3, 4])->last(function ($value, $key) {
    return $value < 3;
});

// 2
```

last 메소드를 인자 없이 호출하여 컬렉션의 가장 마지막 값을 획득할 수도 있습니다. 컬렉션이 비어 있다면, null 이 반환됩니다:

```php
collect([1, 2, 3, 4])->last();

// 4
```

## macro() {#collection-method}
macro 메소드를 사용하면 런타임에 Collection 클래스에 메소드를 추가할 수 있습니다. 자세한 정보는 컬렉션 확장하기 문서를 참조하십시오.


## make() {#collection-method}
make 메소드는 새로운 컬렉션 인스턴스를 생성합니다. 컬렉션 생성하기 부분을 참고하십시오.


## map() {#collection-method}
map 메소드는 컬렉션 전체를 반복하여 주어진 콜백에 각각의 값을 전달합니다. 콜백은 자유롭게 아이템을 변경하고 반환하여, 변경된 아이템으로 구성된 새로운 컬렉션이 만들어 집니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$multiplied = $collection->map(function ($item, $key) {
    return $item * 2;
});

$multiplied->all();

// [2, 4, 6, 8, 10]
```

{note} 대다수의 다른 컬렉션 메소드와 같이, map 메소드는 새로운 컬렉션 인스턴스를 반환합니다; 이 메소드는 호출된 컬렉션을 변경하지 않습니다. 원래의 컬렉션을 변경하고자 한다면 transform메소드를 사용하십시오.


## mapInto() {#collection-method}
mapInto() 메소드는 컬렉션의 아이템을 반복하면서 전달된 클래스의 생성자에 주어진 값을 전달하여 새로운 인스턴스를 생성합니다:

```php
class Currency
{
    /**
     * Create a new currency instance.
     *
     * @param  string  $code
     * @return void
     */
    function __construct(string $code)
    {
        $this->code = $code;
    }
}

$collection = collect(['USD', 'EUR', 'GBP']);

$currencies = $collection->mapInto(Currency::class);

$currencies->all();

// [Currency('USD'), Currency('EUR'), Currency('GBP')]
```

### mapSpread() {#collection-method}
mapSpread 메소드는 컬렉션의 아이템을 돌면서 각각의 중첩된 아이템 값을 주어진 콜백에 전달합니다. 콜백에서는 자유롭게 아이템을 수정하고 반환할 수 있으며, 그 결과 수정된 아이템으로 구성된 새로운 컬렉션을 반환합니다:

```php
$collection = collect([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]);

$chunks = $collection->chunk(2);

$sequence = $chunks->mapSpread(function ($odd, $even) {
    return $odd + $even;
});

$sequence->all();

// [1, 5, 9, 13, 17]
```

### mapToGroups() {#collection-method}
mapToGroups 메소드는 컬렉션의 항목을 지정된 콜백별로 그룹화합니다. 콜백은 하나의 키 / 값 쌍을 포함하는 그룹화 된 값으로 구성된 새로운 컬렉션을 반환합니다:

```php
$collection = collect([
    [
        'name' => 'John Doe',
        'department' => 'Sales',
    ],
    [
        'name' => 'Jane Doe',
        'department' => 'Sales',
    ],
    [
        'name' => 'Johnny Doe',
        'department' => 'Marketing',
    ]
]);

$grouped = $collection->mapToGroups(function ($item, $key) {
    return [$item['department'] => $item['name']];
});

$grouped->toArray();

/*
    [
        'Sales' => ['John Doe', 'Jane Doe'],
        'Marketing' => ['Johhny Doe'],
    ]
*/

$grouped->get('Sales')->all();

// ['John Doe', 'Jane Doe']
```

### mapWithKeys() {#collection-method}
mapWithKeys 메소드는 컬렉션 전체를 반복하며 각각의 값을 주어진 콜백에 전달합니다 콜백은 하나의 키 / 값 쌍을 포함하는 연관 배열을 반환합니다:

```php
$collection = collect([
    [
        'name' => 'John',
        'department' => 'Sales',
        'email' => 'john@example.com'
    ],
    [
        'name' => 'Jane',
        'department' => 'Marketing',
        'email' => 'jane@example.com'
    ]
]);

$keyed = $collection->mapWithKeys(function ($item) {
    return [$item['email'] => $item['name']];
});

$keyed->all();

/*
    [
        'john@example.com' => 'John',
        'jane@example.com' => 'Jane',
    ]
*/
```

### max() {#collection-method}
max 메소드는 주어진 키에 해당하는 최대 값을 반환합니다:

```php
$max = collect([['foo' => 10], ['foo' => 20]])->max('foo');

// 20

$max = collect([1, 2, 3, 4, 5])->max();

// 5
```

### median() {#collection-method}
median 메소드는 주어진 키에 대한 중간값을 반환합니다

```php
$median = collect([['foo' => 10], ['foo' => 10], ['foo' => 20], ['foo' => 40]])->median('foo');

// 15

$median = collect([1, 1, 2, 4])->median();

// 1.5
```

### merge() {#collection-method}
merge 메소드는 주어진 배열 또는 컬렉션을 원래의 컬렉션과 합칩니다. 배열 안에 들어 있는 키가 컬렉션에 들어 있는 키와 일치한다면, 주어진 배열의 값이 원래의 컬렉션 안의 값을 덮어 쓸 것입니다:

```php
$collection = collect(['product_id' => 1, 'price' => 100]);

$merged = $collection->merge(['price' => 200, 'discount' => false]);

$merged->all();

// ['product_id' => 1, 'price' => 200, 'discount' => false]
```

만약 주어진 아이템의 키가 숫자라면, 이 값들은 컬렉션의 가장 마지막에 추가됩니다:

```php
$collection = collect(['Desk', 'Chair']);

$merged = $collection->merge(['Bookcase', 'Door']);

$merged->all();

// ['Desk', 'Chair', 'Bookcase', 'Door']
```

### min() {#collection-method}
min 메소드는 주어진 키에 대한 최소 값을 반환합니다:

```php
$min = collect([['foo' => 10], ['foo' => 20]])->min('foo');

// 10

$min = collect([1, 2, 3, 4, 5])->min();

// 1
```

### mode() {#collection-method}
mode 메소드는 확률분포에서의 중앙값을 반환합니다:

```php
$mode = collect([['foo' => 10], ['foo' => 10], ['foo' => 20], ['foo' => 40]])->mode('foo');

// [10]

$mode = collect([1, 1, 2, 4])->mode();

// [1]
```

### nth() {#collection-method}
nth 메소드는 매 n 번째 존재하는 요소로 구성된 새로운 컬렉션을 생성합니다:

(역자주: 첫번째 인자로 주어진 숫자로 나누어 나머지가 0인 경우의 아이템들로 구성된 컬렉션이 반환됩니다. 인덱스는 0부터 시작)

```php
$collection = collect(['a', 'b', 'c', 'd', 'e', 'f']);

$collection->nth(4);

// ['a', 'e']
```

두번째 인자로 offset을 전달할 수도 있습니다:

```php
$collection->nth(4, 1);

// ['b', 'f']
```

### only() {#collection-method}
only 메소드는 컬렉션 안에서 지정된 키에 대한 아이템만을 반환합니다:

```php
$collection = collect(['product_id' => 1, 'name' => 'Desk', 'price' => 100, 'discount' => false]);

$filtered = $collection->only(['product_id', 'name']);

$filtered->all();

// ['product_id' => 1, 'name' => 'Desk']
```

only 메소드의 반대는, except메소드를 확인하십시오.


### pad() {#collection-method}
pad 메소드는 배열이 지정된 사이즈에 도달할 때까지, 배열을 주어진 값으로 채웁니다. 이 메소드는 array_pad PHP 함수와 같은 형태로 동작합니다.

배열의 앞쪽을 채우기 위해서는 사이즈를 음수로 지정하면 됩니다. 주어진 사이즈의 절대 값이 배열의 길이보다 작거나 같으면 메소드는 실행되지 않습니다.

```php
$collection = collect(['A', 'B', 'C']);

$filtered = $collection->pad(5, 0);

$filtered->all();

// ['A', 'B', 'C', 0, 0]

$filtered = $collection->pad(-5, 0);

$filtered->all();

// [0, 0, 'A', 'B', 'C']
```

### partition() {#collection-method}
partition 메소드는 PHP의 list 함수와 함께 사용되어 주어진 조건식을 만족하는, 그리고 만족하지 못하는 두개의 요소들로 나누는데 사용할 수 있습니다:

```php
$collection = collect([1, 2, 3, 4, 5, 6]);

list($underThree, $aboveThree) = $collection->partition(function ($i) {
    return $i < 3;
});

$underThree->all();

// [1, 2]

$aboveThree->all();

// [3, 4, 5, 6]
```

### pipe() {#collection-method}
pipe 메소드는 컬렉션을 주어진 콜백에 전달하고 그 결과를 반환합니다:

```php
$collection = collect([1, 2, 3]);

$piped = $collection->pipe(function ($collection) {
    return $collection->sum();
});

// 6
```

### pluck() {#collection-method}
pluck 메소드는 주어진 키에 대한 모든 값을 반환합니다:

```php
$collection = collect([
    ['product_id' => 'prod-100', 'name' => 'Desk'],
    ['product_id' => 'prod-200', 'name' => 'Chair'],
]);

$plucked = $collection->pluck('name');

$plucked->all();

// ['Desk', 'Chair']
```

또한 컬렉션에서 반환되는 결과의 키로 지정하고자 하는 값을 지정할 수도 있습니다:

```php
$plucked = $collection->pluck('name', 'product_id');

$plucked->all();

// ['prod-100' => 'Desk', 'prod-200' => 'Chair']
```

중복되는 키가 존재한다면, 마지막에 매칭되는 요소가 pluck 결과 컬렉션에 추가됩니다:

```php
$collection = collect([
    ['brand' => 'Tesla',  'color' => 'red'],
    ['brand' => 'Pagani', 'color' => 'white'],
    ['brand' => 'Tesla',  'color' => 'black'],
    ['brand' => 'Pagani', 'color' => 'orange'],
]);

$plucked = $collection->pluck('color', 'brand');

$plucked->all();

// ['Tesla' => 'black', 'Pagani' => 'orange']
```

### pop() {#collection-method}
pop 메소드는 컬렉션의 마지막 값을 반환하고 제거합니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$collection->pop();

// 5

$collection->all();

// [1, 2, 3, 4]
```

### prepend() {#collection-method}
prepend 메소드는 컬렉션의 앞 부분에 아이템을 추가합니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$collection->prepend(0);

$collection->all();

// [0, 1, 2, 3, 4, 5]
```

또한 두번째 인자로 앞에 붙이고자 하는 아이템의 키를 설정할 수도 있습니다.

```php
$collection = collect(['one' => 1, 'two' => 2]);

$collection->prepend(0, 'zero');

$collection->all();

// ['zero' => 0, 'one' => 1, 'two' => 2]
```

### pull() {#collection-method}
pull 메소드는 주어진 키에 해당하는 아이템을 반환하고 삭제합니다:

```php
$collection = collect(['product_id' => 'prod-100', 'name' => 'Desk']);

$collection->pull('name');

// 'Desk'

$collection->all();

// ['product_id' => 'prod-100']
```

### push() {#collection-method}
push 메소드는 컬렉션의 마지막에 아이템을 추가합니다:

```php
$collection = collect([1, 2, 3, 4]);

$collection->push(5);

$collection->all();

// [1, 2, 3, 4, 5]
```

### put() {#collection-method}
put 메소드는 주어진 키와 값을 컬렉션에 추가합니다:

```php
$collection = collect(['product_id' => 1, 'name' => 'Desk']);

$collection->put('price', 100);

$collection->all();

// ['product_id' => 1, 'name' => 'Desk', 'price' => 100]
```

### random() {#collection-method}
random 메소드는 컬렉션의 아이템중 하나를 랜덤하게 반환합니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$collection->random();

// 4 - (retrieved randomly)
```

얼마나 많은 아이템을 랜덤으로 조회할지 random 메소드에 선택적으로 정수값을 전달할 수 있습니다. 수신할 아이템의 수를 명서직으로 전달하면 아이템 컬렉션이 반환됩니다:

```php
$random = $collection->random(3);

$random->all();

// [2, 4, 5] - (retrieved randomly)
```

컬렉션이 요청된 것보다 더 적은 갯수의 아이템을 가지고 있다면, 메소드는 InvalidArgumentException을 던집니다.


### reduce() {#collection-method}
reduce 메소드는 각각의 인자를 반복하여 다음 반복에 결과를 전달하면서 컬렉션을 하나의 값으로 줄입니다:

```php
$collection = collect([1, 2, 3]);

$total = $collection->reduce(function ($carry, $item) {
    return $carry + $item;
});

// 6
```

첫번째 반복에서 $carry 의 값은 null 입니다; 그러나 초기 값을 지정하고자 하는 경우에 reduce 의 두번째 인자로 전달할 수 있습니다:

```php
$collection->reduce(function ($carry, $item) {
    return $carry + $item;
}, 4);

// 10
```

### reject() {#collection-method}
reject 메소드는 컬렉션에서 지정된 콜백을 사용하여 필터링을 합니다. 콜백은 결과 컬렉션에서 제거되어야 하는 아이템의 경우 true를 반환해야합니다:

```php
$collection = collect([1, 2, 3, 4]);

$filtered = $collection->reject(function ($value, $key) {
    return $value > 2;
});

$filtered->all();

// [1, 2]
```

reject 메소드의 반대는, filter메소드를 확인하십시오.


### reverse() {#collection-method}
reverse 메소드는 컬렉션의 원래의 키는 유지하면서 아이템의 순서를 반대가 되게 합니다:

```php
$collection = collect(['a', 'b', 'c', 'd', 'e']);

$reversed = $collection->reverse();

$reversed->all();

/*
    [
        4 => 'e',
        3 => 'd',
        2 => 'c',
        1 => 'b',
        0 => 'a',
    ]
*/
```

### search() {#collection-method}
search 메소드는 주어진 값이 컬렉션에 있는지 찾아서 해당 값의 키를 반환합니다. 아이템을 찾지 못한 경우, false 가 반환됩니다.

```php
$collection = collect([2, 4, 6, 8]);

$collection->search(4);

// 1
```

검색은 "느슨한" 비교로 (타입을 엄격하게 비교하지 않습니다) 다시말해 문자열과 정수값의 경우 동일한 값이라면 같다고 판단합니다. 타입에 일치하는 "엄격한" 비교를 수행하려면 true를 메소드의 두 번째 인자로 전달하면 됩니다:

```php
$collection->search('4', true);

// false
```

다른 방법으로, 검색 콜백을 전달하여 콜백을 통과 하는 첫 번째 아이템을 획득할 수도 있습니다:

```php
$collection->search(function ($item, $key) {
    return $item > 5;
});

// 2
```

### shift() {#collection-method}
shift 메소드는 컬렉션에서 첫 번째 아이템을 제거하고 해당 값을 반환합니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$collection->shift();

// 1

$collection->all();

// [2, 3, 4, 5]
```

### shuffle() {#collection-method}
shuffle 메소드는 컬렉션의 아이템을 랜덤하게 섞어버립니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$shuffled = $collection->shuffle();

$shuffled->all();

// [3, 2, 5, 1, 4] - (generated randomly)
```

### slice() {#collection-method}
slice 메소드는 주어진 인덱스에서 시작하는 컬렉션으로 잘라냅니다:

```php
$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);

$slice = $collection->slice(4);

$slice->all();

// [5, 6, 7, 8, 9, 10]
```

만약 반환되는 컬렉션의 사이즈를 제한하고자 한다면, 메소드의 두번재 인자로 사이즈를 전달하면 됩니다:

```php
$slice = $collection->slice(4, 2);

$slice->all();

// [5, 6]
```

반환되는 슬라이스는 기본적으로 키 값을 유지 한 채 반환합니다. 만약 이전의 원래 키를 유지하지 않길 원한다면, 새로운 인덱스를 구성하기 위해서 values 메소드를 사용할 수 있습니다.


### sort() {#collection-method}
sort 메소드는 컬렉션을 정렬합니다. 정렬된 컬렉션은 원래의 배열 키를 유지 하기 때문에, 다음 예제에서 연속된 숫자 인덱스를 리셋하기 위해서 values메소드를 사용할 것입니다:

```
$collection = collect([5, 3, 1, 2, 4]);

$sorted = $collection->sort();

$sorted->values()->all();

// [1, 2, 3, 4, 5]
```

보다 복잡한 정렬이 필요하다면, sort 메소드에 여러분의 고유한 알고리즘을 위한 콜백을 전달할 수 있습니다. 컬렉션의 sort 메소드가 호출될 때의 동작은 uasortPHP 문서를 참고하십시오.

{tip} 만약 중첩된 배열이나 객체의 컬렉션을 정렬할 필요가 있다면, sortBy 또는 sortByDesc 메소드를 확인하십시오.


### sortBy() {#collection-method}
sortBy 메소드는 주어진 키에 의한 정렬을 수행합니다. 정렬된 컬렉션은 원래의 배열 키를 유지 하기 때문에, 다음 예제에서 연속된 숫자 인덱스를 리셋하기 위해서 values메소드를 사용할 것입니다:

```php
$collection = collect([
    ['name' => 'Desk', 'price' => 200],
    ['name' => 'Chair', 'price' => 100],
    ['name' => 'Bookcase', 'price' => 150],
]);

$sorted = $collection->sortBy('price');

$sorted->values()->all();

/*
    [
        ['name' => 'Chair', 'price' => 100],
        ['name' => 'Bookcase', 'price' => 150],
        ['name' => 'Desk', 'price' => 200],
    ]
*/
```

컬렉션의 값을 어떻게 정렬하는지를 결정하기 위해 콜백을 전달할 수도 있습니다:

```php
$collection = collect([
    ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
    ['name' => 'Chair', 'colors' => ['Black']],
    ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
]);

$sorted = $collection->sortBy(function ($product, $key) {
    return count($product['colors']);
});

$sorted->values()->all();

/*
    [
        ['name' => 'Chair', 'colors' => ['Black']],
        ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
        ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
    ]
*/
```

### sortByDesc() {#collection-method}
이 메소드의 사용법은 sortBy 메소드와 동일하지만, 반대의 순서로 컬렉션을 정렬합니다.


### sortKeys() {#collection-method}
sortKeys 메소드는 컬렉션을 지정된 배열의 키를 기준으로 정렬합니다.

```php
$collection = collect([
    'id' => 22345,
    'first' => 'John',
    'last' => 'Doe',
]);

$sorted = $collection->sortKeys();

$sorted->all();

/*
    [
        'first' => 'John',
        'id' => 22345,
        'last' => 'Doe',
    ]
*/
```

### sortKeysDesc() {#collection-method}
이 메소드의 사용법은 sortKeys 메소드와 동일하지만, 반대의 순서로 컬렉션을 정렬합니다.


### splice() {#collection-method}
splice 메소드는 지정된 인덱스에서 시작하여 아이템을 잘라내고 제거하여 반환합니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$chunk = $collection->splice(2);

$chunk->all();

// [3, 4, 5]

$collection->all();

// [1, 2]
```

반환되는 결과의 사이즈를 제한하기 위해서 두번째 인자로 전달할 수 있습니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$chunk = $collection->splice(2, 1);

$chunk->all();

// [3]

$collection->all();

// [1, 2, 4, 5]
```

추가적으로, 컬렉션에서 아이템을 제거하고, 대체할 새로운 아이템을 세번째 인자로 전달할 수 있습니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$chunk = $collection->splice(2, 1, [10, 11]);

$chunk->all();

// [3]

$collection->all();

// [1, 2, 10, 11, 4, 5]
```

### split() {#collection-method}
split 메소드는 컬렉션을 주어진 갯수의 그룹으로 나눕니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$groups = $collection->split(3);

$groups->toArray();

// [[1, 2], [3, 4], [5]]
```

### sum() {#collection-method}
sum 메소드는 컬렉션 안에 있는 모든 아이템들의 합을 반환합니다:

```php
collect([1, 2, 3, 4, 5])->sum();

// 15
```

컬렉션이 중첩된 배열이나 객체를 가지고 있다면, 어떤 값을 더해야할지 결정하는 키를 전달해야합니다:

```php
$collection = collect([
    ['name' => 'JavaScript: The Good Parts', 'pages' => 176],
    ['name' => 'JavaScript: The Definitive Guide', 'pages' => 1096],
]);

$collection->sum('pages');

// 1272
```

추가적으로, 컬렉션의 어떤 값을 더해야 할지 결정하는 콜백을 전달할 수도 있습니다:

```php
$collection = collect([
    ['name' => 'Chair', 'colors' => ['Black']],
    ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
    ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
]);

$collection->sum(function ($product) {
    return count($product['colors']);
});

// 6
```

### take() {#collection-method}
take 메소드는 지정된 숫자의 아이템으로 이루어진 새로운 컬렉션을 반환합니다:

```php
$collection = collect([0, 1, 2, 3, 4, 5]);

$chunk = $collection->take(3);

$chunk->all();

// [0, 1, 2]
```

컬렉션의 끝에서 부터 가져올 아이템의 갯수를 지정하기 위해서 음수를 전달할수도 있습니다:

```php
$collection = collect([0, 1, 2, 3, 4, 5]);

$chunk = $collection->take(-2);

$chunk->all();

// [4, 5]
```

### tap() {#collection-method}
tap 메소드는 컬렉션에 콜백과 함께 전달되며, 원래의 컬렉션에 영향을 주지 않고 특정 지점에 대해서 어떤 작업을 수행하고자 할 때 사용됩니다:

```php
collect([2, 4, 3, 1, 5])
    ->sort()
    ->tap(function ($collection) {
        Log::debug('Values after sorting', $collection->values()->toArray());
    })
    ->shift();

// 1
```

### times() {#collection-method}
times 메소드는 주어진 양의 횟수만큼 콜백을 수행한 결과로 새로운 컬렉션을 생성합니다:

```php
$collection = Collection::times(10, function ($number) {
    return $number * 9;
});

$collection->all();

// [9, 18, 27, 36, 45, 54, 63, 72, 81, 90]
```

이 메소드는 Eloquent 모델을 생성하는 팩토리와 조합할 때 유용합니다.

```php
$categories = Collection::times(3, function ($number) {
    return factory(Category::class)->create(['name' => 'Category #'.$number]);
});

$categories->all();

/*
    [
        ['id' => 1, 'name' => 'Category #1'],
        ['id' => 2, 'name' => 'Category #2'],
        ['id' => 3, 'name' => 'Category #3'],
    ]
*/
```

### toArray() {#collection-method}
toArray 메소드는 컬렉션을 PHP 배열로 변환합니다. 컬렉션의 값이 Eloquent 이라면, 이 모델또한 배열로 변환될 것입니다:

```php
$collection = collect(['name' => 'Desk', 'price' => 200]);

$collection->toArray();

/*
    [
        ['name' => 'Desk', 'price' => 200],
    ]
*/
```

{note} toArray 는 또한 모든 컬렉션의 중첩된 객체도 배열로 변환할 것입니다. 근본적인 raw 배열을 얻기를 원한다면 all 메소드를 대신 사용하십시오.


### toJson() {#collection-method}
toJson 메소드는 모든 컬렉션을 JSON serialized 문자열으로 변환합니다:

```php
$collection = collect(['name' => 'Desk', 'price' => 200]);

$collection->toJson();

// '{"name":"Desk", "price":200}'
```

### transform() {#collection-method}
transform 메소드는 컬렉션을 반복하여 컬렉션의 각각의 아이템에 대해서 주어진 콜백을 호출합니다. 컬렉션 안에 있는 아이템들은 자동으로 콜백의 결과로 대체될 것입니다:

```php
$collection = collect([1, 2, 3, 4, 5]);

$collection->transform(function ($item, $key) {
    return $item * 2;
});

$collection->all();

// [2, 4, 6, 8, 10]
```

{note} 다른 컬렉션 메소드와 다르게, transform 메소드는 컬렉션 자신을 변경합니다. 대신에, 새로운 컬렉션을 생성하려면 map 메소드를 사용하십시오.


### union() {#collection-method}
union 메소드는 주어진 배열을 컬렉션에 추가합니다. 만약 원래의 컬렉션에서 이미 가지고 있는 키를 주어진 배열에서 가지고 있다면, 원래의 컬렉션 값이 우선합니다:

```php
$collection = collect([1 => ['a'], 2 => ['b']]);

$union = $collection->union([3 => ['c'], 1 => ['b']]);

$union->all();

// [1 => ['a'], 2 => ['b'], 3 => ['c']]
```

### unique() {#collection-method}
unique 메소드는 컬렉션 안에서 유니크한 모든 아이템들을 반환합니다. 반환된 컬렉션은 원래의 배열 키를 유지 하기 때문에, 다음 예제에서 연속된 숫자 인덱스를 리셋하기 위해서 values메소드를 사용할 것입니다:

```php
$collection = collect([1, 1, 2, 2, 3, 4, 2]);

$unique = $collection->unique();

$unique->values()->all();

// [1, 2, 3, 4]
```

중첩된 배열이나 객체를 다룰 때에는, 유니크 여부를 결정할 키를 지정할 수 있습니다:

```php
$collection = collect([
    ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
    ['name' => 'iPhone 5', 'brand' => 'Apple', 'type' => 'phone'],
    ['name' => 'Apple Watch', 'brand' => 'Apple', 'type' => 'watch'],
    ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
    ['name' => 'Galaxy Gear', 'brand' => 'Samsung', 'type' => 'watch'],
]);

$unique = $collection->unique('brand');

$unique->values()->all();

/*
    [
        ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
        ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
    ]
*/
```

또한 아이템이 유니크한지 결정하기 위한 콜백을 전달할 수도 있습니다:

```php
$unique = $collection->unique(function ($item) {
    return $item['brand'].$item['type'];
});

$unique->values()->all();

/*
    [
        ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
        ['name' => 'Apple Watch', 'brand' => 'Apple', 'type' => 'watch'],
        ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
        ['name' => 'Galaxy Gear', 'brand' => 'Samsung', 'type' => 'watch'],
    ]
*/
```

unique 메소드는 아이템의 값을 비교할 때 "느슨한" 비교를 수행하기 때문에, 정수값이 문자형일 때에도 정수형 값과 동일하다고 판단합니다. 타입에 대한 "엄격한" 비교를 원한다면 uniqueStrict 메소드를 사용하십시오.


### uniqueStrict() {#collection-method}
이 메소드는 unique와 사용방법이 동일합니다. 차이점은 "엄격한" 비교를 수행한다는 점입니다.


### unless() {#collection-method}
unless 메소드는 메소드에 주어진 첫번째 인자값이 true가 아닐 때 두번째 인자로 전달되는 콜백을 실행합니다:

```php
$collection = collect([1, 2, 3]);

$collection->unless(true, function ($collection) {
    return $collection->push(4);
});

$collection->unless(false, function ($collection) {
    return $collection->push(5);
});

$collection->all();

// [1, 2, 3, 5]
```

unless 메소드의 반대는 when 메소드를 참고하십시오.


### unwrap() {#collection-method}
정적 메소드인 unwrap 메소드는 해당되는 경우, 컬렉션의 아이템을 컬렉션의 형태에서 해제하여 기본 타입 형태로 반환합니다:

```php
Collection::unwrap(collect('John Doe'));

// ['John Doe']

Collection::unwrap(['John Doe']);

// ['John Doe']

Collection::unwrap('John Doe');

// 'John Doe'
```

### values() {#collection-method}
values 메소드는 키를 리셋하고 연속적인 정수를 키로 하는 새로운 컬렉션을 반환합니다:

```php
$collection = collect([
    10 => ['product' => 'Desk', 'price' => 200],
    11 => ['product' => 'Desk', 'price' => 200]
]);

$values = $collection->values();

$values->all();

/*
    [
        0 => ['product' => 'Desk', 'price' => 200],
        1 => ['product' => 'Desk', 'price' => 200],
    ]
*/
```

### when() {#collection-method}
when 메소드는 when 메소드에 주어진 첫 번째 인자가 true로 판정 될 때 두번째 인자로 전달되는 콜백을 실행합니다:

```php
$collection = collect([1, 2, 3]);

$collection->when(true, function ($collection) {
    return $collection->push(4);
});

$collection->when(false, function ($collection) {
    return $collection->push(5);
});

$collection->all();

// [1, 2, 3, 4]
```

when 메소드의 반대는, unless 메소드를 참고하십시오.


### where() {#collection-method}
where 메소드는 주어진 키 / 값 쌍에 대해서 컬렉션을 필터링합니다:

```php
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->where('price', 100);

$filtered->all();

/*
    [
        ['product' => 'Chair', 'price' => 100],
        ['product' => 'Door', 'price' => 100],
    ]
*/
```

where 메소드는 아이템의 값을 확인할 때 타입을 "느슨한" 비교 수행하기 때문에, 문자형으로 된 정수값이라도 정수형과 동일하다고 판단합니다. "엄격한" 비교를 사용하여 필터링을 하려면 whereLoose 메소드를 사용하십시오.


### whereStrict() {#collection-method}
이 메소드는 where 메소드와 동일한 사용법을 가지고 있습니다;하지만 모든 값이 "강력하게" 비교되어 집니다.(타입까지 일치하는지 체크합니다)


### whereIn() {#collection-method}
whereIn 메소드는 주어진 배열 안에 포함 된 주어진 키/값을 사용하여 컬렉션을 필터링합니다:

```php
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->whereIn('price', [150, 200]);

$filtered->all();

/*
    [
        ['product' => 'Bookcase', 'price' => 150],
        ['product' => 'Desk', 'price' => 200],
    ]
*/
```

whereIn 메소드는 아이템 값을 "느슨하게" 비교하기 때문에, 문자형 정수값이더라도 정수형과 동일하다고 판단합니다. "엄격한" 비교를 통해서 필터링 하려면 whereInStrict 메소드를 사용하십시오.


### whereInStrict() {#collection-method}
이 메소드는 whereIn 와 동일합니다만, 모든 값들은 "엄격한" 비교를 진행합니다.

(역자주 : 느슨한 비교와 엄격한 비교는 ==와 ===의 차이처럼 타입과 값이 모두 일치하는지 비교하는 정도를 나타냅니다)


### whereInstanceOf() {#collection-method}
whereInstanceOf 메소드는 컬렉션을 주어진 클래스 타입으로 필터링합니다:

```php
$collection = collect([
    new User,
    new User,
    new Post,
]);

return $collection->whereInstanceOf(User::class);
```

### whereNotIn() {#collection-method}
whereNotIn 메소드는 컬렉션안에 주어진 키 / 값을 포함하지 않은 값들을 필터링합니다:

```php
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->whereNotIn('price', [150, 200]);

$filtered->all();

/*
    [
        ['product' => 'Chair', 'price' => 100],
        ['product' => 'Door', 'price' => 100],
    ]
*/
```

whereNotIn 메소드는 아이템의 값을 확인할 때 "느슨하게" 비교하기 때문에, 문자형 정수값이더라도 정수형과 동일하다고 판단합니다. (타입을 엄격하게 비교하지 않습니다) "엄격한" 비교를 원한다면, whereNotInStrict 메소드를 사용하십시오.


### whereNotInStrict() {#collection-method}
이 메소드의 사용법은 whereNotIn 메소드와 동일하지만, 모든 값들은 "엄격한" 비교를 수행합니다.


### wrap() {#collection-method}
정적 메소드인 wrap 메소드는 가능한 경우, 주어진 값을 컬렉션으로 감쌉니다:

```php
$collection = Collection::wrap('John Doe');

$collection->all();

// ['John Doe']

$collection = Collection::wrap(['John Doe']);

$collection->all();

// ['John Doe']

$collection = Collection::wrap(collect('John Doe'));

$collection->all();

// ['John Doe']
```

### zip() {#collection-method}
zip 메소드는 해당 인덱스의 원래 컬렉션의 값으로 주어진 배열의 값을 함께 합칩니다:

```php
$collection = collect(['Chair', 'Desk']);

$zipped = $collection->zip([100, 200]);

$zipped->all();

// [['Chair', 100], ['Desk', 200]]
```

Higher Order Messages
컬렉션은 공통된 작업을 수행하는데 필요한 "higher order message"를 제공합니다. 컬렉션에서 higher order message 가 가능한 메소드들은 average, avg, contains, each, every, filter, first, flatMap, groupBy, keyBy, map, max, min, partition, reject, sortBy, sortByDesc, sum, 그리고 unique 입니다.

각각의 higher order message 는 컬렉션 인스턴스의 동적 속성에 접근할 수 있습니다. 예를 들자면, 컬렉션 안에 있는 각 객체의 메소드를 호출하기 위해서 each higher order message 를 사용해보겠습니다:

```php
$users = User::where('votes', '>', 500)->get();

$users->each->markAsVip();
마찬가지로 sum higher order message를 사용하여 사용자 컬렉션의 "전체 투표 수를" 확인할 수 있습니다:

$users = User::where('group', 'Development')->get();

return $users->sum->votes;
```
