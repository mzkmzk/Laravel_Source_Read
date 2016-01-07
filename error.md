# error

#1. 不要在自己的Model下,创建构造方法

因为Model本来就有构造方法

```php
 public function __construct(array $attributes = [])
    {
        $this->bootIfNotBooted();
        $this->syncOriginal();
        $this->fill($attributes);
    }
    
```

如果你重写了构造方法.在使用Model::Create()的时候就会被坑啦..除非你能实现上述功能

```php
 public static function create(array $attributes = [])
    {
        $model = new static($attributes);
        
        $model->save();

        return $model;
    }
```
