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

### 目录结构

- Linux将硬件映射成文件进行管理，一切皆文件
- 树状目录结构
- 常见的目录结构

### 远程操作

- 远程登录 xshell
- 文件传输 xftp

### vi和vim

三种模式 
切换到插入模式 a/i 
退出插入模式 esc
保存退出 :wq

<img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230728165844248.png" alt="image-20230728165844248"  />

<img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230728171113860.png" alt="image-20230728171113860"  />

常用操作
![image-20230728171658130](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230728171658130.png)



### 开机、重启

常见开关机指令
<img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230728172145144.png" alt="image-20230728172145144" style="zoom: 50%;" />



### 用户管理

su - cora			切换用户
logout				注销
useradd cora		添加用户
passwd cora		添加密码
userdel cora		删除用户 但保留目录（需要root权限)
userdel -r cora 	连目录一起删除 注意！
id cora 				查询用户信息
whoami/who am i 查看当前用户
groupadd group1 新增组
groupdel group1	删除组
useradd -g group2 xiaowang 新增用户时直接指定组名(若创建时不指定，会自动分配一个同名组)
usermod -g group1 xiaowang 修改用户到其他组



### Linux实用指令

