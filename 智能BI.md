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
4. AI切换？自定义优化？



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



TODO 设计数据库表
索引添加

### 数据库表设计

```SQL
-- 用户表
create table if not exists fastbi.user
(
    id           bigint auto_increment comment 'id'
        primary key,
    userAccount  varchar(256)                           not null comment '账号',
    userPassword varchar(512)                           not null comment '密码',
    userName     varchar(256)                           null comment '用户昵称',
    userAvatar   varchar(1024)                          null comment '用户头像',
    userRole     varchar(256) default 'user'            not null comment '用户角色：user/admin',
    createTime   datetime     default CURRENT_TIMESTAMP not null comment '创建时间',
    updateTime   datetime     default CURRENT_TIMESTAMP not null on update CURRENT_TIMESTAMP comment '更新时间',
    isDelete     tinyint      default 0                 not null comment '是否删除'
)
    comment '用户' collate = utf8mb4_unicode_ci;

create index user_userAccount_index
    on fastbi.user (userAccount);
```