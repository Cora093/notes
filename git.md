## **git**



### 概念

git是一款开源的分布式版本控制软件

github是流行的代码托管平台

- 文件的版本控制

	保存重要数据 恢复数据

- 版本控制软件的基础功能

	1. 保存和管理文件 自动添加版本号
	2. 提供客户端工具进行访问
	3. 比对不同版本的文件

- 集中式版本控制
	存在文件冲突覆盖问题——通过加锁解决——开发效率低

	![](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230425185154516.png)

- 分布式版本控制
	![](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230425185545159.png)

	通过添加本地仓库 将保存和上传/下载操作分开进行

### 使用

- 安装
	安装git和GitHubDesktop（一款基于git的第三方gui工具）
	
- 提交 `commit`
	文件发生变化后（与仓库中对比） 用户可以提交到本地仓库

	1. 本地仓库的存储路径是`.git`文件夹
	2. 每次提交的版本号是40个16进制数字，也叫提交码
		![image-20230425202815514](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230425202815514.png)

- 分支操作

	

	![image-20230425205332218](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230425205332218.png)

	![image-20230425205602872](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230425205602872.png)

	1. 添加分支
	2. 切换分支
		![image-20230425210133418](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230425210133418.png)
	3. 提交至分支
	4. 合并分支到主分支
		![image-20230425210335282](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230425210335282.png)
		如果分支矛盾 需要手动进行操作
	5. 为分支操作添加/删除tag
		![image-20230425211703882](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230425211703882.png)

- 远程仓库
	- github/gitee
	- push推送
	- pull拉取

- README文档
	说明文档，一般建议添加

- ignore功能
	忽略某个或某类不想上传的文件

- 版本号
	版本号是40个16进制数字 还能用于在object文件夹中定位对应文件
	通过Git Bash  就能解析出文件中的内容
	![image-20230427204714894](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230427204714894.png)

	删改增在仓库中的本质

	![image-20230427205212693](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230427205212693.png)

	分支操作的本质
	![image-20230427210018793](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230427210018793.png)























