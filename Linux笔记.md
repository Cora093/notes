# Linux笔记





## Linux概述

### 发展历史

- Linux与Unix
- Linus的0.01版本
- 几个常见发行版

### 安装方法

1. 云服务器
2. vmware虚拟机
3. WSL



## Linux基础

### 基础目录结构

- Linux将硬件映射成文件进行管理，一切皆文件
- 树状目录结构
- 常见的目录结构

### 远程操作方式

- 远程登录 xshell
- 文件传输 xftp



### 开机、重启

常见开关机指令
<img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230728172145144.png" alt="image-20230728172145144" style="zoom: 50%;" />





## Linux实用指令

### vi和vim

三种模式 
切换到插入模式 a/i 
退出插入模式 esc
保存退出 :wq

<img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230728165844248.png" alt="image-20230728165844248"  />

<img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230728171113860.png" alt="image-20230728171113860"  />

常用操作
![image-20230728171658130](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230728171658130.png)



### 用户管理

su - cora			切换用户
logout				注销
useradd cora		添加用户
passwd cora		添加密码
userdel cora		删除用户 但保留目录（需要root权限)
userdel -r cora 	连目录一起删除 注意！（需要root权限)
id cora 				查询用户信息
whoami/who am i 查看当前用户
groupadd group1 新增组
groupdel group1	删除组
useradd -g group2 xiaowang 新增用户时直接指定组名(若创建时不指定，会自动分配一个同名组)
usermod -g group1 xiaowang 修改用户到其他组



### 运行级别

![image-20230808155743105](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230808155743105.png)



### root密码找回

[Linux（版本：高于CentOS7.6）找回root密码 超级详细！！！_郁宇风、黄的博客-CSDN博客](https://blog.csdn.net/weixin_52960369/article/details/131584441)



### 帮助指令

![image-20230808161048759](C:\Users\Cora\AppData\Roaming\Typora\typora-user-images\image-20230808161048759.png)



### 文件目录指令

pwd:显示当前绝对路径

ls,ll:显示当前目录下所有文件

cd:切换目录 cd~ cd..

mkdir 创建目录 -p 创建多级

rmdir 删除目录 有内容无法删除

强制删除 rm -rf 

touch 创建空文件

cp 复制 -r 复制整个目录 \cp强制覆盖不确认

rm 删除文件或目录 -f 强制删 -r 递归删

mv 移动/重命名

cat 查看文件内容 -n显示行号  | more

![image-20230808164130609](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230808164130609.png)

less 根据显示需要加载
![image-20230808164610803](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230808164610803.png)

echo 输出到控制台 `>`覆盖写入  `>>`追加写入

head显示文件头n行
tail同理

ln -s 软连接 相当于创建快捷方式

history 查看历史指令  !n 执行编号为n的指令



### 时间日期类

date 显示时间 date -s 设置系统时间
cal 显示日历



### 查找指令

find搜索
![image-20230808170845589](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230808170845589.png)

locate快速定位
![image-20230808171135618](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230808171135618.png)

grep 和 管道符`|`的搭配使用
eg: cat a.txt | grep "yes"
grep命令中 -n显示行号 -i忽略大小写



### 压缩和解压类

gzip/gunzip 压缩/解压 单个文件
zip/unzip 可以压缩文件夹
tar 打包
![image-20230808172756117](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230808172756117.png)



## 组管理和权限管理

### 组的概念

linux中每个用户都属于一个用户组

### 文件权限划分

- 所有者
  所有者一般是文件的创建者，修改文件所有者的指令:chown
- 所在组
  查看文件所在组:ll
  修改文件所在组:chgrp 组名 文件名
- 其他组

### Linux中的权限管理

![image-20230809145802082](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230809145802082.png)

![image-20230809150243945](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230809150243945.png)

chmod修改权限
![image-20230809150915766](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230809150915766.png)

![image-20230809152911730](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230809152911730.png)

<img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230809155132228.png" alt="image-20230809155132228" style="zoom:50%;" />



## 定时任务调度

### crond任务调度

crontab命令 设置定时任务 使用cron表达式
![image-20230809155900847](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230809155900847.png)



### at定时任务

一次性的定时任务
![image-20230809172502974](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230809172502974.png)



## Linux磁盘分区、挂载

### 原理

![image-20230810103444067](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230810103444067.png)

一个硬盘分区 **挂载** 到一个目录下

![image-20230810103530952](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230810103530952.png)

### 常用命令

lsblk:查看当前硬盘分区挂载情况

du:查看目录使用情况

tree:树状显示



## Linux网络配置

### 虚拟机的NAT网络配置原理

![image-20230810111705466](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230810111705466.png)

### 自动分配ip和指定ip

自动分配：避免ip冲突，但ip地址不固定
指定ip：ip固定

### 修改为指定ip

1. linux虚拟机修改配置文件
2. VMware设置修改

### linux虚拟机设置主机名

![image-20230810135734952](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230810135734952.png)

<img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230810135913477.png" alt="image-20230810135913477" style="zoom: 50%;" />

设置hosts映射之后，能通过主机名找到某个地址

![image-20230810140332435](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230810140332435.png)



## Linux进程相关

### 进程相关指令

ps指令查看进程
常用：ps -ef | grep

![image-20230815152514227](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815152514227.png)

杀死进程 kill -9 进程号


### 服务相关指令

![image-20230815153536833](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815153536833.png)

systemctl指令

![image-20230815161432950](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815161432950.png)

防火墙进行端口管理

![image-20230815162331341](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815162331341.png)

top指令动态监控进程

![image-20230815162646094](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815162646094.png)

netstat监控网络状态
![image-20230815163319956](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815163319956.png)



## RPM和YUM

### rpm

rpm包的概念

简单查询

![image-20230815165405815](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815165405815.png)

rpm包的卸载 安装

![image-20230815165751273](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815165751273.png)

![image-20230815165931684](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815165931684.png)



### yum

![image-20230815170148727](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815170148727.png)

 

## shell编程

### shell的概念

![image-20230815171319969](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815171319969.png)

### shell的执行方式

![image-20230815172016468](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815172016468.png)

### shell变量

![image-20230815173209342](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815173209342.png)

![image-20230815173313420](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230815173313420.png)







