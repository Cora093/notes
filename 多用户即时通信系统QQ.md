# 多用户即时通信系统QQ

## 分析阶段

### 项目开发流程

需求分析->设计阶段->实现阶段->测试阶段->实施阶段->维护阶段

### 需求分析

1. 用户登录
2. 拉取在线用户列表
3. 无异常退出
4. 私聊
5. 群聊
6. 发文件
7. 服务器推送新闻

### 总体思路分析

![image-20230317000320267](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230317000320267.png)

### 项目初始化

- 新建项目 QQServer 和QQClient
- 定义共有类 user和message 需要实现serializable接口
- user: id passwd 
- massage属性: sender receiver content sendTime msgType(接口实现)
-  <img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230317004100703.png" alt="image-20230317004100703" style="zoom:50%;" />

## 功能实现

### 用户登录

#### 客户端代码

1. 客户端菜单 qqclient.view QQView
2. <img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230317010202736.png" alt="image-20230317010202736" style="zoom:67%;" />
3.  qqclient.utils 导入工具包
4. 输入用户名和密码后 需要到服务端去验证 暂时true 后面补 
5. 验证完毕进入二级菜单（菜单中需要提示用户uid）
6. <img src="https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230317010245830.png" alt="image-20230317010245830" style="zoom:50%;" />
7. 实现 验证登录 qqclient.service UserClientService类 该类完成用户登录验证和用户注册等功能
8. user socket 由于要重复使用 所以做成属性 
9. 方法checkUser 返回boolean 连接到服务器9999端口  发送User对象到服务器 再从服务器读取回复的Message对象（使用对象流）
	通过MessageType得到登录成功或者失败的信息
10. 登录成功 创建一个线程 ClientConnectServerThread 和服务器保持通信 传入socket对象  启动线程
11. 创建ClientConnectServerThread类 继承Thread 接收socket 重写run方法 run方法中while循环 对象流持续接收Message
12. 为了后面客户端的扩展，将线程放入到集合管理 ClientThreadManager类
	该类管理客户端连接到服务器端的线程 使用hashmap集合(static) key是id value是线程
	编写add get方法(通过id获取对应线程)
13. 如果登录失败 关闭socket
14.  UserClientService类 作为成员放入类中  登录判断条件改为checkUser 方法

#### 服务端代码

1. qqserver.service QQServer类 服务端代码 ServerSocket 持续监听9999端口
2.  while循环持续监听 不中断 读取user对象 并验证 100 123456  （暂时）
3. 创建Message对象 回复对应结果 
	（如果连接成功）创建一个ServerConnectClientThread 线程，和客户端保持通信 并启动线程
4. 创建ServerConnectClientThread 继承Thread 接收socket 属性还需要有userid 重写run方法 循环得到Message
	（message暂时不使用 之后会使用）
5. 创建集合管理线程 ServerThreadManager类 该类用于管理服务端的线程 使用HashMap
	与前面相同 add get方法
6. 如果连接失败就不创建线程 返回失败信息Message 关闭socket 
7. 服务器退出while循环 就代表不再监听  需要关闭ServerSocket （在finally分支关闭）、
8. 建包qqframe QQFrame类 该类创建QQServer对象 作为程序入口
9. 优化2的判断：
	在QQServer中 创建一个静态集合validUsers 存放合法用户 来模拟数据库 实现多用户登录
	**注意使用ConcurrentHashMap,可以处理并发的集合，没有线程安全问题**
	在静态代码块，初始化validUsers 添加数据
	checkUser 服务端方法 通过集合中的数据 判断账号密码是否有效
	注意**分别判断账号和密码** 达到提示用户的效果

#### 程序调试

1. 报错 User类初始化错误 原因：一端加了构造函数 另一端没同步 解决方法：另一端也加上 
2. 客户端不能并行运行 解决方法：在设置中打开->允许多个实例



### 拉取在线用户列表

#### 客户端代码

1. qqcommon MessageType类 新增常量
	MESSAGE_COMMON_MES //普通信息
	MESSAGE_GET_ONLINE_FRIEND //客户端要求返回在线用户列表
	MESSAGE_RET_ONLINE_FRIEND //返回在线用户列表
	MESSAGE_CLIENT_EXIT //客户端请求退出
	**注意两端信息统一**

2. 在UserClientService中 新增方法 onlineFriendList  //向服务端请求在线用户列表

	获取当前线程的socket 的 对象输出流 oos：先从集合中通过id找到线程 再找到线程中socket的输出流 构建oos

	构建一个对应类型的Message  通过oos发送

3. 在客户端线程的run方法中 判断服务端回复的Message 类型是否是RET 
	如果不是 暂时不处理 如果是 就取出在线信息列表并显示 便于使用split方法处理

#### 服务端代码

1. 在服务端线程的run方法中 判断客户端发送的Message 类型是否是GET
	如果不是 暂时不处理 如果是 就获取在线用户列表
2. 在Manager类中编写方法 getOnlineUser() 返回在线用户列表（String 用空格隔开）
	遍历集合 遍历HashMap的key  （取得keyset 用iterator遍历）
3. run方法中构建Message setType为RET setContent为得到的字符串 
	在oos中返回给客户端

#### 程序调试

1. 服务端sender显示错误 原因：客户端 Message初始化的时候忘记设置 



### 无异常退出

- 问题：客户端输入9 无法正确退出 
- 原因：主线程虽然结束了 但**子线程**（通信线程）还在运行 没有关闭 导致无法退出  
- 解决方法：
- **客户端**：在main线程调用方法,给服务器端发送一个退出系统的message对象
	对象调用System.exit(0)正常退出
	logout() //退出客户端，并给服务端发送一个退出系统的message对象

- **服务端**：和客户端通信的线程 如果接收到退出系统的Message 
	就把这个线程持有的socket关闭 （Manager中增加一个方法） 并退出线程

### 私聊功能

#### 客户端

1. 接收用户希望给某个其它在线用户聊天的内容
2. 将消息构建成Message对象，通过对应的socket发送给服务器
3. 在通信线程），读取到发送的Message消息，并显示即可

![image-20230320133619204](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230320133619204.png)



#### 服务端

1. 可以读取到客户端发送给某个客户的消息 
2. 从管理线程的集合中，根据message对象的getterid获取到对应线程的socket
3. 然后将message对象转发给指定客户



### 群发功能

大体和私聊差不多 服务端需要遍历集合、排除自己之后转发消息



### 文件发送

暂时跳过

### 服务端推送新闻

#### 服务端

写一条独立线程 专门推送新闻 

推送逻辑类似于群发 需要在QQService线程中启动该线程



