# 自己开发Console

需求是这样的

我需要一条命令创建多个Controller

而Controller里命令和里面的代码我需要自己从另外几个配置文件中获取

我的做法是,既然`laravel`本身就有生成Controller的功能,我需要先做一个类似的功能,先copy到我的Commadns目录下,然后进行改造.

默认的make:controller现在路径`项目/vendor/laravel/framework/src/Illuminate/Routing/Console/ControllerMakeCommand.php`

