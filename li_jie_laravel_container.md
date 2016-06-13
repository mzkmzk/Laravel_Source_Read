# 理解Laravel Container

Laravel中Container中实现主要在`Illuminate/Containner/Container.php`中

# 1. 实现ContainerContract接口

```php
class Container implements ArrayAccess, ContainerContract
```

Container主要实现这两个接口

首先分析`ContainerContract`接口需要实现何接口

## 1.1  bound: 判断接口是否被绑定

实现:

```php
    /**
     * Determine if the given abstract type has been bound.
     *
     * @param  string  $abstract
     * @return bool
     */
    public function bound($abstract)
    {
        return isset($this->bindings[$abstract]) || isset($this->instances[$abstract]) || $this->isAlias($abstract);
    }
```

## 1.2 alias: 设置接口别名

```php
    /**
     * Alias a type to a different name.
     *
     * @param  string  $abstract
     * @param  string  $alias
     * @return void
     */
    public function alias($abstract, $alias)
    {
        $this->aliases[$alias] = $abstract;
    }
```