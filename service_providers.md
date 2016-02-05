# Service Providers

服务提供者

##1. 基本使用

1. 生产一个提供者

        php artisan make:provider RiakServiceProvider

    可以看到一个提供者有几个需要的条件.
    
    1. 继承`Illuminate\Support\ServiceProvider`
    2. boot()方法,
    3. register()方法,只允许进行绑定处理.如果容器没有依赖任何的接口,则没有必要把容器绑定到提供者当中.

2. 绑定(可选)

    `$this->app`可获得实例
    1. 
    2. 绑定单例
    3. 绑定实例
3. 

    