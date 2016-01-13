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
