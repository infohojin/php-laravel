## Raw 쿼리 실행
일단 데이터베이스 연결을 설정하면 DB 파사드를 사용해서 쿼리를 실행 할 수 있습니다. 
DB 파사드는 각각 쿼리 타입에 해당하는 메소드 : select, update, delete 그리고 statement 메소드를 제공합니다.

<br>

### Select 쿼리 실행하기
기본적인 쿼리를 실행하기 위해서 DB 파사드의 select 메소드를 사용할 수 있습니다:

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
        $users = DB::select('select * from users where active = ?', [1]);

        return view('user.index', ['users' => $users]);
    }
}
```

select 메소드의 첫번째 인자는 raw SQL 쿼리이고, 두번째는 쿼리에 바이딩될 매개변수 입니다. 
일반적으로 매개 변수들은 where 절을 위한 값들입니다. 
매개변수 바인딩은 SQL 인젝션을 방지하기 위해 제공됩니다.

select 메소드는 항상 결과를 배열로 반환합니다. 
배열안의 값들은 PHP 의 StdClass 객체 형태로 다음과 같이 결과 값에 엑세스 할 수 있습니다.

```php
foreach ($users as $user) {
    echo $user->name;
}
```

### 이름이 부여된 바인딩 사용하기
`?` 를 사용하는 매개변수 바인딩 대신에, 이름을 지정한 바인딩을 사용한 쿼리를 실행시킬 수 있습니다:

```php
$results = DB::select('select * from users where id = :id', ['id' => 1]);
```

### Insert 문 실행하기
insert 쿼리문을 실행하기 위해서는 DB 파사드의 insert 메소드를 사용하면 됩니다. 
select 메소드와 마찬가지로, 이 메소드는 첫번째 인자로 raw SQL 쿼리를, 두번째로 바인딩할 인자들을 전달 받습니다:

```php
DB::insert('insert into users (id, name) values (?, ?)', [1, 'Dayle']);
```

### Update 쿼리 실행하기
update 메소드는 데이터베이스에 존재하는 레코드 업데이트 하는데 사용되어집니다. 
그 결과 영향을 받은 레코드들의 갯수가 반환될 것입니다:

```php
$affected = DB::update('update users set votes = 100 where name = ?', ['John']);
```


### Delete 쿼리 실행하기
delete 메소드는 데이터베이스에서 레코드를 삭제하는데 사용됩니다. 
update 와 같이, 영향을 받은 레코드 갯수가 반환됩니다:

```php
$deleted = DB::delete('delete from users');
```

### 일반적인 구문의 쿼리 실행하기
몇몇 데이터베이스 구문은 값을 반환하지 않습니다. 
이러한 유형의 작업들을 위해서는 DB 파사드의 statement 메소드를 사용할 수 있습니다:

```php
DB::statement('drop table users');
```

## 쿼리 이벤트 리스닝
애플리케이션에서 실행되는 각각의 쿼리를 확인하고자 한다면 listen 메소드를 사용하면 됩니다. 
이 메소드는 쿼리를 로그로 남기거나, 디버깅 할 때 유용합니다.

서비스 프로바이더에서 쿼리 리스너를 등록할 수 있습니다:

```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\DB;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        DB::listen(function ($query) {
            // $query->sql
            // $query->bindings
            // $query->time
        });
    }

    /**
     * Register the service provider.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}
```
