> Docker

```shell
# 导出镜像
docker save -o imageName

# 导入镜像
docker load < imageName

# 重启docker实例
docker restart name

# 本地文件导入到docker容器中
docker cp ./redis.conf redis:/etc # redis为容器名称 也可以为容器id

# 进入容器
docker exec -it redis /bin/bash # redis为容器名称

# 容器内安装 vim 等命令
apt-get update
apt-get install vim

# 安装mysql
docker run -d --name mysql8Server --restart=always -p7101:3306 -e MYSQL_ROOT_PASSWORD=nuoweidesenlin mysql

# 安装redis 并设置密码
docker run -d --name redis --restart=always -p7202:76379 redis --requirepass "nuoweidesenlin" --appendonly yes
# 输入密码并登录redis
docker exec -it 容器ID redis-cli -a "nuoweidesenlin"


```



