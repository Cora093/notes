## mybatis-plus笔记

https://www.bilibili.com/video/BV12R4y157Be

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
       url: jdbc:mysql://localhost:3306/mybatis_plus_demo?characterEncoding=utf-8&useSSL=false
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

### BaseMapper基本功能测试

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
       //根据集合中的id批量删除
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

3. 修改功能测试

   ```java
       //根据ID修改
       @Test
       public void testUpdateById(){
           User user = new User();
           user.setId(8L);
           user.setName("Cora");
           user.setEmail("99999@abc.com");
           int res = userMapper.updateById(user);
           System.out.println(res);
       }
   ```

   ![image-20230703163117694](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230703163117694.png)

4. 查询功能测试

   ```java
       @Test
       public void testSelect() {
           //根据ID查
   //        User user = userMapper.selectById(1L);
   //        System.out.println(user);
   
           //根据ID批量查
   //        List<Long> list = Arrays.asList(1L, 2L, 8L);
   //        List<User> users = userMapper.selectBatchIds(list);
   //        users.forEach(System.out::println);
   
           //根据map中的条件查
   //        HashMap<String, Object> map = new HashMap<>();
   //        map.put("name", "Tom");
   //        map.put("age", 28);
   //        List<User> users = userMapper.selectByMap(map);
   //        users.forEach(System.out::println);
   
           //查询所有数据
           List<User> users = userMapper.selectList(null);
           users.forEach(System.out::println);
   
       }
   ```

   ![image-20230703164445014](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230703164445014.png)

   同样支持自定义方法、xml文件映射



### IService基本功能测试

1. 准备

   1. 创建Service接口继承IService

   2. 创建Service实现类 继承ServiceImpl类 实现Service接口

      ```java
      public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {}
      ```

2. ```java
       //测试查询总记录数
       @Test
       public void testCount() {
           long count = userService.count();
           log.info("查询到{}条记录", count);
       }
   ```

   ![image-20230703171007232](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230703171007232.png)

3. ```java
       //测试批量添加
       @Test
       public void testSaveBatch() {
           ArrayList<User> list = new ArrayList<>();
           for (int i = 0; i < 5; i++) {
               User user = new User();
               user.setName("Cora" + i);
               user.setAge(20 + i);
               list.add(user);
           }
           boolean res = userService.saveBatch(list);
           log.info("返回值为{}", res);
       }
   ```

   ![image-20230703171755043](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230703171755043.png)

### 常用注解

- 问题：实体类名和数据库表名不一致
  2种解决方法

  1. 在实体类加注解@Tablename("表名")
  2. 在yml配置文件中全局配置
     ![image-20230703172833732](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230703172833732.png)

- 问题：主键字段不为id/主键名和列名不一致/使用主键递增替代雪花算法

  - 为主键加注解@TableId 标记为主键

  - value值设置为列名

  - type值设置为`IdType.AUTO`
    ![image-20230703174215972](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230703174215972.png)

    ![image-20230703174342633](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230703174342633.png)

- 全局配置主键生成策略
  ![image-20230703174559457](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230703174559457.png)

- 问题：字段名和列名不一致
  同理 使用@TableField("字段名")

- @TableLogic 表示字段为逻辑删除
  添加该注解之后，删除操作和查询操作会自动改变

### 条件构造器wapper

![image-20230704100008357](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704100008357.png)

1. 组装查询条件

   ```java
       @Test
       public void test1() {
           // 查询用户名包含a,年龄在20-30之间，邮箱信息不为null的用户信息
           QueryWrapper<User> wrapper = new QueryWrapper<>();
           wrapper.like("name", "a")
                   .between("age", 20, 30)
                   .isNotNull("email");
           List<User> users = userMapper.selectList(wrapper);
           users.forEach(System.out::println);
       }
   ```

   ![image-20230704101109973](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704101109973.png)

2. 组装排序条件

   ```java
       @Test
       public void test2() {
           //查询用户信息，按照年龄的降序排序，若年龄相同，刚按照id升序排序
           QueryWrapper<User> wrapper = new QueryWrapper<>();
           wrapper.orderByDesc("age")
                   .orderByAsc("id");
           List<User> users = userMapper.selectList(wrapper);
           users.forEach(System.out::println);
       }
   ```

   ![image-20230704102037789](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704102037789.png)

3. 组装删除条件

   ```java
       @Test
       public void test3() {
           //删除邮箱为aaa@aaa.com的信息
           QueryWrapper<User> wrapper = new QueryWrapper<>();
           wrapper.eq("email", "aaa@aaa.com");
           int res = userMapper.delete(wrapper);
           System.out.println(res);
       }
   ```

   ![image-20230704102732565](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704102732565.png)

4. 条件修改

   ```java
       @Test
       public void test4() {
           //将（年龄大于20并且用户名中包含有a）或邮箱null的用户信息修改
           QueryWrapper<User> wrapper = new QueryWrapper<>();
           wrapper.gt("age", 20)
                   .like("name", "a")
                   .or()
                   .isNull("email");
   
           User user = new User();
           user.setEmail("test4@test.com");
   
           int res = userMapper.update(user, wrapper);
           System.out.println(res);
       }
   ```

   ![](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704103630859.png)

   ```java
       @Test
       public void test5() {
           //将用户名中包含有a并且（年龄大于20或邮箱为null)的用户信息修改
           //lambda表达式中的条件优先执行
           QueryWrapper<User> wrapper = new QueryWrapper<>();
           wrapper.like("name", "a")
                   .and(i -> i.gt("age", 20).or().isNull("email"));
   
           User user = new User();
           user.setEmail("test5@test.com");
   
           int res = userMapper.update(user, wrapper);
           System.out.println(res);
       }
   ```

   ![image-20230704104741723](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704104741723.png)

5. 查询指定字段

   ```java
       @Test
       public void test6() {
           //查询年龄==20的id, name信息
           QueryWrapper<User> wrapper = new QueryWrapper<>();
           wrapper.select("id", "name")
                   .eq("age", 20);
           List<Map<String, Object>> maps = userMapper.selectMaps(wrapper);
           System.out.println(maps);
       }
   ```

   ![image-20230704105445032](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704105445032.png)

6. QueryWrapper实现子查询

   ```java
       @Test
       public void test7() {
           //QueryWrapper实现子查询
           QueryWrapper<User> wrapper = new QueryWrapper<>();
           wrapper.like("name", "J")
                   .inSql("age", "select age from user where age < 22");
           List<User> users = userMapper.selectList(wrapper);
           users.forEach(System.out::println);
       }
   ```

   ![image-20230704110311939](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704110311939.png)

7. UpdateWrapper实现修改

   ```java
       @Test
       public void test8() {
           //UpdateWrapper实现修改
           //修改用户名包含J且年龄小于22的 邮箱
           UpdateWrapper<User> wrapper = new UpdateWrapper<>();
           wrapper.set("email", "test8@test.com")
                   .like("name", "J")
                   .lt("age", 22);
           int res = userMapper.update(null, wrapper);
           System.out.println(res);
       }
   ```

   ![image-20230704111101271](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704111101271.png)

8. 模拟开发中组装条件

   ```java
       @Test
       public void test9() {
           //模拟开发中组装条件
           String userName = "";
           Integer beginAge = 20;
           Integer endAge = 22;
   
           QueryWrapper<User> wrapper = new QueryWrapper<>();
           if(StringUtils.isNotBlank(userName)) {
               wrapper.like("name", userName);
           }
           if(beginAge != null){
               wrapper.ge("age", beginAge);
           }
           if(endAge != null) {
               wrapper.le("age", endAge);
           }
           List<User> users = userMapper.selectList(wrapper);
           users.forEach(System.out::println);
       }
   ```

   ![image-20230704112153500](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704112153500.png)

   优化：使用condition组装条件

   ```java
       @Test
       public void test10() {
           //模拟开发中组装条件 condition优化
           String userName = "";
           Integer beginAge = 20;
           Integer endAge = 22;
   
           QueryWrapper<User> wrapper = new QueryWrapper<>();
           wrapper.like(StringUtils.isNotBlank(userName), "name", userName)
                   .ge(beginAge != null, "age", beginAge)
                   .le(endAge != null, "age", endAge);
   
           List<User> users = userMapper.selectList(wrapper);
           users.forEach(System.out::println);
       }
   ```

   ![image-20230704112716283](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704112716283.png)

9. LambdaQueryWrapper 的使用

   ```java
       @Test
       public void test11() {
           //LambdaQueryWrapper 的使用
           String userName = "";
           Integer beginAge = 20;
           Integer endAge = 22;
   
           LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
           wrapper.like(StringUtils.isNotBlank(userName), User::getName, userName)
                   .ge(beginAge != null, User::getAge, beginAge)
                   .le(endAge != null, User::getAge, endAge);
   
           List<User> users = userMapper.selectList(wrapper);
           users.forEach(System.out::println);
       }
   ```

   效果相同，可以防止写错字段名

10. LambdaUpdateWrapper 的使用 同理

    ```java
        @Test
        public void test12() {
            //LambdaUpdateWrapper 的使用
            // 修改用户名包含J且年龄小于22的 邮箱
            LambdaUpdateWrapper<User> wrapper = new LambdaUpdateWrapper<>();
            wrapper.set(User::getEmail, "test12@test.com")
                    .like(User::getName, "J")
                    .lt(User::getAge, 22);
            int res = userMapper.update(null, wrapper);
            System.out.println(res);
    
        }
    ```

    ![image-20230704114328758](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230704114328758.png)

### 插件







