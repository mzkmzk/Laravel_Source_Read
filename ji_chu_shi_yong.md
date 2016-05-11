# 基础使用

## 1.基础使用 

1. 创建Providers

    ```php
    php artisan make:provider ReleaseServiceProvider
    ```
    
    里面主要有两个方法
    
    1. boot: 负责引导和初始化,这里不要对未知事物的依赖
    2. register: 负责注册,可以对未知事物的依赖

2. 注册和使用

     现在很简单的一个需求,一个公用类ReleasePre,希望注入到ActivityController中
     
     1. register()中声明

      ```php
      class ReleaseServiceProvider extends ServiceProvider
      {
          public function boot()
          {
          }

          public function register()
          {
              $this->app->when('App\Http\Controllers\ActivityController')
                  ->needs('App\Services\ReleasePre')
                  ->give('App\Services\ReleasePre');
          }
      }

      ```


## 参考链接

讲解IOC<https://phphub.org/topics/789>
    