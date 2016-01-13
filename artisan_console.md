# Artisan Console

##1. 基本使用

1. 生成Command类

        php artisan make:console Command_Name
        
        or
        
        php artisan make:console Command_Name --command=emails:send
    
    区别就是下者会顺便给Command指定一个命令名称,
    
