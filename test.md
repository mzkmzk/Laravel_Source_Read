# Test

没测试,你就不能说你的项目是健壮的.

##1. 基本使用

测试文件名和方法的规则:

1. 测试的类名和文件名要以Test结尾.
2. 方法名要以test开头. 
3. 文件名不能重复(即使在tests的不同子目录下).
4. `tests`目录下的`TestCase.php`并不能删除.然而`TestCase.php`文件中的`$baseurl`并没什么卵用.
5. `cd`到项目中phpunit就可以执行测试了..(对了 官网doucment的`php artisan make:test UserTest`是无法执行了,作者也表示会修改Document或在未来添加这个命令).


来一发Demo.
```php
	<?php

	use Illuminate\Foundation\Testing\WithoutMiddleware;
	use Illuminate\Foundation\Testing\DatabaseMigrations;
	use Illuminate\Foundation\Testing\DatabaseTransactions;

	class User_Test extends TestCase
	{
    	public function testQuery()
    	{
        	$this->post('/Controller/User_Controller/query', ['id' => '1'])
            	->seeJson([
                	'username' => "admin",
            	]);
    	}
	}
```	
一开始我是想在测试每个方法的时候输出一个语句表示测试到哪方法,但是没找到获取哪个类的哪个方法调用了测试方法的方法- -..所以只能输出在测试哪一个类..

修改TestCase.php,增加了`error_log("正在测试: " . get_class($this));`这么一句,结果是执行了多少个测试方法就输出多少次类名...求教如何输出方法..
```php
class TestCase extends Illuminate\Foundation\Testing\TestCase
{
    /**
    * The base URL to use while testing the application.
    *
    * @var string
    */
    protected $baseUrl = 'http://localhost';

    /**
    * Creates the application.
    *
    * @return \Illuminate\Foundation\Application
    */
    public function createApplication()
    {
        $app = require __DIR__.'/../bootstrap/app.php';
        error_log("正在测试: " . get_class($this));
        $app->make(Illuminate\Contracts\Console\Kernel::class)->bootstrap();

        return $app;
    }
}
```	
额...对了重写setUp()方法会在每个测试方法之前都调用..和我们之前实现的效果一样.

注意`__construct方法`也是每测试一个方法执行一次的.因为每个测试方法是不同的实例,如果想共享变量,请用`static`.


