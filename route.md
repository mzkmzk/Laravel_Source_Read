# Route

## 1.无法通过group获取参数名称

```php
Route::group(['prefix' => 'v1/{Entity_Controller}'], function (){
  //此处无法获取Entity_Controller,只能在下属的get/post/match中作为参数获取.
 Route::get('query',function($Entity_Controller){

    });
});
```