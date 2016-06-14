# 理解Laravel Container

Laravel中Container中实现主要在`Illuminate/Containner/Container.php`中

# 1. 实现ContainerContract接口

```php
class Container implements ArrayAccess, ContainerContract
```

Container主要实现这两个接口

首先分析`ContainerContract`接口需要实现何方法

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

## 1.5 bind: 注册接口道容器中

实现

```php
    /**
     * Register a binding with the container.
     *
     * @param  string|array  $abstract
     * @param  \Closure|string|null  $concrete
     * @param  bool  $shared
     * @return void
     */
    public function bind($abstract, $concrete = null, $shared = false)
    {
        //当接口为["接口名"=>"别名"]时这样绑定接口名和别名,这种情况docs无提及
        if (is_array($abstract)) {
            list($abstract, $alias) = $this->extractAlias($abstract);

            $this->alias($abstract, $alias);
        }

        //删除接口当前绑定的instances和aliases
        $this->dropStaleInstances($abstract);

        //当没有具体的实例,则使用接口为实体
        if (is_null($concrete)) {
            $concrete = $abstract;
        }

        //当实体为闭包时(实际大多数情况如此),
        if (! $concrete instanceof Closure) {
            $concrete = $this->getClosure($abstract, $concrete);
        }

        //绑定接口
        $this->bindings[$abstract] = compact('concrete', 'shared');

        //若接口已绑定,则重新装载
        if ($this->resolved($abstract)) {
            $this->rebound($abstract);
        }
    }
```

`$this->getClosure`的实现需要注意一下


```php
protected function getClosure($abstract, $concrete)
{
    return function ($c, $parameters = []) use ($abstract, $concrete) {
        $method = ($abstract == $concrete) ? 'build' : 'make';

        return $c->$method($concrete, $parameters);
    };
}
```

## 1.6 bindIf: 如果绑定了重新绑定

```php
    /**
     * Register a binding if it hasn't already been registered.
     *
     * @param  string  $abstract
     * @param  \Closure|string|null  $concrete
     * @param  bool  $shared
     * @return void
     */
    public function bindIf($abstract, $concrete = null, $shared = false)
    {
        if (! $this->bound($abstract)) {
            $this->bind($abstract, $concrete, $shared);
        }
    }
```

## 1.7 singleton: 绑定共享单例

```php
    /**
     * Register a shared binding in the container.
     *
     * @param  string|array  $abstract
     * @param  \Closure|string|null  $concrete
     * @return void
     */
    public function singleton($abstract, $concrete = null)
    {
        $this->bind($abstract, $concrete, true);
    }
```

## 1.8 extend: 拓展接口

```php
    /**
     * "Extend" an abstract type in the container.
     *
     * @param  string    $abstract
     * @param  \Closure  $closure
     * @return void
     *
     * @throws \InvalidArgumentException
     */
    public function extend($abstract, Closure $closure)
    {
        if (isset($this->instances[$abstract])) {
            $this->instances[$abstract] = $closure($this->instances[$abstract], $this);

            $this->rebound($abstract);
        } else {
            $this->extenders[$abstract][] = $closure;
        }
    }
```

## 1.9 instance: 绑定实例

```php
/**
   * Register an existing instance as shared in the container.
   *
   * @param  string  $abstract
   * @param  mixed   $instance
   * @return void
   */
  public function instance($abstract, $instance)
  {
     //先抽绑定接口=>别名,然后把无别名的接口去除掉
      if (is_array($abstract)) {
          list($abstract, $alias) = $this->extractAlias($abstract);

          $this->alias($abstract, $alias);
      }

      unset($this->aliases[$abstract]);

      //如果发生并发绑定,则重新装载
      $bound = $this->bound($abstract);

      $this->instances[$abstract] = $instance;

      if ($bound) {
          $this->rebound($abstract);
      }
  }
```