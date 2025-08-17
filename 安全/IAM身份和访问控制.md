# 1. 什么是IAM

身份(Identity)+访问(Access)控制，都包含配置、操作两个步骤，形成4个项目

身份配置：注册（定义凭证）

身份操作：认证

访问配置：授权

访问操作：鉴权

# 2. 为什么需要IAM

1. 提高系统整体安全性
2. 各子系统不必建设IAM，降低成本，提高开发效率
3. 支持SSO、联邦身份认证，提供用户体验

# 3. 核心概念

## 3.1. EIAM vs CIAM

EIAM：ToB的IAM

CIAM：ToC的IAM

## 3.2. 用户、账号、身份

## 3.3. 认证、授权、鉴权

## 3.4. IdP、SP

IdP：身份提供方。负责认证、鉴权，生成认证令牌供SP使用，一般由IAM充当。

SP：服务提供方。负责向用户提供服务，SP不直接对用户认证，而是重定向到IdP。

1. 单点登录（SSO）
2. 联邦身份认证（FIM）
3. 身份代理（Identity Broker）

## 3.5. 访问控制模型

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/安全/images/IAM身份和访问控制/1724663730566-221b038e-c7ba-4b6c-96ba-f7f66b599f09.png)

ACL：基于权限列表。需要穷举所有权限，并关联到用户上，建设简单但扩展性低

RBAC：基于角色。解耦用户和权限，比ACL更加灵活。从能力上分为RBAC0、RBAC1、RBAC2、RBAC3，见[论文](https://profsandhu.com/articles/advcom/a98rbac.pdf)

ABAC：基于属性，或基于策略。使用用户组、角色组，以及如部门、访问时间、资源分类等各种数据进行访问控制，建设成本高，但具有很强灵活性、扩展性。

# 4. 协议规范

JWT