# 基础使用

## 1.基础使用 

1. 创建Providers

    ```php
    php artisan make:provider RiakServiceProvider
    ```
    
    里面主要有两个方法
    
    1. boot: 负责引导和初始化,这里不要对未知事物的依赖
    2. register: 负责注册,可以对未知事物的依赖


## 参考链接

讲解IOC<https://phphub.org/topics/789>
    