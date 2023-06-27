## mybatis-plus笔记



### 环境准备

1. 建表->添加数据

   ```sql
   CREATE DATABASE mybatis_plus;
   use mybatis_plus;
   CREATE TABLE user
   (
       id    bigint(20) NOT NULL COMMENT '主键ID',
       name  varchar(30) DEFAULT NULL COMMENT '姓名',
       age   int(11)     DEFAULT NULL COMMENT '年龄',
       email varchar(50) DEFAULT NULL COMMENT '邮箱',
       PRIMARY KEY (id)
   ) ENGINE = InnoDB
     DEFAULT CHARSET = utf8;
   
   
   INSERT INTO user (id, name, age, email)
   VALUES (1, 'Jone', 18, 'test1@baomidou.com'),
          (2, 'Jack', 20, 'test2@baomidou.com'),
          (3, 'Tom', 28, 'test3@baomidou.com'),
          (4, 'Sandy', 21, 'test4@baomidou.com'),
          (5, 'Billie', 24, 'test5@baomidou.com')
          
   ```

2. 添加依赖

   ```xml
   <!--依赖：mybatis-plus mysql lombok-->
   <dependency>
       <groupId>com.mysql</groupId>
       <artifactId>mysql-connector-j</artifactId>
       <scope>runtime</scope>
   </dependency>
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <optional>true</optional>
   </dependency>
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-boot-starter</artifactId>
       <version>3.5.1</version>
   </dependency>
   ```

3. 配置文件

   ```yaml
   spring:
     #配置数据源信息
     datasource:
       #数据源类型
       type: com.zaxxer.hikari.HikariDataSource
       #数据库的连接信息
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:3306/mybatis_plus?characterEncoding=utf-8&useSSL=false
       username: root
       password: 
   mybatis-plus:
     configuration:
       #驼峰转换
       map-underscore-to-camel-case: true
       #输出SQL语句
       log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
   ```

   ![image-20230627221518223](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230627221518223.png)

4. 实体类

   ```java
   @Data
   public class User {
       private Long id;
       private String name;
       private Integer age;
       private String email;
   }
   ```

5. 继承BaseMapper并配置扫描

   ```java
   public interface UserMapper extends BaseMapper<User> {
   }
   ```

   ```java
   @SpringBootApplication
   @MapperScan("com.example.mybatisplusdemo.mapper")
   public class MybatisPlusDemoApplication {
   
   	public static void main(String[] args) {
   		SpringApplication.run(MybatisPlusDemoApplication.class, args);
   	}
   
   }
   ```

6. 简单测试

   ```java
   @Slf4j
   @SpringBootTest
   class UserMapperTest {
       @Resource
       private UserMapper userMapper;
   
       @Test
       public void testSelectList() {
           List<User> userList = userMapper.selectList(null);
           for (User user: userList) {
               log.info("输出{}", user);
           }
       }
   }
   ```

   ![image-20230627223124980](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230627223124980.png)

### 基本功能测试

1. 新增功能测试

   ```java
       @Test
       public void testInsert() {
           User user = new User();
           user.setAge(22);
           user.setName("老王");
           user.setEmail("abcdefg@qq.com");
   
           int res = userMapper.insert(user);
           log.info("result:{}",res);
           log.info("id:{}",user.getId());
       }
   ```

   ![image-20230627225840909](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230627225840909.png)

2. 删除功能测试

   ```java
       //根据id删除    
   	@Test
       public void testDeleteById() {
           Long targetId = 1673707101237141506L;
           int res = userMapper.deleteById(targetId);
           log.info("res:{}", res);
       }
   ```

   ![image-20230627231524956](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230627231524956.png)

   ```java
       //根据Map中的条件删除
   	@Test
       public void testDeleteByMap() {
           Map<String, Object> map = new HashMap<>();
           map.put("name", "张三");
           map.put("age", 23);
           int res = userMapper.deleteByMap(map);
           log.info("res:{}", res);
       }
   ```

   ![image-20230627232022945](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230627232022945.png)

   ```java
       //根据集合中的id删除
       @Test
       public void testDeleteBatchIds() {
           ArrayList<Long> idList = new ArrayList<>();
           idList.add(4L);
           idList.add(1673713569113841666L);
           int res = userMapper.deleteBatchIds(idList);
           log.info("res:{}", res);
       }
   ```

   ![image-20230627232617747](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230627232617747.png)

3. 



