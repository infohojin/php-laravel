---
layout: laravel
title: Laravel
subtitle: Contracts
cate:
    name: "컨셉"
    link: "/Laravel/Contracts"
---

## Contract 참조
---

아래는 대부분의 라라벨 Contract와 그에 대응되는 파사드들의 레퍼런스입니다.

Contract	References Facade
Illuminate\Contracts\Auth\Access\Authorizable	  
Illuminate\Contracts\Auth\Access\Gate	Gate
Illuminate\Contracts\Auth\Authenticatable	  
Illuminate\Contracts\Auth\CanResetPassword	 
Illuminate\Contracts\Auth\Factory	Auth
Illuminate\Contracts\Auth\Guard	Auth::guard()
Illuminate\Contracts\Auth\PasswordBroker	Password::broker()
Illuminate\Contracts\Auth\PasswordBrokerFactory	Password
Illuminate\Contracts\Auth\StatefulGuard	 
Illuminate\Contracts\Auth\SupportsBasicAuth	 
Illuminate\Contracts\Auth\UserProvider	 
Illuminate\Contracts\Bus\Dispatcher	Bus
Illuminate\Contracts\Bus\QueueingDispatcher	Bus::dispatchToQueue()
Illuminate\Contracts\Broadcasting\Factory	Broadcast
Illuminate\Contracts\Broadcasting\Broadcaster	Broadcast::connection()
Illuminate\Contracts\Broadcasting\ShouldBroadcast	 
Illuminate\Contracts\Broadcasting\ShouldBroadcastNow	 
Illuminate\Contracts\Cache\Factory	Cache
Illuminate\Contracts\Cache\Lock	 
Illuminate\Contracts\Cache\LockProvider	 
Illuminate\Contracts\Cache\Repository	Cache::driver()
Illuminate\Contracts\Cache\Store	 
Illuminate\Contracts\Config\Repository	Config
Illuminate\Contracts\Console\Application	 
Illuminate\Contracts\Console\Kernel	Artisan
Illuminate\Contracts\Container\Container	App
Illuminate\Contracts\Cookie\Factory	Cookie
Illuminate\Contracts\Cookie\QueueingFactory	Cookie::queue()
Illuminate\Contracts\Database\ModelIdentifier	 
Illuminate\Contracts\Debug\ExceptionHandler	 
Illuminate\Contracts\Encryption\Encrypter	Crypt
Illuminate\Contracts\Events\Dispatcher	Event
Illuminate\Contracts\Filesystem\Cloud	Storage::cloud()
Illuminate\Contracts\Filesystem\Factory	Storage
Illuminate\Contracts\Filesystem\Filesystem	Storage::disk()
Illuminate\Contracts\Foundation\Application	App
Illuminate\Contracts\Hashing\Hasher	Hash
Illuminate\Contracts\Http\Kernel	 
Illuminate\Contracts\Mail\MailQueue	Mail::queue()
Illuminate\Contracts\Mail\Mailable	 
Illuminate\Contracts\Mail\Mailer	Mail
Illuminate\Contracts\Notifications\Dispatcher	Notification
Illuminate\Contracts\Notifications\Factory	Notification
Illuminate\Contracts\Pagination\LengthAwarePaginator	 
Illuminate\Contracts\Pagination\Paginator	 
Illuminate\Contracts\Pipeline\Hub	 
Illuminate\Contracts\Pipeline\Pipeline	 
Illuminate\Contracts\Queue\EntityResolver	 
Illuminate\Contracts\Queue\Factory	Queue
Illuminate\Contracts\Queue\Job	 
Illuminate\Contracts\Queue\Monitor	Queue
Illuminate\Contracts\Queue\Queue	Queue::connection()
Illuminate\Contracts\Queue\QueueableCollection	 
Illuminate\Contracts\Queue\QueueableEntity	 
Illuminate\Contracts\Queue\ShouldQueue	 
Illuminate\Contracts\Redis\Factory	Redis
Illuminate\Contracts\Routing\BindingRegistrar	Route
Illuminate\Contracts\Routing\Registrar	Route
Illuminate\Contracts\Routing\ResponseFactory	Response
Illuminate\Contracts\Routing\UrlGenerator	URL
Illuminate\Contracts\Routing\UrlRoutable	 
Illuminate\Contracts\Session\Session	Session::driver()
Illuminate\Contracts\Support\Arrayable	 
Illuminate\Contracts\Support\Htmlable	 
Illuminate\Contracts\Support\Jsonable	 
Illuminate\Contracts\Support\MessageBag	 
Illuminate\Contracts\Support\MessageProvider	 
Illuminate\Contracts\Support\Renderable	 
Illuminate\Contracts\Support\Responsable	 
Illuminate\Contracts\Translation\Loader	 
Illuminate\Contracts\Translation\Translator	Lang
Illuminate\Contracts\Validation\Factory	Validator
Illuminate\Contracts\Validation\ImplicitRule	 
Illuminate\Contracts\Validation\Rule	 
Illuminate\Contracts\Validation\ValidatesWhenResolved	 
Illuminate\Contracts\Validation\Validator	Validator::make()
Illuminate\Contracts\View\Engine	 
Illuminate\Contracts\View\Factory	View
Illuminate\Contracts\View\View	View::make()