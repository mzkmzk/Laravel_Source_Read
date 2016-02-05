# Service Providers

服务提供者

##1. 基本使用

1. 生产一个提供者

        php artisan make:provider RiakServiceProvider

    可以看到一个提供者有几个需要的条件.
    
    1. 继承`Illuminate\Support\ServiceProvider`
    2. boot()方法
    3. register()方法