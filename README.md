# TaaS API
TiDB hackthon 2022 Project.

## 团队介绍

团队名称：TiDB十一年老粉

团队成员：来自神州数码TiDB交付团队的 @hey-hoho @Orion7r @Mystery-cyf @CHENZMN

## 项目介绍

常规的应用开发流程都是直接面向数据库编程，通过编写各种各样的 SQL 脚本实现数据操作，但实际上大部分操作都是简单的增删改查，虽然有 ORM 框架能提升开发效率，但还是存在大量重复工作。

![](https://s1.sukiui.com/i/2022/10/02/aabi7a2.png)

本项目的核心理念是把数据库当做一种服务来提供（TiDB as a Service），即应用端面向 TiDB 的 API 编程。在一些比较简单的业务场景下，能够屏蔽繁琐的 SQL 增删改查操作，直接通过 RPC 调用达到数据操作目的。

这也是本项目取名 TaaS API 的由来。

这样的好处是，我们的应用底层有了一套具备高扩展性的分布式数据库，并且对应用透明（不需要知道有多少个节点、多少集群、部署架构）。其次，我们获得了一组自动生成的数据接口加速开发效率。最后，对于小型应用可以共用一个 TaaS API 平台，节省硬件资源，降低运维成本。

像表单收集类小程序（问卷调查、疫情风险申报等），就是非常典型的应用场景。

![](https://s1.sukiui.com/i/2022/10/02/1115glr.png)

## 背景&动机

**1、硬件资源痛点**

我们团队做过很多小型应用的 TiDB 迁移工作，在这个过程中有个非常严重的问题，就是对于这种小应用客户不愿意投入高配置的服务器来部署 TiDB 集群，往往就是3台4c16g这种虚拟机来部署，甚至也有打算一台虚拟机就上 TiDB 的。这种情况对我们 TiDB 运维人员来说非常痛苦，基本配置不达标后面会带来各种奇葩问题。

**2、让更多的小应用能使用分布式数据库**

有很多应用功能非常简单，数据量也不大，单独搭一套 TiDB 集群有点大材小用成本也扛不住。那怎么办？嗯，最简单的办法就是搞一套好点的集群大家都来用。那为什么不直接按业务系统分库？嗯，共用的时候还想能独占一部分资源。

**3、拯救crud boy**

让后端程序员有时间写点更好玩的东西，拥抱低代码。

## 项目设计

![](https://s1.sukiui.com/i/2022/10/02/1115hjs.png)

分布式业务数据库系统提供了一组抽象的数据服务，使用者无需关心具体的数据存储方式和存储结构，只需要使用系统提供的设计器快速创建业务实体和关系，然后使用丰富的 API 即可投入到业务开发，快速构建应用程序，服务底层对使用者完全透明。

现阶段我们希望 TaaS API 能提供以下核心能力：

**1、模型设计器**

用 TaaS API 来做应用开发根本不需要知道数据连接地址，甚至不用写一行 SQL 代码，所有的 Schema 变更都通过可视化模型设计器来完成，就算随便找个产品经理一点都不懂 SQL 的也能上手就用。

**2、自动生成模型API**

在模型设计完成后 TaaS API 会帮你生成可调用的服务，提供 Restful 和 GraphQL 两套 API，真正实现了跨平台和跨语言，最简单的情况下js就能直接调用，再也不用担心前后端互相甩锅了。

**3、API调试功能**

为了方便测试接口，提供调试工具也是必不可少的，建完模型马上就可以拿数据来验证，不需要额外装各种调试工具。

**4、多实例支持**

这里的实例可以理解为业务应用，每个应用有自己独立的模型和 API，独立的后端进程，每个应用可以实现简单的生命周期管理（创建、发布、销毁等）。

## 项目展望

- 1、进一步降低应用开发门槛（提供 SDK？进一步实现低代码设计？）。

- 2、给每个业务应用提供独立的域名、访问用户、权限等（实例透明）。

- 3、API 租户，资源隔离（依赖 TiDB 多租户能力）。

- 4、多环境支持（dev、nat、prod），一键迁移。

- 5、可以考虑嵌入到 TiDB Cloud 中，作为实例的一个扩展功能提供服务。

- 6、商业化的思考（API 调用量、带宽、容量、OPS 等计费模型）。

...