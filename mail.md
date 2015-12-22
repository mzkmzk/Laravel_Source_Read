#Mail

##1. 安装

        sudo composer require guzzlehttp/guzzle  ~5.3|~6.0

##2. 配置

主要配置`config/mail.php`

    'driver' => 'smtp',
    'host' => 'smtp.qq.com',
    'port' => 465,
    'from' => ['address' => '2326382007@qq.com', 'name' => 'HapLab'],
    'encryption' => 'ssl',
    'username' => '麦哥@qq.com',
    'password' => '***',//注意此密码不是账号密码,而是开启STMP后的密码
    'sendmail' => '/usr/sbin/sendmail -bs',
    'pretend' => false,
    
##3. 发送

###3.1 基本发送

        Mail::send('发送的vie', $data, function($message) use($data)
        {
            $message->to('收件人@qq.com', '')->subject('欢迎注册我们的网站，请激活您的账号！');
        });
    

## 参考链接

Laravel官网<http://laravel.com/docs/5.1/mail>

配置QQ邮箱<https://lvwenhan.com/laravel/436.html>