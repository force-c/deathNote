[TOC]

# linux 权限 

```sh
# 更改文件或者目录的属主
chown -R 用户名 文件/目录

```

# linux 用户

```shell
# 查看所有用户
cat /etc/passwd

# 新增用户
useradd username

# 修改密码
passwd password

# 删除用户
userdel -r username
```

# 常用命令

## find

```sh
# 使用-name参数查看/etc目录下面所有的.conf结尾的配置文件：
find /etc -name "*.conf"
# 使用-size参数查看/etc目录下面大于1M的文件：
find /etc -size +1M
# 列出当前目录及子目录下所有文件和文件夹：
find .
# 在/var/log目录下忽略大小写查找以.log结尾的文件名：
find /var/log -iname "*.log"
# 搜索超过七天内被访问过的所有文件：
find . -type f -atime +7
# 找出/home下不是以.txt结尾的文件：
find /home ! -name "*.txt"
```

## rename

```sh
# 将所有以jpg结尾的文件重命名为以png结尾的文件：
rename .jpg .png *.jpg
# 将main1.c重命名为main.c：
rename main1.c main.c main1.c
```

## tail

```sh
# 显示文件file的最后10行：
tail file
# 一直变化的文件总是显示后10行：
tail -f 10 file
# 显示文件file的内容，从第20行至文件末尾：
tail +20 file
# 显示文件file的最后10个字符：
tail -c 10 file
```

## cat

```sh
# 查看文件内容
cat nice.log
# 显示行数（空行也编号
cat -n nice.log
# 显示行数（多个空行算一个编号）
cat -s nice.log
# 显示行数（空行不编号）
cat -b nice.log
# 查看文件的内容，并添加行数编号后输出到另外一个文件中：
cat -n linuxcool.log > linuxprobe.log 
# 清空文件的内容：
cat /dev/null > /root/filename.txt
# 持续写入文件内容，碰到EOF符后结束并保存：
[root@linuxcool ~] cat > filename.txt <<EOF
> Hello, World 
> Linux!
> EOF


```

## tar

```sh
# 压缩文件
tar -zcvf nice.log.tar nice.log

```

## zcat

```sh
# 不解压缩文件的情况下，显示压缩包中文件的内容:
zcat nice.gz
# 查看多个压缩文件：
zcat nice.gz nice2.gz
# 查看普通文件的内容:
zcat -f nice.log
# 获取压缩文件的属性（压缩大小，未压缩大小，比率 -- 压缩率）：
zcat -l nice.gz
```

## mv

```sh
# 将文件file_1重命名为file_2:
mv file_1 file_2
# 将文件file移动到目录dir中 ：
mv file /dir
# 将目录dir1移动目录dir2中（前提是目录dir2已存在，若不存在则改名)：
mv /dir1 /dir2
# 将目录dir1下的文件移动到当前目录下
mv /dir1/* .
```

## cp

```sh
# 复制多个文件：
cp -r file1 file2 file3 dir
```

## echo

```sh
# 结合输出重定向符，将字符串信息导入文件中：
echo "It is a test" > linuxcool.log
# 输出带有换行符的内容：
echo -e "a\nb\nc"
```

## rm

```sh
# 递归删除目录及目录下所有文件：
rm -rf /data/log
# 删除当前目录下所有文件：
rm -rf *
# 清空系统中所有的文件（谨慎）：
rm -rf /*
# 直接删除，不会有任何提示：
rm -f test.txt.bz2
```

