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
