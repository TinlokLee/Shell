###  SHELL 编程

###  一 	概述

shell 是Linux核心，是操作系统的最外层，合并编程语言，一些控制进程和文件，启动控制其它程序，用户与操作系统之间的命名解释器

实现日常自动化运维工作

```
操作环境                    vi  / ｖｉｍ　编辑器 
#！/bin/bash
#  注释文件
# by authors lee 2018
echo 'hello Python!'
```

####  1	常用命令

|                          |                    |      |
| ------------------------ | ------------------ | ---- |
| vim first_shell.sh       | #! /bin/bash       |      |
| echo 'Hello python'      | echo 输出shell命令 |      |
| chmod o+x first_shell.sh | 增加执行权限       |      |
| ./first_shell.sh         | 执行shell脚本      |      |
|                          |                    |      |

####  2 	变量详解

系统变量直接调用  $PWD

|      |                                                             |      |
| ---- | ----------------------------------------------------------- | ---- |
| $0   | 当前程序脚本名称                                            |      |
| $n   | 当前程序第n个参数 cat aa.sh lee(lee第一个参数)              |      |
| $#   | 当前程序参数个数（不包括程序本身）                          |      |
| $?   | 命令程序执行后状态（一般返回0或执行成功）  检测脚本是否正确 |      |
| $UID | 当前用户的ID                                                |      |
| $PWD | 当前所在们目录                                              |      |
| $*   | 当前程序所有参数（不包括程序本身）                          |      |
| $i   | 变量                                                        |      |
|      |                                                             |      |

```
$1  echo 'The \$1 is $1?'
$?  echo 'The \$? is $?'
$*  echo 'The \$* is $*'
$#  echo 'The \$# is $#'
echo -e '\033[32mp ...\033[1m'  添加颜色蓝色
：wq
sh aaa.sh abc 123  999
>>$1 is abc            第一个参数
>>$? is 0			   命令执行结果
>>$* is abc 123  999   所有参数
>>$# is 3			   参数个数
```

两种：局部变量和环境变量   A=111

```
#！/bin/bash
# define path var
A=123
echo"$A"
./xx.sh  执行
```

####  3	if 条件语句

```
#！/bin/bash
N1=100
N2=200
if (($N1 > $N2));then
	echo'this is $N1 greate $N2'
else
	echo'this is $N2 greate $N1'
fi
''' 错误无输出 '''
```

| 逻辑运算符       |                                    |      |
| ---------------- | ---------------------------------- | ---- |
| -f               | 判断文件是否存在  if []            |      |
| -d               | 判断目录是否存在                   |      |
| -eq              | 等于                               |      |
| -ne              | 不等于                             |      |
| -lt  -le -gt -ge | 小于。。                           |      |
| -a               | 逻辑表达式，and 双方都成立         |      |
| -o -z            | -or  -z 空字符串，输入输出是否为空 |      |

```
#! /bin/bash
DIR=/tmp/999/
if [ ！-d DIR ];then
		mkdir -p $DIR
		echo -e'\033[32mThis $DIR Create success!\033[0m'
else
		echo -e'\033[32mThis $DIR is exist,Please exit!033[0m'
fi
:wq
测试脚本 /bin/bash -n aaa.sh  无输出则正确
bin/bash aaa.sh    执行 ./aaa.sh
ll /tmp/999/   查看该目录是否存在
```

```
判断文件是否存在
#! /bin/bash
# authors lee 
FILES=/tmp/999.txt
if [ ! -f $FILES ];then
		echo 'ok' >> $FEILES   >覆盖原内容 >>追加内容
else
		echo -e '033[32m...\033[1m'
		cat $FIELS
fi
```

```
mysql -uroot- p
grant all on cacti.* to backup@'localhost' identified by '111111' 授权所有权限
----
每天运行
crontab -e 任务切换
0  0  * * * /bin/bash /data/shell/aaa.sh >>/tmp/mysql_bak.log  每天凌晨十二点执行脚本，输出到日志中
```

![shell MySQL备份脚本1](C:\Users\Administrator\Desktop\项目源码参考截图\shell MySQL备份脚本1.jpg)

![shell MySQL备份脚本2](C:\Users\Administrator\Desktop\项目源码参考截图\shell MySQL备份脚本2.jpg)

####  4 LAPM 脚本

一键源码安装 LAMP 脚本：打印菜单

```
自动安装apache  MySQL PHP 服务器
配置文档
查看服务器是否启动
netatst -tnl
/usr/local/apache/bin/apachectl -t
/usr/local/apache/bin/apachectl start  手动启动
ps -ef |grep http    查看

```

#### 5	for 语句

```
for i in `seq 1 15`  反引号会作为命名执行
do
		echo -e'...'
done
--------------------------------------
求和1,100
j=0
for ((i=1;i<=100;i++))
do
		j=`expr $i + $j`
done
echo $j
# sh -x aa.sh 查看shell脚本执行过程
--------------------------------------
找到log，批量打包
# tar czf all.tgz*
#!/bin/bash
for i in `find . -name '*.log'|tail -2`
do
		tar -czxf aaa$i.tgz $i
done
---
cp  复制到指定路径
cd
tar xzf aaa.tgz  -C bbb/ 解压到 bbb文件夹
---------------------------------------
远程批量传文件
ssh-keygen 免秘钥登陆
FILES=$*
if [ -z $* ];then
		echo -e '.....'
		exit
fi

for i in 'echo ip1 ip2'
do
		scp -r $FILES root@$i:/root/
done
---- 
ssh -l root 127.   'df -h'
或者
ssh-copy-id -i /root/.ssh/id_rsa.pub 127...
```

####  6 while 语句

```
i=0
while [[ $i -lt 10 ]]
# while (( $i < 10 ))
do
		echo "..."
		((i++))
done
-------------------------
read -p "..."
read $a
-------------------------
while read line
do
		echo $line
done
-------------------------
多服务器读取列表任务
while read line
do
	echo -e " \033[32mtxt root@$line:/tmp \033[0m"
done <list.txt

```

case 语句

```
case $1 in
	apache )
	echo ''
	;;
	...
esac

```

select 语句

```

```



###  二	进阶

####  1  shell 数组和函数编程

```
function foo()
command
-------------------------------------------------
安装nginx
function　ngnix_install()
{
    wget -c ${url}/$(FILES)
    tar xzf 
}
select i in "nginx" "php"
do
echo $i
   选择菜单
case $1 in
	nginx )
	nginx_install;;
	*    )
	echo "usage: .."
	;;
esac
done	
# ngnix_install && php_install
```

####  2  awk sed grep find命令

gg  修改位置最前  G 最后

sed  '/aa/aaaa'   预编辑  加参数 -i 修改

sed  -i 's/192.168.../g'  test.txt  直接修改原文件

sed 's/^/& /g' tset.txt        文章行首加 空格

sed '/sss/a -----'  test.txt  结尾添加修改

sed '/sss/i -----'  test     之前添加

sed -n '1,5p' test.txt        打印1,5行内容

```
找出最大值,最小值
cat number.txt |sed 's/ /\n/g'|grep -v '^$'|sort -nr|sed -n  '1p;$p' 空格替换为换行,排除以空格开始结尾，排序后输出最大最小值
-v 'a' 匹配排除a
```

cat  /etc/password|awk -F: '{print $1}'

打印用户名，以：分割的第一个值

```
找出 ip
grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}"
egrep -E "|"   多级匹配
wenjian|grep 'aaa' |awk '{print $1}'|sed 's/addr:/ /g'
--------------------------------------------
df -h|grep '/$'|awk "{print $5}"|sed 's/%//g'
查看磁盘根目录，输出第五个数，并去掉%
---------------------------------------------
find . -maxdepth 1 -type f -name "*.txt" -mtime -2 -exec rm -rf {} \;
找当前目录，一级目录下，找出2天内，所有名为.txt 的文件并删除
find . -mtime +30 -exec rm -rf {} \; 删除30天之前的，exec/xargs 层结
find . maxdepth 1 -size +100M -type f 找到大于100M的文件  
-exec mv {} /tmp/ \;移到tmp文件下
```

awk  find  grep sed  四剑客，自动化运维常用

xagrs 和 exec  用法区别，后者可 cp 拷贝，mv 移动

####  3  完整备份和增量备份

```
tar   sync   cp  python方法
每周日做完整备份，平时做增量备份

shell脚本备份
tar -g /tmp/aaa -czvf /tmp/bbb.tar.gz /data/sh/  全备份
tar -g /tmp/snapshot -czvf /tmp/ccc.tar.gz data/sh/ 增量备份 snapshot 快照
-----------------
自动时间备份
echo `data +%d   %u`日 星期


```

####    4  自动禁止恶意IP访问

```
安全：机房的物理安全，防护墙，封禁恶意IP
ssh - l root 192.168.99.101 远程链接新服务器
yum install openssh-clIents 安装客户端包
ssh
ssh - l root 192.168.99.101  登陆
输入密码
tail -fn 100 /var/log/secure  查看访问错误日志
tail -n 100 /var/log/secure |grep "Failed passwd" |awk '{print $ 11}'|sort |uniq -c |sort nr |awk '{print $1}' 取出错误日志中的 ip,统计出现次数，取出该ip  
vi /etc/sysconfig/iptables           编辑  ：x 退出
- A INPUT -s 192.168.99.999 -j DROP  加入到防火墙中，禁止掉

脚本实现，批量封禁
黑名单机制，加入到黑名单，然后drop，具体脚本，百度

```

####   5 网站自动化部署管理

```
拷贝批量文件到多台服务器，同步目录 rsync

if  [ ! -f ip.txt ];then
		echo " "
cat <<EOF
EOF
		exit
fi
if
		[ -z "$1"];then
		echo "  "
		exit
fi		
count=`cat ip.txt |wc -l`
rm -rf ip.txt.swp
while  ((i< $count))
do
i=`expr $i + 1`
sed"${i}s/^/&${i} /g" ip.txt >>ip.txt.swp
打印第一行，并追加到新的文件
IP=`awk -v I="$i" '{if(I=$1)print $2}' ip.txt.swp`
scp -r $1 root@${IP}:$2
done

----------------------------------------------------------
远程执行命令脚本
企业中应用非常多
```

####   6  批量监控服务及报警

```
开源如 nginx 实时监控
shell 脚本实现监控
批量或多个单个检查服务是够启动，如没有，则发送邮件告警通知
1  需要for循环或者参数输入
2  系统服务有哪些，什么状态表示启动
3  没启动的状态是什么
4  发送邮件，邮件格式
apache 查看是否启动： /etc/init.d/httpd status
标准查看： ps -ef |grep httpd(nginx)



```

####  7  服务器硬件信息收集

```
1  创建数据库表
2  导出EXCEL格式
```

####   8   磁盘监控报警服务

```
df -h 
while read line 
分布式监控系统

```



###	三	总结

####   1  shell WEB界面展示

```
HTML 超文本标记语言 <html> <head> <body>
<a target="blank">  blank 跳转
<h1> <tr><td> <table> CSS样式表美化页面表格等

```

运维自动化，Python 自动化实现

wc -l   查看数量

clear   清屏

cat -n a.txt   查看文件

%s/^ //g  以空格开始换成空

```
id和URL列表
把所有的图片追加进去
for i in `find . -name "*.jpg" -or -name "*.jpg" -or -name "*.png"|sed 's/^.\///g'`;do echo http://192.199.22.213/cacti/$i;done >>/tmp/a.txt

HTML='/var/www/html/index.html'
echo -e "033[32mProcess Start,Please wait.../033[1m"
echo "---------------------------------------------"
sleep 2

cat >$HTML <<EOF
<html>
<body>
<table broder="1" width="100" cellpadding="0" cellspacing="0">
<tr align="center"><td>id号</td><td>URL列表</td></tr>
EOF
while read line
do
		ID=`echo $line|awk '{print $1}'`
		URL=`echo $line|awk '{print $2}'`
		echo $ID $URL
		sleep 1
		echo "<tr align="center"><td>$ID</td><td><a target="biank" href=$URL>$URL</a></td></tr>" >>$HTML
done </tmp/list.txt
cat >>$HTML <<EOF

保存，退出 ：wq
sh auto_html.sh  执行shell脚本命令
```

###  四  编程

参考：http://www.cse.unsw.edu.au/~cs2041/12s2/lec/shell/examples.notes.html

1   用 shell 编程，判断一个文件是不是字符设备文件，如果是拷贝到/dev目录下

```
#!/bin/bash
# by authors lee

FILENAME=echo "Input file name: "
read FILENAME
if [ - c "$FILENAME" ];then
cp $FILENAME /dev
fi
```

2   添加一个用户组名为class1，然后为该组添加30个用户，用户名形式为stdxxx,其中xx从1到30

```
#！/bin/bash
i=1
groupadd class1
while [ $i -le 30 ]
do
if [ $i le 9 ];then
UESRNAME=stu0${i}
else
USERNAME=stu${i}
fi
useradd $USERNAME
mkdir /home/$USERNAME
chown -R $USERNAME /home/$USERNAME
chgrp -r class1 /home/$USERNAME
i=$(($i+1))
done

```

3   自动删除50个账号，账号名为stud1-stud50

```
#!/bin/bash

i=1
while [ $i -le 50 ]
do
userdel -r stud${i}
i=$((i+1))
done
```

4   

```
#!/bin/sh

DIRNAME=`ls /root |grep bak`
if [ -z "$DIRNAME" ];then
mkdir /root/bak
cd /root/bak
fi
YY=`date +%y`
MM=`dtae +%m`
DD=`date +%d`
BACKETC=$YYMMDD_etc.tar.gz
tar zcvf $BACKETC /etc
echo "fileback finished!"
```

5   某系统管理员需每天做一定的重复工作，请按照下列要求，编制一个解决方案：

（1）在下午4 :50删除/abc目录下的全部子目录和全部文件；

（2）从早8:00～下午6:00每小时读取/xyz目录下x1文件中每行第一个域全部数据加入到/backup目录下bak01.txt文件内；

（3）每逢星期一下午5:50将/data目录下的所有目录和文件归档并压缩为文件：backup.tar.gz；

（4）在下午5:55将IDE接口的CD-ROM卸载（假设：CD-ROM的设备名为hdc）；

（5）在早晨8:00前开机后启动

```
vi 创建一个prgx文件，内容如下

50 16 * * * rm -r /abc/*
0 8 -18/1 * * * cut -f1 /xyz/x1 >;>; /backup/bako1.txt
50 17 * * * tar zcvf backup.tar.gz /data
55 5 * * * umount /dev/hdc
有超级用户登陆，用crontab执行 prgx 文件中的内容
root@xxx:#crontab prgx
```

6   在每月第一天备份并压缩/etc目录所有内容，存放在/root/bak目录，且文件名形式如下yymmdd_etc,shell程序fileback存放在/usr/bin目录下

```
编写定时任务

echo "0 0 1 * * /bin/sh /usr/bin/fileback" >; /root/etcbackcron
crontab /root/etcbakcron
或使用crontab -e 命名添加定时任务
0 1 * * * /bin/sh /usr/bin/fileback
```

7   有一普通用户想在每周日凌晨零分定期备份/usr/backup到/tmp目录下，实现方法

```
使用crontab -e 命令创建crontab文件，格式如下
0 0 * * sun cp -r /usr/backup /tmp

方法二： 用户在自己的目录下新建文件夹file,内容格式如下
0 * * sun cp -r /usr/backup /tmp
然后执行 corntab file 使生效
```

8   在/usrdata 目录下建立50个目录，即user1~user50，并设置用户权限，其中其它用户读；文件所有者：读，写，执行，文件所有者所在权限，读，执行

```
#！/bin/sh
i=1
while [ i -le 50 ]
do 
if [ -d /usrdata ];then
mkdir -p /usrdata/user$i
chmod 754 /usrdata/usrdata$i
echo "user$i"
let "i = i + 1"   # i=$(($i+1))
else
mkdir /usrdata
mkdir -p /usrdata/user$i
chmod 754 /usrdata/user$i
echo "uesr$i"
i=$(($i+1))
fi
done
```

9    MySQL备份实例，自动备份MySQL，并删除30天前的备份文件

```
#!/bin/sh 
#auto backup mysql 
#lee
#PATH DEFINE 
BAKDIR=/data/backup/mysql/`date +%Y-%m-%d` 
MYSQLDB=www 
MYSQLPW=backup 
MYSQLUSR=backup 
if[ $UID -ne 0 ];then 
echo "This script must use administrator or root user ,please exit! "
sleep 2 
exit 0 
fi 
if[ ! -d $BAKDIR ];then 
mkdir -p $BAKDIR 
else 
echo "This is $BAKDIR exists ,please exit …. "
sleep 2 
exit 
fi 
###mysqldump backup mysql 
/usr/bin/mysqldump -u$MYSQLUSR -p$MYSQLPW -d $MYSQLDB >/data/backup/mysql/`date +%Y-%m-%d`/www_db.sql 
cd $BAKDIR ; tar -czf www_mysql_db.tar.gz *.sql 
cd $BAKDIR ;find . -name “*.sql” |xargs rm -rf[ $? -eq 0 ]&&echo “This `date +%Y-%m-%d` RESIN BACKUP is SUCCESS” 
cd /data/backup/mysql/ ;find . -mtime +30 |xargs rm -rf
```

10    自动安装Nginx脚本，采用case方式，选择方式，也可以根据实际需求改成自己想要的脚本

```
#!/bin/sh 
###nginx install shell 
###wugk 2012-07-14 
###PATH DEFINE 
SOFT_PATH=/data/soft/ 
NGINX_FILE=nginx-1.2.0.tar.gz 
DOWN_PATH=http://nginx.org/download/ 
if[ $UID -ne 0 ];then 
echo This script must use administrator or root user ,please exit! 
sleep 2 
exit 0 
fi 
if[ ! -d $SOFT_PATH ];then 
mkdir -p $SOFT_PATH 
fi 
download () 
{ 
cd $SOFT_PATH ;wget $DOWN_PATH/$NGINX_FILE 
} 
install () 
{ 
yum install pcre-devel -y 
cd $SOFT_PATH ;tar xzf $NGINX_FILE ;cd nginx-1.2.0/ &&./configure –prefix=/usr/local/nginx/ –with-http_stub_status_module –with-http_ssl_module 
[ $? -eq 0 ]&&make &&make install 
} 
start () 
{ 
lsof -i :80[ $? -ne 0 ]&&/usr/local/nginx/sbin/nginx 
} 
stop () 
{ 
ps -ef |grep nginx |grep -v grep |awk ‘{print $2}’|xargs kill -9 
} 
exit () 
{ 
echo $? ;exit 
} 
###case menu ##### 
case $1 in 
download ) 
download 
;; 
install ) 
install 
;; 
start ) 
start 
;; 
stop ) 
stop 
;; 
* ) 
echo “USAGE:$0 {download or install or start or stop}” 
exit 
esac
```

11   批量解压tar脚本，批量解压zip并且建立当前目录

```
#！/bin/sh
PATH1=/tmp/images
PATH2=/usr/www/images
for i in `ls ${PATH1}/*`
do
tar xvf $i -C $PATH2
done
------------------------------------------------------------
这个脚本是所有文件在一个目录子下，实际情况可能在下一级目录或更深目录下，我们可以通过find查找
#！/bin/sh
PATH1=/tmp/images
PATH2=/usr/www/images
for i in `find $PATH1 -name"*.tar"`
do
tar xvf $i -C $PATH2
done
--------------------------------------------------------------
如果是zip文件，例如123189.zip 132342.zip 等等批量文件，默认unzip直接解压不带自身目录，意思是解压123189.zip完当前目录就是图片，不能创建123189目录下并解压，可以用shell脚本实现

#！/bin/sh
PATH1=/tmp/images
PATH2=/usr/www/images
cd $PATH1
for i in `find . -name"*.zip"|awk -F . {print $2} `
do 
mkdir -p PATH2$i
unzip -o .$i.zip -d PATH2$i
done
```

ssh  批量上传文件

1   将上传文件列表放在一个test文件中

```
root@ubuntu:/home/zhangy# cat test 
/home/zhangy/test/aaa 
/home/zhangy/test/nginx.conf 
/home/zhangy/test/test.sql 
/home/zhangy/test/pa.txt 
/home/zhangy/test/password
```

2   批量上传脚本

vim file_upload.sh

```
#!/bin/sh 
DATE=`date +%Y_%m_%d_%H` 
if [ $1 ] 
then 
for file in $(sed '/^$/d' $1) //去掉空行 
do 
if [ -f $file ] //普通文件 
then 
res=`scp $file $2:$file` //上传文件 
if [ -z $res ] //上传成功 
then 
echo $file >> ${DATE}_upload.log //上传成功的日志 
fi 
elif [ -d $file ] //目录 
then 
res=`scp -r $file $2:$file` 
if [ -z $res ] 
then 
echo $file >> ${DATE}_upload.log 
fi 
fi 
done 
else 
echo "no file" >> ${DATE}_error.log 
fi
```

上传成功返回一个空行，不成功什么也不返回

3   上传格式

```
./file_upload.sh test 192.168.1.111
```

test 是上传列表文件，ip 是上传地址

shell 脚本实现，转换文件大小写

```
#!/bin/sh
for i in `ls`
do
if [ ! -f $x ];then
continue
fi
lc=`echo $x | tr '[A-Z]' '[a-z]'`
mv -i $x $lc
fi
done看网站 Watch a Website

```

看网站 Watch a Website

```
% watch_website.sh http://ticketek.com.au/ 'Ke[sS$]+ha' andrewt@cse.unsw.edu.au 
repeat_seconds=300 #check every 5 minutes 
if test $# = 3 
then 
 url=$1 
 regexp=$2 
 email_address=$3 
else 
 echo "Usage: $0 <url> <regex>" 1>&2 
 exit 1 
fi 
while true 
do 
 if wget -O- -q "$url"|egrep "$regexp" >/dev/null 
 then 
 echo "Generated by $0" | mail -s "$url now matches $regexp" $email_address 
 exit 0 
 fi 
 sleep $repeat_seconds 
done
```

转GIF到PNG convert GIF files to PNG

```
if [ $# -eq 0 ] 
then 
 echo "Usage: $0 files..." 1>&2 
 exit 1 
fi 
if ! type giftopnm 2>/dev/null 
then 
 echo "$0: conversion tool giftopnm not found " 1>&2 
 exit 1 
fi 
# missing "in ..." defaults to in "$@" 
for f 
do 
 case "$f" in 
 *.gif) 
 # OK, do nothing 
 ;; 
 *) 
 echo "gif2png: skipping $f, not GIF" 
 continue 
 ;; 
 esac 
 dir=`dirname "$f"` 
 base=`basename "$f" .gif` 
 result="$dir/$base.png" 
 giftopnm "$f" | pnmtopng > $result && echo "wrote $result" 
done
```

计数 Counting

```
if test $# = 1 
then 
 start=1 
 finish=$1 
elif test $# = 2 
then 
 start=$1 
 finish=$2 
else 
 echo "Usage: $0 <start> <finish>" 1>&2 
 exit 1 
fi 
for argument in "$@" 
do 
 if echo "$argument"|egrep -v '^-?[0-9]+$' >/dev/null 
 then 
 echo "$0: argument '$argument' is not an integer" 1>&2 
 exit 1 
 fi 
done 
number=$start 
while test $number -le $finish 
do 
 echo $number 
 number=`expr $number + 1` # or number=$(($number + 1)) 
done

```

5. 字频率 Word Frequency

```
sed 's/ /
/g' "$@"| # convert to one word per line 
tr A-Z a-z| # map uppercase to lower case 
sed "s/[^a-z']//g"| # remove all characters except a-z and ' 
egrep -v '^$'| # remove empty lines 
sort| # place words in alphabetical order 
uniq -c| # use uniq to count how many times each word occurs 
sort -n # order words in frequency of occurrance
```

