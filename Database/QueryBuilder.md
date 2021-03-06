---
layout: laravel
title: Laravel
subtitle: Prologue
    
---

데이터베이스: 쿼리 빌더
소개하기
결과 조회하기
결과 분할하기
Aggregates-집계문
Selects-선택
Raw 표현식
Joins-조인
Unions-유니온
Where 구문
파라미터 그룹
존재여부를 판단하는 Where 절
JSON Where 절
Ordering, Grouping, Limit, & Offset
Conditional-조건적 구문
Inserts-삽입
Updates-수정
JSON 컬럼 업데이트
Increment-증가 & Decrement-감소
Deletes-삭제
Pessimistic Locking

# 소개하기
라라벨의 데이터베이스 쿼리 빌더는 데이터베이스 쿼리들을 만들고 운영하는데 다양한 인터페이스의 편의기능를 제공합니다. 여러분들의 애플리케이션에서 운영하는 대부분의 데이터베이스 시스템에서 잘 작동합니다.

라라벨의 쿼리 빌더는 PDO 파라미터 바인딩을 사용하여 SQL injection 공격을 방지합니다. 따라서 쿼리에 바인딩할 문자열들을 따로 정리하고 전달할 필요가 없습니다.


# 결과 조회하기
한 테이블에서 모든 행들을 가져오기
DB 파사드의 table 메소드를 사용하여 쿼리를 시작할 수 있습니다. table 메소드는 주어진 테이블에 대한 쿼리빌더 인스턴스를 반환하고, 다양한 제약을 메소드 체이닝 형태로 구성할 수 있으며, 마지막으로 get 메소드를 사용하여 결과를 가져올 수 있습니다:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\DB;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * Show a list of all of the application's users.
     *
     * @return Response
     */
    public function index()
    {
        $users = DB::table('users')->get();

        return view('user.index', ['users' => $users]);
    }
}
```

get 메소드는 각각의 결과가 PHP StdClass 객체 인스턴스로 구성된 Illuminate\Support\Collection 을 반환합니다. 객체의 속성이 컬럼인것 처럼, 각각의 컬럼의 값에 엑세스 할 수 있습니다.

```php
foreach ($users as $user) {
    echo $user->name;
}
```

## 테이블에서 하나의 결과 / 컬럼 가져오기
데이터베이스 테이블에서 하나의 row을 가져오고자 한다면, first 메소드를 사용하면 됩니다. 이 메소드는 하나의 StdClass 객체를 반환할 것입니다:

```php
$user = DB::table('users')->where('name', 'John')->first();

echo $user->name;
```

전체 row 가 필요하지 않다면, value 메소드를 사용하여 레코드에서 하나의 값만 추출할 수 있습니다. 메소드는 컬럼의 직접적인 값을 반환할 것입니다:

```php
$email = DB::table('users')->where('name', 'John')->value('email');
```

## 컬럼 값의 목록 조회하기
한개의 컬럼의 값들을 포함하고 있는 컬렉션을 조회하고자 한다면, pluck 메소드를 사용할 수 있습니다. 이 예제에서는 사용자의 역할의 이름의 컬렉션을 조회할 것입니다:

```php
$titles = DB::table('roles')->pluck('title');

foreach ($titles as $title) {
    echo $title;
}
```

반환될 컬렉션에서 사용될 특정한 컬럼 키를 지정할 수도 있습니다:

```php
$roles = DB::table('roles')->pluck('title', 'name');

foreach ($roles as $name => $title) {
    echo $title;
}
```

## 결과 분할하기
데이터베이스 레코드가 많은 작업을 수행해야 한다면, chunk 메소드를 사용하는 것을 고려하십시오. 이 메소드는 한번에 결과에 대한 하나의 작은 chunk 를 획득하고, 각각의 chunk를 Closure 를 통해서 처리합니다. 이 메소드는 다수의 레코드를 처리하는 아티즌 명령어를 작성하는데 유용합니다. 예를 들어, 전체 users 테이블에서 한번에 100개의 레코드를 가져온다고 해보겠습니다:

```php
DB::table('users')->orderBy('id')->chunk(100, function ($users) {
    foreach ($users as $user) {
        //
    }
});
```

Closure 에서 false를 반환하여, 더이상의 chunk를 처리하지 않도록 중단할 수 있습니다:

```php
DB::table('users')->orderBy('id')->chunk(100, function ($users) {
    // Process the records...

    return false;
});
```

## Aggregates-집계문
쿼리빌더는 또한 count, max, min, avg 그리고 sum 과 같은 다양한 집계 관련 메소드를 제공합니다. 이 메소드들은 쿼리를 생성한 뒤에 호출하면 됩니다:

```php
$users = DB::table('users')->count();

$price = DB::table('orders')->max('price');
```

물론, 이 메소드들과 다른 구문을 조합하여 사용할 수 있습니다:

```php
$price = DB::table('orders')
                ->where('finalized', 1)
                ->avg('price');
```

## 레코드가 존재하는지 확인하기
쿼리 조건에 맞는 레코드가 존재하는지 확인하기 위해서 count 메소드를 사용하는 대신에, exists 와 doesntExist 메소드를 사용할 수 있습니다:

```php
return DB::table('orders')->where('finalized', 1)->exists();

return DB::table('orders')->where('finalized', 1)->doesntExist();
```


## Selects-선택
Select문 지정하기
당연하게도, 항상 데이터베이스의 테이블에서 모든 컬럼을 조회하고자 하지는 않을 것입니다. select 메소드를 사용하여, 쿼리에서 select 문을 위한 지정을 할 수 있습니다:

```php
$users = DB::table('users')->select('name', 'email as user_email')->get();
```

distinct 메소드는 쿼리가 고유한 결과를 반환하도록 강제할 수 있습니다:

```php
$users = DB::table('users')->distinct()->get();
```

이미 쿼리빌더 인스턴스를 가지고 있고, 존재하는 select 구문에 선택할 컬럼을 추가하고자 한다면, addSelect 메소드를 사용할 수 있습니다:

```php
$query = DB::table('users')->select('name');

$users = $query->addSelect('age')->get();
```

## Raw 표현식
때로는, 쿼리에서 Raw Expressions를 사용하고자 할 수도 있습니다. raw expression 을 생성하기 위해서는 DB::raw 메소드를 사용할 수 있습니다:

```php
$users = DB::table('users')
                     ->select(DB::raw('count(*) as user_count, status'))
                     ->where('status', '<>', 1)
                     ->groupBy('status')
                     ->get();
```

{note} 이러한 구문들은 쿼리를 문자열 형태로 주입하기 때문에, SQL 인젝션에 취약하지 않도록 특별히 주의해야 합니다!


## Raw 메소드
DB::raw 를 사용하는 대신에, 쿼리의 다양한 부분을 raw expression으로 대체하기 위해서 다음의 메소드를 사용할 수 있습니다.

selectRaw
selectRaw 메소드는 select(DB::raw(...)) 대신 사용할 수 있습니다. 이 메소드는 옵션 배열을 두번째 인자로 받습니다:

```php
$orders = DB::table('orders')
                ->selectRaw('price * ? as price_with_tax', [1.0825])
                ->get();
```
whereRaw / orWhereRaw
whereRaw 와 orWhereRaw 메소드는 쿼리의 where 절에 raw 한 구문을 삽입하는데 사용할 수 있습니다. 이 메소드도 옵션 배열을 두번째 인자로 받습니다:

```php
$orders = DB::table('orders')
                ->whereRaw('price > IF(state = "TX", ?, 100)', [200])
                ->get();
```

havingRaw / orHavingRaw
havingRaw 와 orHavingRaw 메소드는 having 절의 값으로 raw 문자열을 설정하는데 사용됩니다. 이 메소드는 두번째 인자로 (옵션값) 바인딩 배열을 받을 수 있습니다:
```php
$orders = DB::table('orders')
                ->select('department', DB::raw('SUM(price) as total_sales'))
                ->groupBy('department')
                ->havingRaw('SUM(price) > ?', [2500])
                ->get();
```

orderByRaw
orderByRaw 메소드는 order by 절의 값을 raw 한 문자열로 설정하는데 사용합니다:

```php
$orders = DB::table('orders')
                ->orderByRaw('updated_at - created_at DESC')
                ->get();
```

## Joins-조인
Inner Join
쿼리 빌더를 사용해서 조인문(join statement)을 만들수 있습니다. 기본적인 "inner join" 을 수행하기 위해서는 쿼리 빌더 인스턴스에 join 메소드를 사용하면 됩니다. join 메소드에 전달되는 첫번째 인자는 join을 수행할 테이블의 이름이며, 구 이후는 join 을 실행할 때 컬럼의 제약 조건입니다. 물론, 다음과 같이 하나의 쿼리에서 여러 테이블을 join 할 수도 있습니다:

```php
$users = DB::table('users')
            ->join('contacts', 'users.id', '=', 'contacts.user_id')
            ->join('orders', 'users.id', '=', 'orders.user_id')
            ->select('users.*', 'contacts.phone', 'orders.price')
            ->get();
```

## Left 조인
"inner join" 대신 "left join" 을 수행하고자 한다면, leftJoin 메소드를 사용하십시오. leftJoin 메소드는 join 메소드와 동일한 구성을 가집니다:

```php
$users = DB::table('users')
            ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
            ->get();
```

## Cross Join
"Cross Join"을 수행하고자 한다면 조인 하고자 하는 테이블 이름과 함께 crossJoin 메소드를 사용하면 됩니다. 크로스 조인은 첫 번째 테이블과 조인된 테이블 사이의 cartesian product 를 생성합니다:

```php
$users = DB::table('sizes')
            ->crossJoin('colours')
            ->get();
```

## 보다 복잡한 Join
보다 복잡한 join 구문도 지정할 수 있습니다. 이렇게 하기 위해서, join 메소드의 두번째 인자로 Closure 를 전달하십시오. Closure 는 JoinCaluse 객체를 전달 받아, join 구문에 제약사항을 지정할 것입니다.

```php
DB::table('users')
        ->join('contacts', function ($join) {
            $join->on('users.id', '=', 'contacts.user_id')->orOn(...);
        })
        ->get();
```

join 구문에 "where" 을 사용하고자 한다면, join 에 where와 orWhere 메소드를 사용하면 됩니다. 두개의 컬럼을 비교하는 대신 이러한 메소드들은 값과 컬럼을 비교합니다:

```php
DB::table('users')
        ->join('contacts', function ($join) {
            $join->on('users.id', '=', 'contacts.user_id')
                 ->where('contacts.user_id', '>', 5);
        })
        ->get();
```

## Unions-유니온
쿼리 빌더는 두 개의 쿼리를 "union" 하는 간편한 방법을 제공합니다. 다음과 같이 초기화한 쿼리를 생성하고 두 번째 쿼리를 결합하기 위하여 union 메소드를 사용할 수 있습니다:

```php
$first = DB::table('users')
            ->whereNull('first_name');

$users = DB::table('users')
            ->whereNull('last_name')
            ->union($first)
            ->get();
```

{tip} unionAll 메소드도 union 메소드와 같은 방법으로 사용할 수 있습니다.


## Where 절
간단한 Where 절
쿼리 빌더 인스턴스에서 where 절을 쿼리에 추가하기 위해서는 where 메소드를 사용할 수 있습니다. 가장 기본적인 where 메소드 호출 방법에는 3가지 인자가 필요합니다. 첫번째 인자는 컬럼의 이름입니다. 두번째 인자는 데이터베이스가 지원하는 연산자입니다. 마지막으로, 세번째 인자는 컬럼에 대해 비교하는 값입니다.

다음과 같이 "votes" 컬럼이 100과 일치하는 값을 가진 쿼리를 수행합니다:

```php
$users = DB::table('users')->where('votes', '=', 100)->get();
```

보다 편리하게 사용하기 위해서, 컬럼이 주어진 값과 동일한가를 확인하고자 한다면, where 메소드의 두번째 인자로 바로 값을 전달할 수도 있습니다:

```php
$users = DB::table('users')->where('votes', 100)->get();
```

물론, where절을 작성할 때 다양한 다른 연산자를 사용할 수 있습니다:

```php
$users = DB::table('users')
                ->where('votes', '>=', 100)
                ->get();

$users = DB::table('users')
                ->where('votes', '<>', 100)
                ->get();

$users = DB::table('users')
                ->where('name', 'like', 'T%')
                ->get();
```

또한 where 함수에 조건에 대한 배열을 전달 할 수도 있습니다:

```php
$users = DB::table('users')->where([
    ['status', '=', '1'],
    ['subscribed', '<>', '1'],
])->get();
```

## Or 구문
쿼리에 or 절을 추가하는 것과 같이 where 제약에 더해서 체이닝 형태로 사용할 수 있습니다. orWhere 메소드는 where 메소드와 동일한 인자를 전달 받습니다:

```php
$users = DB::table('users')
                    ->where('votes', '>', 100)
                    ->orWhere('name', 'John')
                    ->get();
```

## 추가적인 Where 구문
whereBetween

whereBetween 메소드는 컬럼의 값이 두개의 값 사이의 값인지 확인합니다:

```php
$users = DB::table('users')
                    ->whereBetween('votes', [1, 100])->get();
```

whereNotBetween

whereNotBetween 메소드는 컬럼의 값이 두개의 값 사이가 아닌지 확인합니다:

```php
$users = DB::table('users')
                    ->whereNotBetween('votes', [1, 100])
                    ->get();
```

whereIn / whereNotIn

whereIn메소드는 주어진 컬럼의 값이 주어진 배열안에 값에 속해 있는지 확인합니다:

```php
$users = DB::table('users')
                    ->whereIn('id', [1, 2, 3])
                    ->get();
```

whereNotIn 메소드는 주어진 컬럼의 값이 주어진 배열안에 포함되어 있지 않은지 확인합니다:

```php
$users = DB::table('users')
                    ->whereNotIn('id', [1, 2, 3])
                    ->get();
```

whereNull / whereNotNull

whereNull메소드는 주어진 컬럼의 값이 NULL 인지 확인합니다:

```php
$users = DB::table('users')
                    ->whereNull('updated_at')
                    ->get();
```

whereNotNull메소드는 주어진 컬럼의 값이 NULL이 아닌지 확인합니다:

```php
$users = DB::table('users')
                    ->whereNotNull('updated_at')
                    ->get();
```

whereDate / whereMonth / whereDay / whereYear / whereTime

whereDate 메소드는 컬럼의 값이 date 값인지 비교하는데 사용됩니다:

```php
$users = DB::table('users')
                ->whereDate('created_at', '2016-12-31')
                ->get();
```

whereMonth 메소드는 컬럼의 값이 한해의 지정된 달과 같은지 비교하는데 사용됩니다:

```php
$users = DB::table('users')
                ->whereMonth('created_at', '12')
                ->get();
```

whereDay 메소드는 컬럼의 값이 한달의 지정된 일과 같은지 비교하는데 사용됩니다:

```php
$users = DB::table('users')
                ->whereDay('created_at', '31')
                ->get();
```

whereYear 메소드는 컬럼의 값이 지정된 년도와 같은지 비교하는데 사용됩니다:

```php
$users = DB::table('users')
                ->whereYear('created_at', '2016')
                ->get();
```

whereTime 메소드는 컬럼의 값을 특정 시간과 비교하는데 사용할 수 있습니다:

```php
$users = DB::table('users')
                ->whereTime('created_at', '=', '11:20:45')
                ->get();
```

whereColumn

whereColumn 메소드는 두개의 컬럼이 동일하는 것을 확인하는데 사용할 수 있습니다:

```php
$users = DB::table('users')
                ->whereColumn('first_name', 'last_name')
                ->get();
```

또한 비교 연산자를 메소드에 전달할 수도 있습니다:

```php
$users = DB::table('users')
                ->whereColumn('updated_at', '>', 'created_at')
                ->get();
```

whereColumn메소드는 또한 다수의 조건 배열을 전달 받을 수도 있습니다. 이 조건들은 and 연산자를 사용하여 연결됩니다:

```php
$users = DB::table('users')
                ->whereColumn([
                    ['first_name', '=', 'last_name'],
                    ['updated_at', '>', 'created_at']
                ])->get();
```

## 파라미터 그룹
가끔은 "where exists" 절이나 중첩된 파라미터를 그룹으로 묶어서 수행하는것과 같이, 복잡한 조건문을 사용해야 할 필요가 있습니다. 라라벨의 쿼리 빌더는 이 경우 다음과 같이 사용할 수 있습니다. 다음의 괄호 안에 제약조건을 그룹으로 묶는 예제를 살펴보겠습니다:

```php
DB::table('users')
            ->where('name', '=', 'John')
            ->where(function ($query) {
                $query->where('votes', '>', 100)
                      ->orWhere('title', '=', 'Admin');
            })
            ->get();
```

위에서 보시다 시피, where 메소드에 전달된 하나의 Closure가 쿼리빌더의 제약조건을 그룹으로 묶고 있습니다. 이 Closure는 괄호로 포함된 제약조건을 설정하는데 사용할 쿼리빌더 인스턴스를 전달받습니다. 이 예제는 다음과 같은 SQL을 생성할 것입니다:

```php
select * from users where name = 'John' and (votes > 100 or title = 'Admin')
```

{tip} 글로벌 스코프가 적용될 때 예상치 못한 동작이 실행되는 것을 방지하기 위해서 항상 orWhere 그룹을 호출해야합니다.


## 존재여부를 판단하는(Exists) where 절
whereExists 메소드는 SQL 쿼리에 where exists 를 작성할 수 있도록 합니다. whereExists 메소드는 쿼리에 "exist" 구문을 저으이할 수 있도록 쿼리 빌더를 인자로 받아들이는 Closure를 인자로 받아들입니다:

```php
DB::table('users')
            ->whereExists(function ($query) {
                $query->select(DB::raw(1))
                      ->from('orders')
                      ->whereRaw('orders.user_id = users.id');
            })
            ->get();
```

위의 쿼리는 다음과 같은 SQL을 생성합니다:

```php
select * from users
where exists (
    select 1 from orders where orders.user_id = users.id
)
```

## JSON Where 절
라라벨은 또한 JSON 컬럼 타입을 지원하는 데이터베이스의 JSON 컬럼 타입에 대한 쿼리를 지원합니다. 현재는 MySQL 5.7 그리고 PostgreSQL에 포함되어 있습니다. JSON 컬럼 질의를 하기 위해서는 -> 연산자를 사용하십시오:

```php
$users = DB::table('users')
                ->where('options->language', 'en')
                ->get();

$users = DB::table('users')
                ->where('preferences->dining->meal', 'salad')
                ->get();
```

#Ordering, Grouping, Limit, & Offset
#orderBy 정렬
orderBy 메소드는 주어진 컬럼에 대한 쿼리의 결과를 정렬합니다. orderBy 메소드의 첫번째 인자는 정렬하고자 하는 컬럼이며, 두번째 인자는 asc 또는 desc의 정렬 방식을 제어하는 인자가 됩니다:

```php
$users = DB::table('users')
                ->orderBy('name', 'desc')
                ->get();
```

## latest / oldest
latest 와oldest 메소드는 여러준이 손쉽게 날짜를 기반으로 결과를 정렬할 수 있게 해줍니다. 기본적으로 결과는 created_at 컬럼을 기준으로 정렬됩니다. 또는 정렬에 기준이 되는 컬럼이름을 전달할 수 있습니다:

```php
$user = DB::table('users')
                ->latest()
                ->first();
```

## inRandomOrder 랜덤 정렬
inRandomOrder 메소드는 쿼리 결과를 랜덤하게 정렬하고자 할때 사용할 수 있습니다. 예를 들어, 사용자를 랜덤하게 가져올 때 사용할 수 있습니다:

```php
$randomUser = DB::table('users')
                ->inRandomOrder()
                ->first();
```

## groupBy / having
groupBy 와 hanving 메소드는 쿼리 결과를 그룹화하는데 사용합니다. having 메소드는 where 메소드와 사용장법이 비슷합니다:

```php
$users = DB::table('users')
                ->groupBy('account_id')
                ->having('account_id', '>', 100)
                ->get();
```

멀티 컬럼 groupBy를 위해서 groupBy 메소드에 여러개의 인자를 전달 할 수 있습니다:

```php
$users = DB::table('users')
                ->groupBy('first_name', 'status')
                ->having('account_id', '>', 100)
                ->get();
```

보다 복잡한 having 구문에 대해서는 havingRaw 메소드를 참조하십시오.

## skip / take
쿼리에서 반환되는 결과의 갯수를 제한하거나, 주어진 갯수만큼 결과를 건너뛰기 위해서는, skip 과 take 메소드를 사용하면 됩니다:

```php
$users = DB::table('users')->skip(10)->take(5)->get();
```

대신에, limit 과 offset 메소드를 사용할 수도 있습니다:

```php
$users = DB::table('users')
                ->offset(10)
                ->limit(5)
                ->get();
```

## Conditional-조건적 where 절
때로는 어떠한 조건이 참일 때만 해당 where 절이 쿼리에 적용되는 것을 원할 수도 있습니다. 예를 들어, 현재의 요청에서 주어진 입력값이 존재할 때에만 where 구문을 적용하고 싶을 수도 있습니다. 이 경우 when 메소드를 사용할 수 있습니다:

```php
$role = $request->input('role');

$users = DB::table('users')
                ->when($role, function ($query) use ($role) {
                    return $query->where('role_id', $role);
                })
                ->get();
```

when 메소드는 첫번째 파라미터가 true 일때 주어진 클로저를 실행합니다. 첫번째 파라미터가 false 라면 클로저는 실행되지 않을 것입니다.

when 메소드의 세번째 파라미터로 또다른 클로저를 전달할 수 있습니다. 이 클로저는 첫번째 파라미터가 false 일때 실행됩니다. 다음은 이 기능을 어떻게 사용하는지 보여주기 위한 예로, 쿼리의 기본 정렬을 구성한 것입니다: $sortBy = null;

```php
$users = DB::table('users')
                ->when($sortBy, function ($query) use ($sortBy) {
                    return $query->orderBy($sortBy);
                }, function ($query) {
                    return $query->orderBy('name');
                })
                ->get();
```

## Inserts-삽입
쿼리 빌더는 데이터베이스 테이블에 레코드를 추가하는 insert 메소드를 제공합니다. insert 메소드는 컬럼의 이름과 값으로 이루어진 배열을 인자로 전달받습니다:

```php
DB::table('users')->insert(
    ['email' => 'john@example.com', 'votes' => 0]
);
```

또한 한번의 insert 메소드 호출로 테이블에 여러 레코드를 추가하기 위해서 배열의 배열을 전달 할 수도 있습니다. 각각의 배열은 테이블에 추가되고자 하는 row 를 나타냅니다:

```php
DB::table('users')->insert([
    ['email' => 'taylor@example.com', 'votes' => 0],
    ['email' => 'dayle@example.com', 'votes' => 0]
]);
```

## Auto-Incrementing IDs
테이블이 auto-incrementing id를 가지고 있다면 insertGetId 메소드를 사용하여 레코드를 추가하고, 추가된 ID를 획득할 수 있습니다:

```php
$id = DB::table('users')->insertGetId(
    ['email' => 'john@example.com', 'votes' => 0]
);
```

{note} PostgreSQL에서 insertGetId 메소드를 사용하는 경우 auto-incrementing 컬럼의 이름은 id 이어야 합니다. 다른 이름의 ID를 원한다면는 insertGetId 메서드의 두 번째 인자로 컬름 이름을 전달 하십시오.


## Updates-수정
데이터베이스에 레코드를 삽입하는 것에 더해서 쿼리 빌더는 당연히 이미 존재하는 레코드를 update 메소드를 사용하여 변경할 수 있습니다. update 메소드는 insert 메소드와 마찬가지로, 업데이트 하기 위한 컬럼과 컬럼에 대한 값의 쌍으로 이루어진 배열을 인자로 전달받습니다. update 쿼리에 where 구문을 사용하여 범위를 제한할 수도 있습니다:

```php
DB::table('users')
            ->where('id', 1)
            ->update(['votes' => 1]);
```

## JSON 컬럼 업데이트
JSON 컬럼을 업데이트 할때에는 JSON 객체의 해당 키에 엑세스하기 위해서 -> 문법을 사용해야 합니다. 이 작업은 JSON 컬럼을 지원하는 데이터베이스에서만 지원합니다:

```php
DB::table('users')
            ->where('id', 1)
            ->update(['options->enabled' => true]);
```

## 컬럼값의 증가 & 감소
쿼리빌더는 주어진 컬럼의 값을 증가 또는 감소하는 편리한 메소드를 제공합니다. 이는 단축키와 같으며, 수종으로 update 구문을 작성하는 것과 비교하면 보다 간결한 인터페이스를 제공합니다.

이 두개의 메소드는 최소한 하나의 인자를 전달 받습니다: 수정하고자 하는 컬럼. 두번째 인자는 선택적으로 전달되며, 컬럼이 증가되어야할 또는 감소되어야할 양을 나타냅니다.

```php
DB::table('users')->increment('votes');

DB::table('users')->increment('votes', 5);

DB::table('users')->decrement('votes');

DB::table('users')->decrement('votes', 5);
```

또한 이 작업을 수행동안 업데이트 되어야할 컬럼을 추가적으로 지정할 수도 있습니다:

```php
DB::table('users')->increment('votes', 1, ['name' => 'John']);
```

## Deletes-삭제
쿼리빌더는 테이블에서 레코드를 삭제하는데 delete 메소드를 사용할 수 있습니다. delete 구문에서는 delete 메소드를 호출하기 전에 where 절을 추가하여 제약사항을 설정할 수 있습니다:

```php
DB::table('users')->delete();

DB::table('users')->where('votes', '>', 100)->delete();
```

모든 데이터를 삭제하고 auto-incrementing ID를 0으로 초기화 하는 것과 같이, 테이블 전체를 비우고자 한다면 truncate 메소드를 사용할 수 있습니다:

```php
DB::table('users')->truncate();
```

## Pessimistic Locking
쿼리빌더는 또한 여러분이 select 구문에 “Pessimistic Locking-배타적 잠금”을 설정을 할 수 있도록 도와주는 몇가지 기능을 포함하고 있습니다. "공유 lock"과 함께 구문을 실행하기 위해서, 쿼리에 sharedLock 메소드를 사용할 수 있습니다. 공유 lock 은 선택된 row 가 트랜젝션이 커밋되기 전까지 수정되는 것을 방지합니다:

```php
DB::table('users')->where('votes', '>', 100)->sharedLock()->get();
```

이대신에, lockForUpdate 메소드를 사용할 수 있습니다. "수정을 위한" lock 은 row 가 수정되는 것 또는 다른 공유 lock 에 의해서 선택되는 것을 방지합니다:

```php
DB::table('users')->where('votes', '>', 100)->lockForUpdate()->get();
```

