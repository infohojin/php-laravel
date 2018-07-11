## 배열 & 객체

### array_add()
array_add 함수는 배열 내에 키가 존재하지 않는 경우 주어진 key/value 쌍을 배열에 추가합니다:

```php
$array = array_add(['name' => 'Desk'], 'price', 100);
// ['name' => 'Desk', 'price' => 100]
```

### array_collapse()
array_collapse 함수는 배열들의 배열(여러개의 배열)을 하나의 배열로 통합합니다:

```php
$array = array_collapse([[1, 2, 3], [4, 5, 6], [7, 8, 9]]);
// [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### array_divide()
array_divide 함수는 주어진 배열에서 키(key)들을 담고 있는 배열과 값(value)들을 담고 있는 배열, 총 2개의 배열들을 반환합니다:
```php
[$keys, $values] = array_divide(['name' => 'Desk']);

// $keys: ['name']

// $values: ['Desk']
```

### array_dot()
array_dot 함수는 다차원 배열을 ‘점(.)’으로 배열 깊이를 표기하면서 단일 레벨의 배열로 만듭니다:
```php
$array = ['products' => ['desk' => ['price' => 100]]];

$flattened = array_dot($array);

// ['products.desk.price' => 100]
```

### array_except()

array_except 메소드는 주어진 키 / 값 쌍을 배열에서 제거합니다.

```php
$array = ['name' => 'Desk', 'price' => 100];

$filtered = array_except($array, ['price']);

// ['name' => 'Desk']
```

### array_first()

array_first 함수는 넘겨진 배열 중 주어진 조건을 만족하는 첫번째 요소를 반환합니다:

```php
$array = [100, 200, 300];

$first = array_first($array, function ($value, $key) {
    return $value >= 150;
});

// 200
```

메소드에 세번째 파라미터로 기본 값을 지정할 수 있습니다. 배열의 어떠한 값도 조건을 통과하지 못했을 때 이 값이 반환됩니다:

```php
$first = array_first($array, $callback, $default);
```

### array_flatten()
array_flatten 함수는 다차원 배열을 단일 레벨의 1차원 배열로 만듭니다:

```php
$array = ['name' => 'Joe', 'languages' => ['PHP', 'Ruby']];

$flattened = array_flatten($array);

// ['Joe', 'PHP', 'Ruby']
```

### array_forget()

array_forget 함수는 “점(.)”표기법을 사용하여 중첩 배열로부터 주어진 키/ 값 쌍을 제거합니다:

```php
$array = ['products' => ['desk' => ['price' => 100]]];

array_forget($array, 'products.desk');

// ['products' => []]
```

### array_get()

array_get 함수는 “점(.)”표기법으로 중첩 배열로부터 주어진 값을 찾습니다:

```php
$array = ['products' => ['desk' => ['price' => 100]]];

$price = array_get($array, 'products.desk.price');

// 100
```

array_get 함수는 특정 키를 찾지 못한 경우 반환되는 기본값을 지정할 수도 있습니다.

```php
$discount = array_get($array, 'products.desk.discount', 0);

// 0
```

### array_has()
array_has 함수는 "점(.)" 표기를 이용하여 배열에 주어진 아이템 또는 아이템들이 존재하는지 확인합니다:

```php
$array = ['product' => ['name' => 'Desk', 'price' => 100]];

$contains = array_has($array, 'product.name');

// true

$contains = array_has($array, ['product.price', 'product.discount']);

// false
```

### array_last()
array_last 함수는 전달된 조건을 통과하는 아이템의 가장 마지막 요소를 반환합니다:

```php
$array = [100, 200, 300, 110];

$last = array_last($array, function ($value, $key) {
    return $value >= 150;
});

// 300
```

메소드의 세번째 인자로 기본값을 전달할 수 있습니다. 이 값은 조건을 통과하는 값이 없을 때 반환됩니다:

```
$last = array_last($array, $callback, $default);
```

### array_only()
array_only 함수는 특정한 키 / 값 쌍만을 배열로부터 반환합니다:

```php
$array = ['name' => 'Desk', 'price' => 100, 'orders' => 10];

$slice = array_only($array, ['name', 'price']);

// ['name' => 'Desk', 'price' => 100]
```

### array_pluck()
array_pluck 함수는 배열로부터 주어진 키 / 값 쌍의 리스트를 조회합니다:

```php
$array = [
    ['developer' => ['id' => 1, 'name' => 'Taylor']],
    ['developer' => ['id' => 2, 'name' => 'Abigail']],
];

$names = array_pluck($array, 'developer.name');

// ['Taylor', 'Abigail']
```

리스트만들어 지는 결과가 어떻게 키로 변환될지 지정할 수도 있습니다:

```php
$names = array_pluck($array, 'developer.name', 'developer.id');

// [1 => 'Taylor', 2 => 'Abigail']
```

### array_prepend()
array_prepend 함수는 배열의 시작부분에 아이템을 추가할 것입니다:

```php
$array = ['one', 'two', 'three', 'four'];

$array = array_prepend($array, 'zero');

// ['zero', 'one', 'two', 'three', 'four']
```

필요한 경우, 아이템의 키를 지정할 수도 있습니다:

```php
$array = ['price' => 100];

$array = array_prepend($array, 'Desk', 'name');

// ['name' => 'Desk', 'price' => 100]
```

### array_pull()
array_pull 함수는 배열에서 주어진 키 / 값 쌍을 반환함과 동시에 제거합니다:

```php
$array = ['name' => 'Desk', 'price' => 100];

$name = array_pull($array, 'name');

// $name: Desk

// $array: ['price' => 100]
```

메소드의 세번째 인자로 기본값을 전달 할 수 있습니다. 키가 존재하지 않는 경우 이 값이 반환됩니다:

```php
$value = array_pull($array, $key, $default);
```

### array_random()
array_random 함수는 배열에서 임의의 값을 반환합니다:

```php
$array = [1, 2, 3, 4, 5];

$random = array_random($array);

// 4 - (retrieved randomly)
```

두번째 인자로 몇개의 아이템을 반환할지 값을 지정할 수 있습니다. 이 인자를 지정하면, 하나의 아이템이 포함되더라도 배열이 반환 됩니다:

```php
$items = array_random($array, 2);

// [2, 5] - (retrieved randomly)
```

### array_set()
array_set 함수는 "점(.)" 표기법을 이용하여 중첩된 배열 내에 값을 설정합니다:

```php
$array = ['products' => ['desk' => ['price' => 100]]];

array_set($array, 'products.desk.price', 200);

// ['products' => ['desk' => ['price' => 200]]]
```

### array_sort()
array_sort 함수는 값을 기반으로 정렬을 수행합니다:

```php
$array = ['Desk', 'Table', 'Chair'];

$sorted = array_sort($array);

// ['Chair', 'Desk', 'Table']
```

또한 주어진 클로저의 결과 값으로 배열을 정렬 할 수 있습니다:

```php
$array = [
    ['name' => 'Desk'],
    ['name' => 'Table'],
    ['name' => 'Chair'],
];

$sorted = array_values(array_sort($array, function ($value) {
    return $value['name'];
}));

/*
    [
        ['name' => 'Chair'],
        ['name' => 'Desk'],
        ['name' => 'Table'],
    ]
*/
```

### array_sort_recursive()
array_sort_recursive 함수는 sort 함수를 이용하여 반복적으로 배열을 정렬합니다:

```php
$array = [
    ['Roman', 'Taylor', 'Li'],
    ['PHP', 'Ruby', 'JavaScript'],
];

$sorted = array_sort_recursive($array);

/*
    [
        ['Li', 'Roman', 'Taylor'],
        ['JavaScript', 'PHP', 'Ruby'],
    ]
*/
```

### array_where()
array_where 함수는 주어진 클로져를 사용하여 배열을 필터링합니다:

```php
$array = [100, '200', 300, '400', 500];

$filtered = array_where($array, function ($value, $key) {
    return is_string($value);
});

// [1 => '200', 3 => '400']
```

### array_wrap()
array_wrap 함수는 주어진 값을 배열로 만듭니다. 만약 함수에 전달된 값이 배열이라면, 결과에는 변경사항이 없습니다:

```php
$string = 'Laravel';

$array = array_wrap($string);

// ['Laravel']
```

주어진 값이 null인 경우, 빈 배열이 반환됩니다:

```php
$nothing = null;

$array = array_wrap($nothing);

// []
```

### data_fill()
data_fill 함수는 "점" 표기법을 사용하여 중첩된 배열 또는 객체 내부에 빠진 값을 채워넣습니다:

```php
$data = ['products' => ['desk' => ['price' => 100]]];

data_fill($data, 'products.desk.price', 200);

// ['products' => ['desk' => ['price' => 100]]]

data_fill($data, 'products.desk.discount', 10);

// ['products' => ['desk' => ['price' => 100, 'discount' => 10]]]
```

이 함수는 또한 별표를 와일드카드로 받아들여, 해당하는 대상을 채워넣습니다:

```php
$data = [
    'products' => [
        ['name' => 'Desk 1', 'price' => 100],
        ['name' => 'Desk 2'],
    ],
];

data_fill($data, 'products.*.price', 200);

/*
    [
        'products' => [
            ['name' => 'Desk 1', 'price' => 100],
            ['name' => 'Desk 2', 'price' => 200],
        ],
    ]
*/
```

### data_get()
data_get 함수는 "점" 표기법을 사용하여 중첩된 배열 또는 객체 안에서 값을 조회합니다:

```php
$data = ['products' => ['desk' => ['price' => 100]]];

$price = data_get($data, 'products.desk.price');

// 100
```

data_get 함수는 또한 지정된 키를 찾을 수 없는 경우 반환될 기본값을 인자로 받습니다:

```php
$discount = data_get($data, 'products.desk.discount', 0);

// 0
```

### data_set()
data_set 함수는 "dot" 표기법을 사용하여 중첩된 배열 또는 객체 안의 값을 설정합니다:

```php
$data = ['products' => ['desk' => ['price' => 100]]];

data_set($data, 'products.desk.price', 200);

// ['products' => ['desk' => ['price' => 200]]]
```

이 함수는 또한 별표를 와일드카드로 받아들여, 해당하는 대상을 설정합니다:

```php
$data = [
    'products' => [
        ['name' => 'Desk 1', 'price' => 100],
        ['name' => 'Desk 2', 'price' => 150],
    ],
];

data_set($data, 'products.*.price', 200);

/*
    [
        'products' => [
            ['name' => 'Desk 1', 'price' => 200],
            ['name' => 'Desk 2', 'price' => 200],
        ],
    ]
*/
```

기본적으로 존재하는 값은 덮어쓰게 됩니다. 값이 존재하지 않을 때만, 새로운 값을 설정하고자 한다면, 세번째 인자로 false를 전달하면 됩니다:

```php
$data = ['products' => ['desk' => ['price' => 100]]];

data_set($data, 'products.desk.price', 200, false);

// ['products' => ['desk' => ['price' => 100]]]
```

### head()
head 함수는 배열의 첫번째 요소를 반환합니다:

```php
$array = [100, 200, 300];

$first = head($array);

// 100
```

### last()
last 함수는 배열의 마지막 요소를 반환합니다:

```php
$array = [100, 200, 300];

$last = last($array);

// 300
```
