
### 프로바이더 등록하기
---

모든 서비스 프로바이더들은 config/app.php 설정 파일에 등록되어 있습니다. 

이 파일에는 서비스 프로바이더들의 클래스 이름을 나열하고 등록할 수 있는 providers 배열이 포함되어 있습니다. 기본적으로는 라라벨의 코어 서비스 프로바이더들이 배열에 나열되어 있습니다. 이 프로바이더들이 라라벨의 메일러, 큐, 캐시등과 같은 핵심적인 컴포넌트들을 부트스트랩핑 하게 됩니다.

여러분의 프로바이더들을 등록하려면 이 배열에 추가 하면 됩니다:

```php
'providers' => [
    // Other Service Providers
    App\Providers\ComposerServiceProvider::class,
],
```

