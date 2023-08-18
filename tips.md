### SpringBoot多环境配置

1. 配置启动环境 

   根据应用系统中常见的三个运行环境拆分成了多个不同的配置文件，分别独立配置上面各运行环境的配置项。具体如下所示：

   application.yml为项目主配置文件，包含项目所需的所有公共配置。application-dev.yml为开发环境配置文件，包含项目所需的单独配置。application-test.yml为测试环境配置文件。
   application-prod.yml为生产环境配置文件。

2. 切换启动环境

   1. **配置文件**指定项目启动环境

      在application.yml配置文件中增加如下配置项：

      spring.profiles.active=dev

   2. IDEA**编译器指定**项目启动环境

      一般在IDEA启动时，直接在IDEA的Run/debug Configuration页面配置项目启动环境。

      项目调试运行时，IDEA编译器可以通过VM options、Program arguments、Active profiles三个参数设置启动方式。

   3. **命令行**启动指定项目环境

      在命令行通过java-jar命令启动项目时，需要如下指定启动环境：java -jar xxx.jar--spring.profiles.active=dev



### 项目部署

1. 原始部署
   前端

   - 需要web服务器：nginx等
   - 安装nginx：
     1. 系统自带的包管理器yum/apt
     2. 官网下载安装包(本地下载上传/curl下载/wget下载) 并解压
   - 注意更改nginx的config文件 user项 和 location-root项

   后端

   - 需要java maven git

   - ```bash
     yum install -y java-1.8.0-openjdk*
     
     curl -o apache-maven-3.8.5-bin.tar.gz https://dlcdn.apache.org/maven/maven-3/3.8.5/binaries/apache-maven-3.8.5-bin.tar.gz
     
     git clone xxx 下载代码
     
     打包构建，跳过测试
     mvn package -DskipTests
     
     nohup java -jar ./user-center-backend-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod &
     ```

2. 宝塔

3. 容器——docker
   使用 Docker 容器技术，理论上可以封装任何环境和应用，对于后端 Java 项目来说，把 Java 环境、Maven 和 jar 包封装成一个镜像就好了。

   在写 Dockerfile 时，可以直接使用 maven:3.5-jdk-8-alpine 这种基础镜像，自带了 jdk 和 maven，省去了自己写安装脚本的麻烦。 

   Dockerfile 用于指定构建 Docker 镜像的方法

   Dockerfile 一般情况下不需要完全从 0 自己写，建议去 github、gitee 等托管平台参考同类项目（比如 springboot）

   Dockerfile 编写：

   - FROM 依赖的基础镜像
   - WORKDIR 工作目录
   - COPY 从本机复制文件
   - RUN 执行命令
   - CMD / ENTRYPOINT（附加额外参数）指定运行容器时默认执行的命令

   根据 Dockerfile 构建镜像：

   ```
   # 后端
   docker build -t user-center-backend:v0.0.1 .
   
   # 前端
   docker build -t user-center-front:v0.0.1 .
   ```

   Docker 构建优化：减少尺寸、减少构建时间（比如多阶段构建，可以丢弃之前阶段不需要的内容）

   

   docker run 启动

