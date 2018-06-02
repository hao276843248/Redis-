# Reids
## 目录
* [安装Redis](#安装Redis)
* [配置Reids](#配置Reids)
* [服务端和客户端命令](#服务端和客户端命令)
    * [服务器端](#服务器端)
    * [客户端](#客户端)
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

# 安装Redis

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

# 配置Reids

## 配置
* Redis的配置信息在/etc/redis/redis.conf下。
* 查看
    >sudo vi /etc/redis/redis.conf    
## 配置详情
* 绑定IP:如果需要远程访问，可将此⾏注释，或绑定⼀个真实ip
    >bind 127.0.0.1
* 端口，默认6379
    >port 6379
* 是否以守护进程运⾏
    * 如果以守护进程运⾏，则不会在命令⾏阻塞，类似于服务
    * 如果以⾮守护进程运⾏，则当前终端被阻塞
    * 设置为yes表示守护进程，设置为no表示⾮守护进程
    * 推荐设置为yes
    >daemonize yes
* 数据⽂件
    >dbfilename dump.rdb
* 数据文件存储路径
    >dir /var/lib/redis
* 日志文件
    >logfile "/var/log/redis/redis-server.log"

* 数据库个数，默认16个 0-15号
    >database 16
    #### 切换数据库
    >select (库号)
* 主从复制，类似于双机备份。
    >slaveof

# 服务端和客户端命令

## 服务器端
* 服务器端的命令 
    >`redis-server`
* help文档
    >`redis-server --help`
    ```
    python@ubuntu:~$ redis-server --help
    Usage: ./redis-server [/path/to/redis.conf] [options]
        ./redis-server - (read config from stdin)
        ./redis-server -v or --version
        ./redis-server -h or --help
        ./redis-server --test-memory <megabytes>

    Examples:
        ./redis-server (run the server with default conf)
        ./redis-server /etc/redis/6379.conf
        ./redis-server --port 7777
        ./redis-server --port 7777 --slaveof 127.0.0.1 8888
        ./redis-server /etc/myredis.conf --loglevel verbose

    Sentinel mode:
        ./redis-server /etc/sentinel.conf --sentinel
    ```
* 相关命令
    >`ps aux | grep redis` 查看redis服务器进程
    <br>`sudo kill -9 pid` 杀死redis服务器
    <br>`sudo redis-server /etc/redis/redis.conf` 指定加载的配置文件
## 客户端
* 客户端的命令 
    >`redis-cli`
* 可以使⽤help查看帮助⽂档
    >`redis-cli --help`
    ```
    python@ubuntu:~$ redis-cli --help
    redis-cli 3.0.6

    Usage: redis-cli [OPTIONS] [cmd [arg [arg ...]]]
    -h <hostname>      Server hostname (default: 127.0.0.1).
    -p <port>          Server port (default: 6379).
    -s <socket>        Server socket (overrides hostname and port).
    -a <password>      Password to use when connecting to the server.
    -r <repeat>        Execute specified command N times.
    -i <interval>      When -r is used, waits <interval> seconds per command.
                        It is possible to specify sub-second times like -i 0.1.
    -n <db>            Database number.
    -x                 Read last argument from STDIN.
    -d <delimiter>     Multi-bulk delimiter in for raw formatting (default: \n).
    -c                 Enable cluster mode (follow -ASK and -MOVED redirections).
    --raw              Use raw formatting for replies (default when STDOUT is
                        not a tty).
    --no-raw           Force formatted output even when STDOUT is not a tty.
    --csv              Output in CSV format.
    --stat             Print rolling stats about server: mem, clients, ...
    --latency          Enter a special mode continuously sampling latency.
    --latency-history  Like --latency but tracking latency changes over time.
                        Default time interval is 15 sec. Change it using -i.
    --latency-dist     Shows latency as a spectrum, requires xterm 256 colors.
                        Default time interval is 1 sec. Change it using -i.
    --lru-test <keys>  Simulate a cache workload with an 80-20 distribution.
    --slave            Simulate a slave showing commands received from the master.
    --rdb <filename>   Transfer an RDB dump from remote server to local file.
    --pipe             Transfer raw Redis protocol from stdin to server.
    --pipe-timeout <n> In --pipe mode, abort with error if after sending all data.
                        no reply is received within <n> seconds.
                        Default timeout: 30. Use 0 to wait forever.
    --bigkeys          Sample Redis keys looking for big keys.
    --scan             List all keys using the SCAN command.
    --pattern <pat>    Useful with --scan to specify a SCAN pattern.
    --intrinsic-latency <sec> Run a test to measure intrinsic system latency.
                        The test will run for the specified amount of seconds.
    --eval <file>      Send an EVAL command using the Lua script at <file>.
    --help             Output this help and exit.
    --version          Output version and exit.

    Examples:
    cat /etc/passwd | redis-cli -x set mypasswd
    redis-cli get mypasswd
    redis-cli -r 100 lpush mylist x
    redis-cli -r 100 -i 1 info | grep used_memory_human:
    redis-cli --eval myscript.lua key1 key2 , arg1 arg2 arg3
    redis-cli --scan --pattern '*:12345*'

    (Note: when using --eval the comma separates KEYS[] from ARGV[] items)

    When no command is given, redis-cli starts in interactive mode.
    Type "help" in interactive mode for information on available commands.

    ```
* 连接redis
    >`redis-cli`
    ```
    python@ubuntu:~$ redis-cli
    127.0.0.1:6379>
    ```
* 运⾏测试命令
    >`ping`
    ```
    127.0.0.1:6379> ping
    PONG    
    ```
* 切换数据库
    ```
    数据库没有名称，默认有16个，通过0-15来标识，连接redis默认选择第一个数据库
    ```
    > `select` 10
    ```
    127.0.0.1:6379> select 10
    OK
    127.0.0.1:6379[10]>
    ```
# 数据库操作
* 数据结构
    * redis是key-value的数据结构，每条数据都是⼀个键值对
    * 键的类型是字符串
    * 注意：键不能重复

    ![](\img\p1_67.png)
## 数据类型
* ### 字符串string
    * 字符串类型是 Redis 中最为基础的数据存储类型，它在 Redis 中是二进制安全的，这便意味着该类型可以接受任何格式的数据，如JPEG图像数据或Json对象描述信息等。在Redis中字符串类型的Value最多可以容纳的数据长度是512M。
    * ### 保存
* ### 哈希hash
* ### 列表list
* ### 集合set
* ### 有序集合zset
## 键命令











