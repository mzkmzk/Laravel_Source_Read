# Task

需求是:每日9点进行一次数据查询,把结果整理发送邮件

参考地址<https://laravel.com/docs/5.1/scheduling>

##1. 先设置好命令,看` Artisan Console`章节.


##2. Kernel设置命令周期

在`app/Console/Console/Kernel.php`中设置

```php
 protected function schedule(Schedule $schedule)
    {
        $schedule->command('Send_Emails')
            ->dailyAt('09:00');
    }
```

`Send_Emails`是我在步骤1总设置好的命令

##3. 将schedule配置提交到系统

输入命令`crontab -e` 然后编辑下列字符串.

    * * * * * /usr/local/php5-5.6.11-20150710-223902/bin/php /Users/maizhikun/Learning/apache_sites/HapLab_Laravel/artisan  schedule:run
    
大家注意我的php 和我项目的路径都是写着全路径,以免有环境变量问题

``crontab -l` 可查看任务列表

然后就基本OK了

系统任务配置参考<http://linuxtools-rst.readthedocs.org/zh_CN/latest/tool/crontab.html>
