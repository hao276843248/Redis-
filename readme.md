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
* [集群](#集群)

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

    ![](.\img\p1_67.png)
## 数据类型
* ## 字符串string
    * 字符串类型是 Redis 中最为基础的数据存储类型，它在 Redis 中是二进制安全的，这便意味着该类型可以接受任何格式的数据，如JPEG图像数据或Json对象描述信息等。在Redis中字符串类型的Value最多可以容纳的数据长度是512M。
    * ## 保存
        * 设置键值
            >`set key value`
        * 例1：设置键为name值为test的数据
            ```
            127.0.0.1:6379> set name test
            OK 
            ```
        * 设置键值及过期时间，秒为单位
            >`setex key seconds value`
        * 例2：设置键为aa值为aa过期时间为3秒的数据
            ```
            127.0.0.1:6379> setex aa 3 aa
            OK
            ```
        * 设置多个键值
            >`mset key1 value1 key2 value2 ...`
        * 例3：设置键为'a1'值为'python'、键为'a2'值为'java'、键为'a3'值为'c'
            ```
            127.0.0.1:6379> mset a1 python a2 java a3 c
            OK
            127.0.0.1:6379> keys *
            1) "name"
            2) "a3"
            3) "a1"
            4) "a2"
            ```
        * 追加值
            >`append key value`
        * 例4：向键为a1中追加值'haha'
            ```
            127.0.0.1:6379> append 'a1' 'haha'
            (integer) 10
            127.0.0.1:6379> get a1
            "pythonhaha"
            "
            ```
    * ## 获取
        * 获取：根据键获取值，如果不存在此键则返回nil
           >`get key`
           ```
           127.0.0.1:6379> get a1
            "pythonhaha"
           ```
        * 根据多个键获取多个值
            >`mget key1 key2 ...`
            ```
            127.0.0.1:6379> mget a1 a2 a3
            1) "pythonhaha"
            2) "java"
            3) "c"
            ```
    * ## 删除
        * 删除键及对应的值
            >`del key1 key2 ...`
        *  例5：删除键a2、a3
            ```
            127.0.0.1:6379> del a1
            (integer) 1
            ```


* ## 哈希hash
    * ## 类型
        * hash⽤于存储对象，对象的结构为属性、值
        * 值的类型为string
    * ## 增加、修改
        * 设置单个属性
            > `hset key field value`
        * 例1：设置键 user的属性name为itheima
            ```
            127.0.0.1:6379> hset user name itheima
            (integer) 1
            ```
        * 可能存在问题
            ```
            MISCONF Redis is configured to save RDB snapshots, but is currently not able to persist on disk. Commands that may modify the data set are disabled. Please check Redis logs for details about the error.

            Redis被配置为保存数据库快照，但它目前不能持久化到硬盘。用来修改集合数据的命令不能用
            ```
            * 原因 
                >强制关闭Redis快照导致不能持久化。
            * 解决方案
                >运行config set stop-writes-on-bgsave-error no　命令后，关闭配置项stop-writes-on-bgsave-error解决该问题。
        * 设置多个属性
            >`hmset key field1 value1 field2 value2 ...`
        * 例2：设置键u2的属性name为itcast、属性age为11
            ``` 
            127.0.0.1:6379> hmset u2 name itcats age 11
            OK
            127.0.0.1:6379> hkeys u2
            1) "name"
            2) "age"
            ```
    * ## 获取
        * 获取指定键所有的属性
            >`hkeys key`
        * 获取⼀个属性的值
            >`hget key field`
        * 获取多个属性的值
            >`hmget key field1 field2...`
            ```
            127.0.0.1:6379> hkeys u2
            1) "name"
            2) "age"
            ```
        * 获取所有属性的值
            >`hvals key`
            ```
            127.0.0.1:6379> hvals u2
            1) "itcats"
            2) "11"
            ```
    * ## 删除
        * 删除整个hash键及值，使⽤del命令
        * 删除属性，属性对应的值会被⼀起删除
            >`hdel key field1 field2 ...`

* ## 列表list
    * ## list类型
        * 列表的元素类型为string
        * 按照插⼊顺序排序
    * ## 增加
        * 在左侧插⼊数据
            >`lpush key value1 value2 ...`
        * 例1：从键为'a1'的列表左侧加⼊数据a 、 b 、c
            ```
            lpush a1 a b c
            ```
        * 在右侧插⼊数据
            >`rpush key value1 value2 ...`
        * 例2：从键为'a1'的列表右侧加⼊数据0 1
            ```rpush a1 0 1 ```   
        * 在指定元素的前或后插⼊新元素
            >`linsert key before或after 现有元素 新元素`
        * 例3：在键为'a1'的列表中元素'b'前加⼊'3'
            ```   
            127.0.0.1:6379> linsert a1 before b 3
            (integer) 4
            127.0.0.1:6379> lrange a1 0 -1
            1) "c"
            2) "3"
            3) "b"
            4) "a"
            ```
    * ## 获取
        * 返回列表⾥指定范围内的元素
            *start、stop为元素的下标索引
            * 索引从左侧开始，第⼀个元素为0
            * 索引可以是负数，表示从尾部开始计数，如-1表示最后⼀个元素
            >`lrange key start stop`
        * 例4：获取键为'a1'的列表所有元素
            ```
            127.0.0.1:6379> lrange a1 0 -1
            1) "c"
            2) "3"
            3) "b"
            4) "a"
            ```
    * ## 设置指定索引位置的元素值
        * 索引从左侧开始，第⼀个元素为0
        * 索引可以是负数，表示尾部开始计数，如-1表示最后⼀个元素
        >`lset key index value`
        * 例5：修改键为'a1'的列表中下标为1的元素值为'z'
            ```
            lset a 1 z
            ```
    * ## 删除
        * 删除指定元素
            * 将列表中前count次出现的值为value的元素移除
            * count > 0: 从头往尾移除
            * count < 0: 从尾往头移除
            * count = 0: 移除所有
            >`lrem key count value`
        * 例6.1：向列表'a2'中加⼊元素'a'、'b'、'a'、'b'、'a'、'b'
            ```
            lpush a2 a b a b a b
            ```
        * 例6.2：从'a2'列表右侧开始删除2个'b'
            ```
            lrem a2 -2 b
            ```
        * 例6.3：查看列表'a2'的所有元素
            ```
            lrange a2 0 -1
            ```
* ## 集合set
    * ## set类型
        * ⽆序集合
        * 元素为string类型
        * 元素具有唯⼀性，不重复
        * 说明：对于集合没有修改操作
    * ## 增加
        * 添加元素
            >`sadd key member1 member2 ...`
     * ## 获取
       * 返回所有的元素
            >`smembers key`
     * ## 删除
       * 删除指定元素
            >`srem key`
* ## 有序集合zset
    * ## set类型
        * sorted set，有序集合
        * 元素为string类型
        * 元素具有唯⼀性，不重复
        * 每个元素都会关联⼀个double类型的score，表示权重，通过权重将元素从⼩到⼤排序
        * 说明：对于集合没有修改操作
    * ## 增加
        * 添加元素
            >`zadd key score1 member1 score2 member2 ...`
        * 例1：向键'a4'的集合中添加元素'lisi'、'wangwu'、'zhaoliu'、'zhangsan'，权重分别为4、5、6、3
            ```
            zadd a4 4 lisi 5 wangwu 6 zhaoliu 3 zhangsan
            ```

     * ## 获取
        * 返回指定范围内的元素
        * start、stop为元素的下标索引
        * 索引从左侧开始，第⼀个元素为0
        * 索引可以是负数，表示从尾部开始计数，如-1表示最后⼀个元素
        * 返回指定范围内的元素
            >`zrange key start stop`
        * 例2：获取键'a4'的集合中所有元素
            ```
            zrange a4 0 -1
            ```
        * 返回score值在min和max之间的成员
            ```
            zrangebyscore key min max
            ```
        * 返回成员member的score值
            ```
            zscore key member
            ```
        *例4：获取键'a4'的集合中元素'zhangsan'的权重
            ```
            zscore a4 zhangsan
            ```
     * ## 删除
        * 删除指定元素
            >`zrem key member1 member2 ...`
        * 例5：删除集合'a4'中元素'zhangsan'
            ```
            zrem a4 zhangsan
            ```
        * 删除权重在指定范围的元素
            >`zremrangebyscore key min max`
        * 例6：删除集合'a4'中权限在5、6之间的元素
            ```
            zremrangebyscore a4 5 6
            ```
## 键命令
* ## 查找键，参数⽀持正则表达式
    >`keys pattern`
* ## 判断键是否存在，如果存在返回1，不存在返回0
    >`exists key1`
* ## 查看键对应的value的类型
    >`type key`
* ## 删除键及对应的值
    >`del key1 key2 ...`
* ## 设置过期时间，以秒为单位
* ## 如果没有指定过期时间则⼀直存在，直到使⽤DEL移除
    >`expire key seconds`
* ## 查看有效时间，以秒为单位
    >`ttl key`
# 与python交互
## 安装包
* ## 安装方式
    * 第一种：进⼊虚拟环境py_django，联⽹安装包redis
    >  pip install redis
    * 第二种：进⼊虚拟环境py_django，联⽹安装包redis
    >  easy_install redis
    * 第三种：到中⽂官⽹-客户端下载redis包的源码，使⽤源码安装
    >  一步步执行 wget https://github.com/andymccurdy/redis-py/archive/master.zip<br>
    unzip master.zip<br>
    cd redis-py-master<br>
    sudo python setup.py install<br>
* ## 调⽤模块
    * 引⼊模块
        ```py
        from redis import *
        ```
        
    * 这个模块中提供了StrictRedis对象(Strict严格)，⽤于连接redis服务器，并按照不同类型提供 了不同⽅法，进⾏交互操作
    * StrictRedis对象⽅法
        * 通过init创建对象，指定参数host、port与指定的服务器和端⼝连接，host默认为localhost，port默认为6379，db默认为0
        ```py
        sr = StrictRedis(host='localhost', port=6379, db=0)
        # 简写
        sr=StrictRedis()
        ```
        * 根据不同的类型，拥有不同的实例⽅法可以调⽤，与前⾯学的redis命令对应，⽅法需要的参数与命令的参数⼀致
        * ## string
            * set
            * setex
            * mset
            * append
            * get
            * mget
            * key
        * ## keys
            * exists
            * type
            * delete
            * expire
            * getrange
            * ttl
        * ## hash
            * hset
            * hmset
            * hkeys
            * hget
            * hmget
            * hvals
            * hdel
        * ## list
            * lpush
            * rpush
            * linsert
            * lrange
            * lset
            * lrem
        * ## set
            * sadd
            * smembers
            * srem
        * ## zset
            * zadd
            * zrange
            * zrangebyscore
            * zscore
            * zrem
            * zremrangebyscore
* ## 示例
    * ## 准备
    ```py
    from redis import *
    if __name__=="__main__":
        try:
            #创建StrictRedis对象，与redis服务器建⽴连接
            sr=StrictRedis()
        except Exception as e:
            print(e)
    ```
    * ## string-增加
    ```py
    from redis import *
    if __name__=="__main__":
    try:
        #创建StrictRedis对象，与redis服务器建⽴连接
        sr=StrictRedis()
        #添加键name，值为itheima
        result=sr.set('name','itheima')
        #输出响应结果，如果添加成功则返回True，否则返回False
        print(result)
    except Exception as e:
        print(e)
    ```
    * ## string-获取
    ```py
    from redis import *
    if __name__=="__main__":
    try:
        #创建StrictRedis对象，与redis服务器建⽴连接
        sr=StrictRedis()
        #获取键name的值
        result = sr.get('name')
        #输出键的值，如果键不存在则返回None
        print(result)
    except Exception as e:
        print(e)
    ```
    * ## string-修改
    ```py
    from redis import *
    if __name__=="__main__":
    try:
        #创建StrictRedis对象，与redis服务器建⽴连接
        sr=StrictRedis()
        #获取键name的值
        result = sr.get('name')
        #输出键的值，如果键不存在则返回None
        print(result)
    except Exception as e:
        print(e)
    ```
    * ## string-删除
    ```py
    from redis import *
    if __name__=="__main__":
    try:
        #创建StrictRedis对象，与redis服务器建⽴连接
        sr=StrictRedis()
        #设置键name的值，如果键已经存在则进⾏修改，如果键不存在则进⾏添加
        result = sr.delete('name')
        #输出响应结果，如果删除成功则返回受影响的键数，否则则返回0
        print(result)
    except Exception as e:
        print(e)
    ```
    * ## 获取键
    ```py
    from redis import *
    if __name__=="__main__":
    try:
        #创建StrictRedis对象，与redis服务器建⽴连接
        sr=StrictRedis()
        #获取所有的键
        result=sr.keys()
        #输出响应结果，所有的键构成⼀个列表，如果没有键则返回空列表
        print(result)
    except Exception as e:
        print(e)
    ```
# 搭建主从
## 主从概念
* ⼀个master可以拥有多个slave，⼀个slave⼜可以拥有多个slave，如此下去，形成了强⼤的多级服务器集群架构
* master用来写数据，slave用来读数据，经统计：网站的读写比率是10:1
* 通过主从配置可以实现读写分离
* master和slave都是一个redis实例(redis服务)
## 主从配置
* ## 配置主
    * 查看当前主机的ip地址
    >ifconfig
    * 修改etc/redis/redis.conf文件
    >sudo vi redis.conf
    <br>bind 192.168.26.128
    * 重启redis服务
    >sudo service redis stop<br>redis-server redis.conf
* ## 配置从
    * 复制etc/redis/redis.conf文件
    >sudo cp redis.conf ./slave.conf
    * 修改redis/slave.conf文件
    >sudo vi slave.conf
    * 编辑内容
    >bind 192.168.26.128<br>
    slaveof 192.168.26.128 6379<br>
    port 6378
    * redis服务
    >sudo redis-server slave.conf
    * 查看主从关系
    >redis-cli -h 192.168.26.128 info Replication
* ## 数据操作
    * 在master和slave分别执⾏info命令，查看输出信息 进入主客户端
    >redis-cli -h 192.168.26.128 -p 6379
    * 进入从的客户端
    >redis-cli -h 192.168.26.128 -p 6378
    * 在master上写数据
    >set aa aa
    * 在slave上读数据
    > get aa

# 集群
* ## 集群分类
    * 软件层面
        * 只有一台电脑，在这一台电脑上启动了多个redis服务。
    * 硬件层面
        * 存在多台实体的电脑，每台电脑上都启动了一个redis或者多个redis服务。
* ## Python交互
* 安装包如下
    >pip install redis-py-cluster
* redis-py-cluster源码地址https://github.com/Grokzen/redis-py-cluster
* 创建⽂件redis_cluster.py，示例码如下
    ```py
    from rediscluster import *
    if __name__ == '__main__':
    try:
        # 构建所有的节点，Redis会使⽤CRC16算法，将键和值写到某个节点上
        startup_nodes = [
            {'host': '192.168.26.128', 'port': '7000'},
            {'host': '192.168.26.130', 'port': '7003'},
            {'host': '192.168.26.128', 'port': '7001'},
        ]
        # 构建StrictRedisCluster对象
        src=StrictRedisCluster(startup_nodes=startup_nodes,decode_responses=True)
        # 设置键为name、值为itheima的数据
        result=src.set('name','itheima')
        print(result)
        # 获取键为name
        name = src.get('name')
        print(name)
    except Exception as e:
        print(e)
        ```







