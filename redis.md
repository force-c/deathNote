[TOC]

# Redis

## 下载地址

http://download.redis.io/releases/

## 数据结构

### bitmap

> 位图

```bash
# 最后一个数据只能为0 || 1
setbit key 0 0
setbit key 1 0
setbit key 2 1

getbit key 0
getbit key 1

# 统计key中数据为1的个数
bitcount key
```

### hyperloglog

> 统计基数
>
> 占用的内存是固定的 2^64 需要12k
>
> 有0.81%的错误率 允许计算结果有一定的容错率可以使用

```bash
# 添加元素
pfadd keyname1 元素1 元素2 元素3 元素4
pfadd keyname2 元素1 元素2 元素5

# 计算keyname1中元素的个数（去重）
pfcount keyname1

# 合并 keyname1 keyname2 为keyname3 并集
pfmerge keyname3 keyname1 keyname2

```

#### geospatial

> 地理位置



## 事务



## 设置密码

设置临时免密（重启失效）

```sh
# 设置密码
config set requirepass password
# 查看当前密码
config get requirepass
```



设置永久密码（重启依旧生效）

```sh
# 修改redis.conf
# requirepass foobared >  requirepass password
# 重启redis
```

## Docker安装Redis

```sh
# 安装redis 并设置密码 开启aof 持久化机制
docker run -d --name redis --restart=always -p7202:76379 redis --requirepass "nuoweidesenlin" --appendonly yes
# 输入密码并登录redis
docker exec -it 容器ID redis-cli -a "nuoweidesenlin"
# 注意使用docekr安装的redis没有 redis.conf 如需要可以从redis源码中copy redis.conf 到容器的/etc目录下
```

## 持久化

### RDB

​	

```sh
# 3600秒内，如果超过1个key被修改，则发起快照保存
save 3600 1
# 300秒内，如果超过100个key被修改，则发起快照保存
save 300 100
# 60秒内，如果1万个key被修改，则发起快照保存
save 60 10000


```



### AOF

```sh
# 开启持久化 默认为no
appendonly yes 

# 设置生成文件名称
appendfilename "appendonly.aof"

# 设置持久化频率 默认为 everysec
# always: 每次操作都会立即写入aof文件中
appendfsync always
# everysec: 每秒持久化一次(默认配置)
appendfsync everysec
# no: 不主动进行同步操作，默认30s一次
appendfsync no

# 默认不重写aof文件
no-appendfsync-on-rewrite no

```

## 搭建集群

### 哨兵模式

> 哨兵模式

1. 配置哨兵配置文件 sentinel.conf

   ```bash
   # mastername 代表 主机名称
   # ip port 主机地址端口
   # 最后数字1 代表 有1个哨兵认为reids server 主观下线 则该redis server 客观下线  
   # 只有1个哨兵就写1 若有3个哨兵则写2
   # 哨兵数量尽量是奇数
   sentinel monitor mastername ip port 1
   ```

   

2. 修改redis.conf

   ```sh
   daemonize yes
   protected-mode no
   # bind 127.0.0.1 -::1
   ```

   

3. 