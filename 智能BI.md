## 智能BI



### 项目介绍

智能BI数据可视化平台

帆软BI、微软Power BI

> BI，全称商业智能（Business Intelligence），是一种通过各种技术进行数据分析以辅助决策，提升决策效率的方案。它通常被视为一套完整的解决方案，用于将企业的数据有效整合，快速制作出报表以作出决策。BI在数据架构中处于前端分析的位置，其核心作用是对获取的企业数据进行分析和挖掘，为企业提供有价值的信息和洞察，从而帮助企业做出更明智的商业决策。



传统BI平台：
https://chartcube.alipay.com/
需要人工上传、分析、选择 

智能BI平台：
只需导入最原始的数据集和分析目标，就能得到图表和结论
**节约人力成本**



![image-20231109155354414](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20231109155354414.png)





### 需求分析

1. 智能分析，自动生成图表和结论
2. 图表管理
3. 异步化图表生成（消息队列）
4. 扩展：AI切换/自定义优化



### 技术栈

#### 前端



#### 后端

1. SpringBoot
2. MySQL
3. Mybatis Plus
4. 消息队列(RabbitMQ)
5. AI接口
6. EasyExcel 解析数据表格
7. Swagger + Knife4j 项目接口文档
8. Hutool工具库




### 数据库表设计

```SQL
-- 用户表
create table if not exists fastbi.user
(
    id            bigint auto_increment comment 'id'
        primary key,
    user_account  varchar(256)                           not null comment '账号',
    user_password varchar(512)                           not null comment '密码',
    user_name     varchar(256)                           null comment '用户昵称',
    user_avatar   varchar(1024)                          null comment '用户头像',
    user_role     varchar(256) default 'user'            not null comment '用户角色：user/admin',
    create_time   datetime     default CURRENT_TIMESTAMP not null comment '创建时间',
    update_time   datetime     default CURRENT_TIMESTAMP not null on update CURRENT_TIMESTAMP comment '更新时间',
    is_delete     tinyint      default 0                 not null comment '是否删除'
)
    comment '用户' collate = utf8mb4_unicode_ci;

create index user_user_account_index
    on fastbi.user (user_account);


```



``` SQL
-- 图表信息表
create table if not exists fastbi.chart
(
    id              bigint auto_increment comment 'id'
        primary key,
    name            varchar(256)                       not null comment '图表名称',
    goal            text                               null comment '任务分析目标',
    origin_data     text                               null comment '原始输入数据',
    chart_type      varchar(128)                       null comment '图表类型',
    generate_chart  text                               null comment '生成的图表数据',
    generate_result text                               null comment '生成的分析结论',
    user_id         bigint                             null comment '创建者id',
    create_time     datetime default CURRENT_TIMESTAMP not null comment '创建时间',
    update_time     datetime default CURRENT_TIMESTAMP not null on update CURRENT_TIMESTAMP comment '更新时间',
    is_delete       tinyint  default 0                 not null comment '是否删除'
)
    comment '图表信息表' collate = utf8mb4_unicode_ci;
```



### **智能分析模块开发**

1. 用户输入
   1. 分析目标
   2. 上传原始数据(excel)
   3. 控制图表类型、图表名称等
2. 后端校验
   1. 校验用户的输入否合法(比如长度）
   2. 图表数据压缩处理
   3. 成本控制(次数统计和校验、鉴权等)
3. 把处理后的数据输入给AI模型，调用AI接口返回图表信息、结论文本
4. 图表信息(是一段json配置，是一段代码)、结论文本在前端进行展示



### **AI提问技巧**

prompt

```
你是一个数据分析师和前端开发专家，接下来我会按照以下固定格式给你提供内容：
分析需求：
{数据分析的需求或者目标}
原始数据：
{csv格式的原始数据，用','作为分隔符，用'\n'作为换行符}
请根据这两部分内容，按照以下指定格式生成内容（此外不要输出任何多余的开头、结尾、注释）
【【【
前端Echarts V5的option配置对象js代码，合理地将数据进行可视化，不要生成任何多余的内容，比如注释
【【【
{明确的数据分析结论（多于60字 越详细越好），不要生成多余的注释}
```



原始数据

```
分析需求：
分析网站的用户访问数据 预测网站未来的发展
原始数据：
日期,访问量,用户数\n2022/1/1,1000,500\n2022/2/2,1200,550\n2022/3/3,900,480\n2022/4/4,1100,520\n2022/5/5,1300,570\n2022/6/30,1500,650\n
```





### AI接口调用

1. web接口
   openAI、讯飞星火等
2. 云服务商封装过的接口
3. SDK
   鱼聪明等



