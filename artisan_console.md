# Artisan Console

##1. 基本使用

1. 生成Command类

        php artisan make:console Command_Name
        
        or
        
        php artisan make:console Command_Name --command=emails:send
    
    区别就是下者会顺便给Command指定一个命令名称,
    
2. 指定命令名称和简述

        class Send_Emails extends Command{
            protected $signature = 'Send_Emails';
            protected $description = '进度监控邮件Email服务';
            public function __construct()
            {
                parent::__construct();
            }
        
            public function handle()
            {
                //
            }
        }
    可以在构造方法中依赖注入一个类,
    
    handle则为处理的函数
    
3. 在`Kernel.php`载入类

         protected $commands = [
            ...
            \App\Console\Commands\Send_Emails::class,
        ];
        
    然后`php artisan list`就可以看到你的命令了
    
