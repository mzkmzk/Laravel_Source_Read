# 常见错误

###1. env中文问题

报错信息如下

```shell
我执行 "php artisan"

Fatal error: Uncaught exception 'ReflectionException' with message 'Class log does not exist' in /Users/maizhikun/Learning/apache_sites/haplox-wechat/bootstrap/cache/compiled.php on line 1318

ReflectionException: Class log does not exist in /Users/maizhikun/Learning/apache_sites/haplox-wechat/bootstrap/cache/compiled.php on line 1318
```

当env中出现中文,无论执行任何的`artisan`都报错.不能存在中文.