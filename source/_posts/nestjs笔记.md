---
title: nestjs学习笔记
date: 2024-09-12
categories:
    - [学习笔记]
tags:
    - nestjs
---

1. NestJS 基本模块系统
imports 和 providers 的区别：

imports 用于引入其他模块的功能（如 ConfigModule），让当前模块可以使用它们导出的服务或其他资源。
providers 用于声明当前模块中可以注入的服务（如 UsersService），这些服务可以被其他组件（如控制器、守卫）注入使用。
模块之间的依赖关系：

模块可以通过 exports 导出自己的服务，供其他模块 imports 使用，实现模块之间的依赖注入。
2. JWT（JSON Web Token）鉴权
JWT 策略：通过 JwtStrategy 验证请求中的 JWT 令牌，使用 ConfigService 来获取 JWT 的密钥。
生成和验证 JWT：
使用 JwtService.sign() 方法在用户登录时生成 JWT，包含如 userId、role 等信息。
JwtStrategy.validate() 方法用于验证 JWT，并将解码后的用户信息传递给守卫或控制器。
3. NestJS 守卫（Guards）
AuthGuard('jwt')：用于检查请求中是否携带有效的 JWT 令牌，并阻止未授权请求访问保护的路由。
自定义守卫：例如 RolesGuard，用于在路由处理前执行自定义的逻辑（如检查用户是否具有某个角色）。
4. 基于角色的访问控制
定义角色枚举：通过 Role 枚举表示用户的不同角色，如 Admin、Waiter、User。
自定义角色装饰器 @Roles()：用于将角色元数据绑定到路由处理程序上。
ROLES_KEY：用于在元数据中存储角色信息。
RolesGuard：守卫通过 Reflector 从路由中获取角色元数据，并检查当前用户是否具有访问所需的角色。
5. Prisma ORM
Prisma 服务：使用 PrismaService 来与数据库进行交互，提供 findUnique、create 等方法操作用户数据。
用户角色存储：在 Prisma 数据库中使用整数字段存储用户角色，如 0 表示管理员，1 表示服务员，2 表示普通用户。
6. 类型和 DTO
DTO（Data Transfer Object）：通常用于定义传入或传出的数据结构，在控制器中使用。
错误提示 Cannot find module './dto/role' 提醒我们要确保文件和导入路径正确无误，避免文件命名或路径大小写问题。
7. NestJS 自定义装饰器
@SetMetadata()：NestJS 提供的工具，用于将自定义的元数据附加到控制器或处理程序上，比如通过 @Roles() 装饰器设置角色要求。
8. 项目构建与编译
清理和重新编译项目：在项目发生结构或路径变化时，清理编译输出（如 dist 目录）并重新构建，以确保所有更改生效。
