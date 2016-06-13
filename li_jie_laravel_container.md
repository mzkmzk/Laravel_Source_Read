# 理解Laravel Container

Laravel中Container中实现主要在`Illuminate/Containner/Container.php`中

# 1. 实现ContainerContract接口

```php
class Container implements ArrayAccess, ContainerContract
```

Container主要实现这两个接口

首先分析`ContainerContract`接口需要实现何接口

## 1.1  bound($abstract): 判断接口是否被绑定

