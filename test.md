# Test

规则:

1. 测试的类名和文件名要以Test结尾.
2. 方法名要以test开头. 
3. 文件名不能重复(即使在tests的不同子目录下).
4. `tests`目录下的`TestCase.php`并不能删除.然而`TestCase.php`文件中的`$baseurl`并没什么卵用.
5. `cd`到项目中phpunit就可以执行测试了..(对了 官网doucment的`php artisan make:test UserTest`是无法执行了,作者也表示会修改Document或在未来添加这个命令).


来一发Demo.

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
	
一开始我是想在测试每个方法的时候输出一个语句表示测试到哪方法,但是没找到获取哪个类的哪个方法调用了测试方法的方法- -..所以只能输出在测试哪一个类..

修改TestCase.php,增加了`error_log("正在测试: " . get_class($this));`这么一句,结果是执行了多少个测试方法就输出多少次类名...求教如何输出方法..

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
	
额...对了重写setUp()方法会在每个测试方法之前都调用..和我们之前实现的效果一样.

注意`__construct方法`也是每测试一个方法执行一次的.因为每个测试方法是不同的实例,如果想共享变量,请用`static`.


#遇到的错误

1. 程序报错

	1) User_Test::test_Insert
	ErrorException: Invalid argument supplied for foreach()
	
	原因是seeJson需要返回的是JSON,当返回无法JSON格式化的数据,大多数报这个错误.

	但可恶的是这些错误不会报到报错文件!!所以建议实践操作一次.看错误内容是神马.

	虽然在控制台会显示报错,但是偶尔会显示不全所有报错信息.

	[参考](http://stackoverflow.com/questions/31921451/laravel-5-1-phpunit-api-test-returns-always-invalid-argument-error-foreach)	,

2. 伪集合数组问题
		
		//代码
		 $this->post('/Controller/User_Controller/insert', ['id' =>  $this::$id , 'username' => $this::$id])
            ->seeJson([
                'result' => "true",
            ]);
		
		//报错
		1) User_Test::test_Insert
		Unable to find JSON fragment ["result":"true"] within [{"result":true}].
		Failed asserting that false is true.
		
	第一眼看上去以为是前面是数组里的属性,而后面是数组里对象的属性的原因....万万没想到..是因为true前后的双引号的原因..在测试类中去掉就好了	
	
3. 数组需要包含所有属性问题

	问题是我需要检验一个对象中的部分属性,好像实现不了.我的代码
	
		 public function test_Query()
    	{
        	$this->post('/Controller/User_Controller/query', ['id' => $this::$id])
            ->seeJson([
                'data'   => [ 0 =>['username' =>  $this::$id.'_update']],
            ]);
    	}
		
		//测试结果
		1) User_Test::test_Query
		Unable to find JSON fragment ["data":[{"username":"1137598_update"}]] within [{"current_page":1,"data":[{"created_at":"2015-10-29 18:57:13","deleted_at":"0000-00-00 00:00:00","email":"","id":1137598,"last_login_at":"0000-00-00 00:00:00","login_times":0,"name":"","phone":"","privilege":1,"status":0,"updated_at":"2015-10-29 18:57:13","username":"1137598_update"}],"from":1,"last_page":1,"next_page_url":null,"per_page":20,"prev_page_url":null,"to":1,"total":1}].
		Failed asserting that false is true.	