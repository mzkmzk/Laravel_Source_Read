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

实现:

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

## 1.3 tag: 给接口设置标志

实现:

```php
    /**
     * Assign a set of tags to a given binding.
     *
     * @param  array|string  $abstracts
     * @param  array|mixed   ...$tags
     * @return void
     */
    public function tag($abstracts, $tags)
    {
        $tags = is_array($tags) ? $tags : array_slice(func_get_args(), 1);

        foreach ($tags as $tag) {
            if (! isset($this->tags[$tag])) {
                $this->tags[$tag] = [];
            }

            foreach ((array) $abstracts as $abstract) {
                $this->tags[$tag][] = $abstract;
            }
        }
    }
```

函数的作用给每个接口都分配在指定的tag中

## 1.4 tagged: 制造tag下的所有接口

实现:

```php
    /**
     * Resolve all of the bindings for a given tag.
     *
     * @param  string  $tag
     * @return array
     */
    public function tagged($tag)
    {
        $results = [];

        if (isset($this->tags[$tag])) {
            foreach ($this->tags[$tag] as $abstract) {
                $results[] = $this->make($abstract);
            }
        }

        return $results;
    }
```