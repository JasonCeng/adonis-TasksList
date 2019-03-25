# 使用Adonis.js构建一个任务列表Web App

AdonisJS是一个Node.js MVC框架。它提供了一个稳定的生态系统来编写Web服务器，以便您可以专注于业务需求而不是确定最终要选择的包。在本项目中，我将向您展示如何使用AdonisJS构建Web应用程序。

## 目录
* 我们将做的事情
* 项目需求
* 下载Adonis CLI
* 新建项目
* 创建数据库
* 进行Migration
* 创建Task Model
* 创建Routes
* 创建Task Controller
* 创建Master Layout
* 创建Task View
* 添加新Task
* 显示Notification
* 删除一个Task
* 总结

## 我们将做的事情
为了了解如何使用AdonisJS构建应用程序，我们将构建一个简单的任务列表（todo）应用程序。 我们将在本项目中使用AdonisJS 4.0。

接下来我们将用到Adonis.js的以下特性进行构建。

1. Bodyparser
2. Session
3. Authentication
4. Web security middleware
5. CORS
6. Edge template engine
7. Lucid ORM
8. Migrations and seeds

## 项目需求
* Node.js 8.0 or greater
* Npm 3.0 or greater

## 下载Adonis CLI
我们需要首先安装Adonis CLI，它将帮助我们创建新的AdonisJS应用程序，还附带一些有用的命令：
```bash
npm i -g @adonisjs/cli
```

## 新建项目

使用adonis命令来搭建项目骨架

```bash
adonis new adonis-TasksList
```
或者你也可以手动把adonis的仓库clone下来再运行`npm install`

### 创建数据库
我们将从构建应用程序数据库开始。 我们将使用AdonisJS迁移模式来定义应用程序的数据库模式。 在我们深入了解迁移之前，让我们快速花一些时间来设置我们的数据库。


首先我们安装Node.js的mysql驱动，以便让我们的项目能顺利连接mysql数据库
```bash
npm install mysql --save
```

接下来，我们需要让AdonisJS知道我们正在使用MySQL。 看一下`config/database.js`，您会看到包括MySQL在内的不同数据库的配置设置。虽然我们可以直接在配置文件中输入我们的MySQL设置，但这意味着我们每次更改应用程序环境（开发，升级，生产等）时都必须更改这些设置，这实际上是一种不好的做法。

相反，我们将使用环境变量，并根据运行应用程序的环境，它将拉取该环境的设置。 AdonisJS让我们在这里得到了保障。 我们所要做的就是在`.env`文件中输入我们的配置设置。

所以，打开`.env`文件并设置如下：
```bash
// .env

DB_CONNECTION=mysql
DB_HOST=localhost
DB_DATABASE=adonisTasksList
DB_USER=root
DB_PASSWORD=
```
请记得使用您自己的数据库设置相应地更新数据库名称，用户名和密码。

## 进行Migration
为简单起见，我们的应用程序将只有一个数据库表tasks。 tasks表将包含3个字段id，title，created_at和updated_at。 我们将使用adonis make：migration命令创建迁移文件：
```bash
adonis make:migration tasks
```
在提示时选择Create table选项，然后按Enter键。 这将在database / migrations目录中创建一个新文件。 文件名将是一个带有模式名称的时间戳（在我的例子中为1504289855390_tasks_schema.js）。 打开此文件并更新up（），如下所示：
```javascript
// database/migrations/1504289855390_tasks_schema.js

up () {
  this.create('tasks', (table) => {
    table.increments()
    table.string('title')
    table.timestamps()
  })
}
```
incrementments（）将创建一个带有自动增量的id字段并设置为主键。 timestamps（）将分别创建created_at和updated_at字段。 完成后，我们可以运行迁移：
```bash
adonis migration:run
```
## 创建Task Model
设置好数据库和表格后，我们现在创建一个model。 我们称之为`Task`。 虽然我们不会在本教程中广泛使用该模型，但我们将使用模型而不是编写普通数据库查询，因为它们带来了易用性，并提供了一个富有表现力的API来驱动数据流，并允许我们使用Lucid（AdonisJS ORM）。 要创建模型，我们使用`adonis make:model`命令：
