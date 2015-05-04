# Linux 命令 #

## man xxx ##
- 空格向下翻页
- 上下方向键滚动
- /time 搜索
- n向下查找
- N向上查找
- q退出

## man -f xxx ##
- 按种类查看
- man -f ls
- man 1 ls

## 用户分类 ##
- 普通用户	UID > 500
- 根用户		UID = 0
- 系统用户	UID 1 ~ 499

## id ##
- 显示自己的UID和GID

## groups ##
- 显示自己的组

## who ##
- 查看当前正在登录的用户

## cat /etc/passwd ##
- 存储系统的用户名
- 最后两列是家目录和shell目录

## useradd xx ##
- 创建家目录 /home/xxx
- 复制 /etc/skel 下文件到 /home/xxx
- 创建一个与xx一样的用户组
- -d path 指定家目录

## passwd xx ##
- 创建用户后必须设置密码，才允许登录
- passwd 不加参数会修改当前用户密码
- 非 root 用户不能修改别人密码

## cat /etc/shadow ##
- 存储密码

## usermod ##
- usermod -d xxx 修改家目录
- usermod -L xxx 冻结账号
- usermod -U xxx 解冻账号

## userdel xx ##
- 默认不会删除家目录
- userdel -r xxx 同时删除家目录

## groupadd xx ##
- 添加用户组

## cat /etc/group ##
- 存储组的信息
- 最后一列是组内的成员

## groupdel xx ##

## w ##
- 比 who 更详细的当前用户登录信息

## pwd ##
- 查看当前所在目录

## 特殊目录 ##
- .当前目录
- ..当前目录的上层目录
- 以.开始的都是隐藏目录，使用 ls -a 显示隐藏文件

## touch xx ##
- 创建文件
- 同名已存在的文件不会有影响，只会更新创建时间

## rm xx ##
- 删除文件

## mv xx xx ##
- 第一个参数要被移动的文件，第二个参数是移动到的目录
- mv test1.txt test2.txt 重命名

## cat xx ##
- -n 查看时增加行号

## head xx ##
- 查看前10行

## tail xx ##
- 查看后10行
- -f 动态查看文件结尾

## mkdir xx ##
- -p 可以递归创建子目录

## rmdir xx ##
- 只能删除空目录
- -r 可以删除非空目录

## cp xx xx ##
- 第一个参数要被复制的文件，第二个参数是复制到的目录或路径
- 可以复制文件和目录

## 文件权限 ##
- 2-4代表所有者权限 u
- 5-7代表文件所有组权限 g
- 8-10代表其他用户权限 o
- 每组都是rwx的组合
- 第二列代表连接数，子目录+2
- 第三列代表所有人
- 第四列代表所有组
- 第五列是文件大小
- 第六列是创建时间
- 第七列是文件名

## chmod ##
- 增加权限使用+，删除权限使用-,详细权限使用=
- chmod u+r
- chmod u+rw
- chmod u=rwx
- -R递归设置权限
- r=4 w=2 x=1

## chown xx xx ##
- 第一个参数为拥有者，第二个为文件名
- 第一个参数为:拥有者为改变用户组
- -R递归设置拥有者

## file xx ##
- 直接查看文件类型

## find PATH -name FILENAME ##
- 可以使用*模糊搜索

## which xx ##
- 用于从PATH变量所定义的目录中查找可执行文件的绝对目录

## gzip xx/gunzip xx ##
- 压缩和解压单个文件

## tar -zcvf xx.tgz /boot ##
- -z 使用gzip压缩
- -c 创建压缩文件
- -v 显示当前被压缩的文件
- -f 指使用文件名

## tar -zxvf boot.tgz ##
- 解压到当前目录
- -C指定目录 tar -zxvf boot.tgz -C /tmp

## 管道 ##
- 固定大小的缓冲区，大小为4K，使用频繁的通信机制
- 把一个命令输出的内容当做下一个命令的输入
- ls -l /etc/init.d | more

## grep ##
- -i 不区分大小写
- -c 统计包含匹配的行数
- -n 输出行号
- -v 反向匹配

## sort ##
- -n 数字排序
- -t 指定分隔符
- -k 指定第几列
- -r 反向排序

## ifconfig ##
- eth0 表示的是以太网第一块网卡
- lo表示主机环环回地址
- ifcongif eth0 x.x.x.x/24 修改ip
- ifdown xx
- ifup xx
- ifconfig 重启会丢失配置
- 网络配置在 /etc/sysconfig/network-scripts/ifcfg-ethx

## service network restart ##
- 断开后自动重启

## 网关操作 ##
- route add
- route del
- route -n
- 这种方式也是动态的

## DNS ##
- /etc/hosts

## 进程状态 ##
- 运行态、就绪态、阻塞态

## ps ##
- -A 所有进程
- -a 列出不和本终端有关的所有进程
- -w 显示加宽可以显示较多信息
- aux
- ps -ef  aux常用

## top ##
- 动态查看进程状态

## kill xx xx　##
- HUP（1）、KILL（9）、TERM（15），分别代表重启、强行杀掉、正常结束
- 重启不会改变主进程PID
- 强行杀掉可能会造成一定程度的内存泄露

## killall xx ##
- 直接使用进程名

## pidof xx ##
- 更快找到PID

## lsof ##
- 查询进程打开的文件
- lsof -i:xx 查看占用端口的进程

## nice\renice ##
- 优先级是-20~19，数值越低代表优先级越高
- 进程的最终优先级=优先级+nice优先级
- 默认使用0
- 普通用户也可以给自己的进程设定nice优先级，但是范围只限于0~19
- nice -n -10 ./job.sh
- renice -10 -p 5555

## 修改PATH变量 ##
- export PATH=$PATH:/root
- echo "export PATH=$PATH:/root" >> /etc/rc.local

## 使用源码编译Apache ##
- 在Linux系统中，一般在/usr/local/src/目录里下载源码包
- wget http://mirrors.cnnic.cn/apache/httpd/httpd-2.2.23.tar.gz
- 一般来说建议自行编译安装的软件放置的目录为/usr/local/
- configure
- make

## 临时关闭SeLinux ##
- /usr/sbin/setenforce 1 打开 
- /usr/sbin/setenforce 0 关闭
- getenforce 查询

## 永久关闭SeLinux ##
- /etc/selinux/config
- SELINUX=disabled

## 防火墙操作 ##
- /etc/init.d/iptables status 查询
- /etc/init.d/iptables stop 暂时关闭
- chkconfig --level 35 iptables off 永久关闭