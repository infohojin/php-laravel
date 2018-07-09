---
layout: laravel
title: Laravel
subtitle: Prologue
    
---

Eloquent: Collections
소개
사용 가능한 메소드들
커스텀-사용자 정의 Collections

## 소개
get 메소드를 통하거나 하나의 relationship-관계를 통해서 Eloquent로 부터 반환되는 모든 멀티 레코드 결과는 Illuminate\Database\Eloquent\Collection 객체의 인스턴스가 됩니다. Eloquent 컬렉션 객체는 라라벨의 base collection을 상속받기 때문에, 자연스럽게 ELoquent 모델에 대한 결과에서 다양하고 편리한 메소드들을 사용할 수 있습니다.

또한 당연하게도, 모든 컬렉션은 Iterators(반복자)이기 때문에, 간단한 PHP 배열과 같이 반복문 안에서 사용할 수도 있습니다:

```php
$users = App\User::all();

foreach ($users as $user) {
    echo $user->name;
}
```

하지만, 컬렉션은 배열보다 강력하며, 직관적인 인터페이스를 사용하여 메소드 체인이 가능한 map / reduce 메소드를 사용 할 수 있습니다. 예를 들어 사용자 중에서 비활성화된 사용자를 제외하고 남은 사용자들의 이름을 확인하려면 다음과 같이 하면 됩니다:

```php
$users = App\User::where('active', 1)->get();

$names = $users->reject(function ($user) {
    return $user->active === false;
})
->map(function ($user) {
    return $user->name;
});
```

{note} 대부분의 Eloquent 컬렉션 메소드는 새로운 Eloquent 컬렉션 인스턴스를 반환하고, pluck, keys, zip, collapse, flatten 그리고 flip 메소드는 기본 컬렉션 인스턴스를 반환합니다. 또한 만약 map 실행이 Eloquent 모델을 포함하지 않는 컬렉션을 반환한다면, 자동으로 기본 컬렉션으로 캐스팅 될 것입니다.


## 사용가능한 메소드들
기본 컬렉션
모든 Eloquent 컬렉션은 Laravel collection 객체를 상속받습니다; 따라서 라라벨의 기본 컬렉션 클래스의 모든 강력한 메소드들을 상속받습니다.

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
diffKeys
dump
each
eachSpread
every
except
filter
first
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
isEmpty
isNotEmpty
keyBy
keys
last
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
splice
split
sum
take
tap
toArray
toJson
transform
union
unique
uniqueStrict
unless
values
when
where
whereStrict
whereIn
whereInStrict
whereNotIn
whereNotInStrict
zip

## 커스텀-사용자 정의 컬렉션
만약 여러분이 고유한 Collection 객체를 사용하고자 한다면, 모델 클래스에서 newCollection 메소드를 오버라이드(재지정) 하면 됩니다:

```php
<?php

namespace App;

use App\CustomCollection;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Create a new Eloquent Collection instance.
     *
     * @param  array  $models
     * @return \Illuminate\Database\Eloquent\Collection
     */
    public function newCollection(array $models = [])
    {
        return new CustomCollection($models);
    }
}
```

newCollection 메소드를 정의하고 나면, Eloquent가 모델에서 Collection 인스턴스를 반환할 때 언제나 여러분이 지정한 사용자 정의 컬렉션을 반환할 것입니다. 애플리케이션의 모든 모델에서 사용자 정의 컬렉션을 사용하기를 원한다면, 모든 모델 클래스들이 상속받는 기본 Model 클래스에서 newCollection 메소드를 오버라이드 해야만합니다.