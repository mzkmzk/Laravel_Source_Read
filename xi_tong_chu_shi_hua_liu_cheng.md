# 系统初始化流程

# 总体

1. 入口文件`public/index.php`
2. 加载`bootstrap/autoload.php`,主要内容为加载`composer`相关的php文件
3. 加载`bootstrap/app.php`

  1. 实例化application

      ```php
      $app = new Illuminate\Foundation\Application(
        realpath(__DIR.'/../')
      )
      ```
      
      看看实例化的具体函数
      
      ```php
       public function __construct($basePath = null)
    {
        //注册最基本的容器
        $this->registerBaseBindings();
        //
        $this->registerBaseServiceProviders();
        $this->registerCoreContainerAliases();
        if ($basePath) {
            $this->setBasePath($basePath);
        }
    }

      ```

