# Redis

##1. 基本使用

环境为Mac_10.10


###1.1 安装Redis
安装wget`brew install wget`

    ==> Summary
    🍺  /usr/local/Cellar/openssl/1.0.1j_1: 431 files, 15M
    ==> Installing wget
    ==> Downloading https://downloads.sf.net/project/machomebrew/Bottles/wget-1.16.1.yosemite.bottle.tar.gz
    ######################################################################## 100.0%
    ==> Pouring wget-1.16.1.yosemite.bottle.tar.gz
    🍺  /usr/local/Cellar/wget/1.16.1: 9 files, 940K
    
服务器安装redis

参考<http://redis.io/download>

    wget http://download.redis.io/releases/redis-3.0.6.tar.gz
    tar xzf redis-3.0.6.tar.gz
    cd redis-3.0.6
    make
    make test
    
    //启动服务器的 默认监听6379
    src/redis-server
    
    //启动客户端
    src/redis-cli
    