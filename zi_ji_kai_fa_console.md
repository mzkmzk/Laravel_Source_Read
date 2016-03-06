# 自己开发Console

需求是这样的

我需要一条命令创建多个Controller

而Controller里命令和里面的代码我需要自己从另外几个配置文件中获取

我的做法是,既然`laravel`本身就有生成Controller的功能,我需要先做一个类似的功能,先copy到我的Commadns目录下,然后进行改造.

默认的make:controller现在路径`项目/vendor/laravel/framework/src/Illuminate/Routing/Console/ControllerMakeCommand.php`

先来解析下代码

```php
class ControllerMakeCommand extends GeneratorCommand
{
    /**
     * 命令名称
     *
     * @var string
     */
    protected $name = 'make:controller';

    /**
     * 命令简介
     *
     * @var string
     */
    protected $description = 'Create a new resource controller class';

    /**
     * 说明是生成什么文件的,目前只研究出的作用是,当生成成功时,控制台显示`Controller created successfully`的作用
     *
     * @var string
     */
    protected $type = 'Controller';

    /**
     * 获取模板文件
     *
     * @return string
     */
    protected function getStub()
    {
        if ($this->option('plain')) {
            return __DIR__.'/stubs/controller.plain.stub';
        }

        return __DIR__.'/stubs/controller.stub';
    }

    /**
     * 获取默认工作空间
     *
     * @param  string  $rootNamespace
     * @return string
     */
    protected function getDefaultNamespace($rootNamespace)
    {
        return $rootNamespace.'\Http\Controllers';
    }

    /**
     * 设置参数,当声明了--plain 说明只要生成空的Controller,`InputOption::VALUE_NONE`代表这个参数可有可无.
     *
     * @return array
     */
    protected function getOptions()
    {
        return [
            ['plain', null, InputOption::VALUE_NONE, 'Generate an empty controller class.'],
        ];
    }
}

```
OK 目标明确了,我需要改造这个文件

我遇到的主要问题有

###1. 控制台强制要求输入Controller名称

父类`GeneratorCommand`中会默认通过`getArguments`获取参数,这个参数本来是用来获取生成Controller的名称的,但是因为我需要生成的Controller是多个的,是从代码中获取,而不是控制台.

父类的获取参数方法,会要求name是必须的填写的.
```php
 protected function getArguments()
    {
        return [
            ['name', InputArgument::REQUIRED, 'The name of the class'],
        ];
    }
```

而我的Console是不需name参数的所以,要在子类中覆盖父类的方法

```php
 protected function getArguments()
    {
        return [
            //['name', InputArgument::REQUIRED, 'The name of the class'],
        ];
    }
```

###2. 如何传输Controller的名称呢?

但是我们如何从代码中获取生成Controller的名称呢?答案:构造方法

通过观察到父类的父类`GeneratorCommand`有构造方法,

然后父类的父类的父类`Command`有个传递参数的方法`addArgument`,代码如下

```php
/**
     * Adds an argument.
     *
     * @param string $name        The argument name
     * @param int    $mode        The argument mode: InputArgument::REQUIRED or InputArgument::OPTIONAL
     * @param string $description A description text
     * @param mixed  $default     The default value (for InputArgument::OPTIONAL mode only)
     *
     * @return Command The current instance
     */
    public function addArgument($name, $mode = null, $description = '', $default = null)
    {
        $this->definition->addArgument(new InputArgument($name, $mode, $description, $default));

        return $this;
    }

```