## 데이터베이스 트랜잭션
일련의 쿼리들을 하나의 데이터베이스 트랜잭션으로 실행시키기 위해서 DB 파사드의 transaction메소드를 사용할 수 있습니다. 
트랜잭션 메소드에 전달된 Closure 안에서 예외-exception 이 발생하게 되면 트랜잭션은 자동으로 롤백됩니다. 
Closure가 성공적으로 실행되면 트랜잭션은 자동으로 커밋됩니다. 
transaction 메소드를 사용하게 되면 일일이 롤백과 커밋에 대해서 걱정할 필요가 없습니다:

```php
DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
});
```


### 데드락 처리하기
transaction 메소드는 데드락이 발생하면 트랜젝션을 재시도 해야 하는 횟수를 정의하는 두번째 인자를 선택적으로 받아들입니다. 
이러한 시도가 모두 종료되면, exception이 발생합니다:

```php
DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
}, 5);
```


### 수동으로 트랙잭션 사용하기
트랜잭션을 직접 시작하고 롤백과 커밋을 제어하고 싶은 경우는 DB 파사드의 beginTransaction 메소드를 사용하면 됩니다:

```php
DB::beginTransaction();
rollBack 메소드는 트랜잭션을 롤백 할 수 있습니다:

DB::rollBack();
마지막으로 commit 메소드는 트랜잭션을 커밋 할 수 있습니다:

DB::commit();
```

{tip}  
DB 파사드의 트랜잭션 메소드는 쿼리 빌더 와 Eloquent ORM에 모두에서, 트랜잭션을 제어 할 수도 있습니다.

