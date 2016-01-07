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
        //调用构造方法.
        $model = new static($attributes);
        
        $model->save();

        return $model;
    }
```

2. create()方法时指定可设置属性/指定不可设置属性

文档里也有说明

在Model里可以设置

1. `$fillable`数组放置可设置的属性
2. `$guarded`数组里放置不可设置的属性

```php

```

MassAssignmentException in Model.php line 427:
name