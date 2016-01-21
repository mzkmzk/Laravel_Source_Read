#Mail

##1. 安装

        sudo composer require guzzlehttp/guzzle  ~5.3|~6.0

##2. 配置

主要配置`.env`

普通QQ邮箱

    MAIL_DRIVER=smtp
    MAIL_HOST=smtp.qq.com
    MAIL_NAME=HapLab
    MAIL_PORT=465
    MAIL_USERNAME=23263820@qq.com
    MAIL_PASSWORD=lachsogxbfqae等特定密码
    MAIL_ENCRYPTION=ssl

企业qq邮箱

    MAIL_DRIVER=smtp
    MAIL_HOST=smtp.exmail.qq.com
    MAIL_NAME=随便起一个名字
    MAIL_PORT=465
    MAIL_USERNAME=邮箱账号
    MAIL_PASSWORD=明文密码
    MAIL_ENCRYPTION=ssl
    
##3. 发送

###3.1 基本发送

        Mail::send('发送的vie', $data, function($message) use($data)
        {
            `
        });
    

## 参考链接

Laravel官网<http://laravel.com/docs/5.1/mail>

配置QQ邮箱<https://lvwenhan.com/laravel/436.html>