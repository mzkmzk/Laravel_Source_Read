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

如果不设置的话,在使用create方法时可能会报错,因为create调用了fill方法,在fill方法就会检查设置的属性是否可设置,看一下限制的源码

```php
public function fill(array $attributes)
{
    $totallyGuarded = $this->totallyGuarded();

    foreach ($this->fillableFromArray($attributes) as $key => $value) {
        $key = $this->removeTableFromKey($key);

        // The developers may choose to place some attributes in the "fillable"
        // array, which means only those attributes may be set through mass
        // assignment to the model, and all others will just be ignored.
        if ($this->isFillable($key)) {
            $this->setAttribute($key, $value);
        } elseif ($totallyGuarded) {
            throw new MassAssignmentException($key);
        }
    }

    return $this;
}
```
注意一下这个方法里首先访问了`totallyGuarded`方法

```php
public function totallyGuarded()
{
    return count($this->fillable) == 0 && $this->guarded == ['*'];
}
```

public function isFillable($key)
{
    if (static::$unguarded) {
        return true;
    }

    // If the key is in the "fillable" array, we can of course assume that it's
    // a fillable attribute. Otherwise, we will check the guarded array when
    // we need to determine if the attribute is black-listed on the model.
    // 如果key在`fillable`数组中,
    //error_log(__CLASS__ . json_encode($this->fillable));
    if (in_array($key, $this->fillable)) {
        return true;
    }

    if ($this->isGuarded($key)) {
        return false;
    }

    return empty($this->fillable) && ! Str::startsWith($key, '_');
}


```

MassAssignmentException in Model.php line 427:
name