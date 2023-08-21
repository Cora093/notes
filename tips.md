

## 项目部署相关

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

### Docker 部署

docker 是容器，可以将项目的环境（比如 java、nginx）和项目的代码一起打包成镜像，所有同学都能下载镜像，更容易分发和移植。

再启动项目时，不需要敲一大堆命令，而是直接下载镜像、启动镜像就可以了。

docker 可以理解为软件安装包。

Docker 安装：https://www.docker.com/get-started/ 或者宝塔安装



Dockerfile 用于指定构建 Docker 镜像的方法

Dockerfile 一般情况下不需要完全从 0 自己写，建议去 github、gitee 等托管平台参考同类项目（比如 springboot）

Dockerfile 编写：

- FROM 依赖的基础镜像
- WORKDIR 工作目录
- COPY 从本机复制文件
- RUN 执行命令
- CMD / ENTRYPOINT（附加额外参数）指定运行容器时默认执行的命令

根据 Dockerfile 构建镜像：

```bash
# 后端
docker build -t user-center-backend:v0.0.1 .

# 前端
docker build -t user-center-front:v0.0.1 .
```

Docker 构建优化：减少尺寸、减少构建时间（比如多阶段构建，可以丢弃之前阶段不需要的内容）

docker run 启动：

```bash
# 前端
docker run -p 80:80 -d user-center-frontend:v0.0.1

# 后端
docker run -p 8080:8080 user-center-backend:v0.0.1
```

虚拟化

1. 端口映射：把本机的资源（实际访问地址）和容器内部的资源（应用启动端口）进行关联
2. 目录映射：把本机的端口和容器应用的端口进行关联



进入容器：

```bash
docker exec -i -t  fee2bbb7c9ee /bin/bash
```



查看进程：

```bash
docker ps 
```



查看日志：

```bash
docker logs -f [container-id]
```



杀死容器：

```bash
docker kill
```



强制删除镜像：

```bash
docker rmi -f
```







### 查看端口占用

`netstat -ntlp`



### 跨域问题解决

浏览器为了用户的安全，仅允许向 **同域名、同端口** 的服务器发送请求。

如何解决跨域？

最直接的方式：把域名、端口改成相同的

#### 添加跨域头

让服务器告诉浏览器：允许跨域（返回 cross-origin-allow 响应头）

1. 网关支持（Nginx）

```nginx
# 跨域配置
location ^~ /api/ {
    proxy_pass http://127.0.0.1:8080/api/;
    add_header 'Access-Control-Allow-Origin' $http_origin;
    add_header 'Access-Control-Allow-Credentials' 'true';
    add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
    add_header Access-Control-Allow-Headers '*';
    if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Origin' $http_origin;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain; charset=utf-8';
        add_header 'Content-Length' 0;
        return 204;
    }
}
```

2. 修改后端服务

1. 配置 @CrossOrigin 注解

2. 添加 web 全局请求拦截器

   ```java
   @Configuration
   public class WebMvcConfg implements WebMvcConfigurer {
    
       @Override
       public void addCorsMappings(CorsRegistry registry) {
           //设置允许跨域的路径
           registry.addMapping("/**")
                   //设置允许跨域请求的域名
                   //当**Credentials为true时，**Origin不能为星号，需为具体的ip地址【如果接口不带cookie,ip无需设成具体ip】
                   .allowedOrigins("http://localhost:9527", "http://127.0.0.1:9527", "http://127.0.0.1:8082", "http://127.0.0.1:8083")
                   //是否允许证书 不再默认开启
                   .allowCredentials(true)
                   //设置允许的方法
                   .allowedMethods("*")
                   //跨域允许时间
                   .maxAge(3600);
       }
   }
   ```

3. 定义新的 corsFilter Bean，参考：https://www.jianshu.com/p/b02099a435bd

