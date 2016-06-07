# Route原理讲解

# 1. Route使用

这里以post为例,简单对laravel实现Route功能的原理进行解析

基本的调用方法

```php
Route::post('test','TestController@test');
```

# 2. 解析

主要问题有

1. Route::post里的调用的是哪里的方法

