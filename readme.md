# Reids
## 目录
* [Redis安装](#Redis安装)
* [Redis配置](#Reids配置)
* [服务端和客户端命令](#服务端和客户端命令)
* [数据库操作](#数据库操作)
    * [数据类型](#数据类型)
        * [string](#string)
        * [hash](#hash)
        * [list](#list)
        * [set](#set)
        * [zset](#zset)
    * [键命令](#键命令)

* [与python交互](#与python交互)
* [搭建主从](#搭建主从)
* [搭建集群](#搭建集群)

# Redis安装
* ## step1:下载
    >wget http://download.redis.io/releases/redis-4.0.9.tar.gz
* ## step2:解压
    >tar xzf redis-4.0.9.tar.gz
* ## step3:移动，放到usr/local⽬录下
    >sudo mv ./redis-4.0.9 /usr/local/redis/
* ## step4:进⼊redis⽬录
    >cd /usr/local/redis/
* ## step5:生成
    >sudo make
* ## step6:测试,这段运⾏时间会较⻓
    >sudo make test
* ## step7:安装,将redis的命令安装到/usr/local/bin/⽬录
    >sudo make install  
* ## step8:安装完成后，进入目录/usr/local/bin中查看
    >cd /usr/local/bin<br>ls -all
    ### 相关命令
    >* redis-server 启动redis服务器
    >* redis-cli 启动redis命令行客户端
    >* redis-benchmark redis性能测试工具
    >* redis-check-aof AOF文件修复工具
    >* redis-check-rdb RDB文件检索工具
* ## step9:配置⽂件，移动到/etc/⽬录下
    * 配置⽂件⽬录为/usr/local/redis/redis.conf

* ## Mac 上安装 Redis:
    * 安装 Homebrew：
    >https://brew.sh/

    * 使用 brew 安装 Redis
    >https://www.cnblogs.com/cloudshadow/p/mac_brew_install_redis.html

# Reids配置