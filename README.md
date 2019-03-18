# 使用Adonis.js构建一个任务列表Web App

AdonisJS是一个Node.js MVC框架。它提供了一个稳定的生态系统来编写Web服务器，以便您可以专注于业务需求而不是确定最终要选择的包。在本项目中，我将向您展示如何使用AdonisJS构建Web应用程序。

接下来我们将用到Adonis.js的以下特性进行构建。

1. Bodyparser
2. Session
3. Authentication
4. Web security middleware
5. CORS
6. Edge template engine
7. Lucid ORM
8. Migrations and seeds

## 搭建项目(build)

使用adonis命令来搭建项目骨架

```bash
adonis new adonis-T
```
或者你也可以手动把adonis的仓库clone下来再运行`npm install`

### 数据迁移(Database and Migrations)

首先我们安装Node.js的mysql驱动，以便让我们的项目能顺利连接mysql数据库
```bash
npm install mysql --save
```
