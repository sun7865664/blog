---
title: AppSmith低代码平台
language: zh-CN
date: 2022-05-30 15:53:41
tags:
- AppSmith
- 低代码平台
- 开发框架
---
### AppSmith低代码平台
#### 关键字
#开发框架 #低代码平台 #AppSmith

#### 一、简介

  

> `Appsmith` 是一个用于构建内部应用程序的低代码、开源框架。通过拖放组件来构建完全自定义的管理面板、`CRUD` 应用程序和工作流。使用 `30` 多个 `React` 组件来构建没有 `HTML/CSS` 的页面。

  

##### 1、工作原理

  

![img](https://img-blog.csdnimg.cn/img_convert/29914d5f5db5c90d53808ec822bfd974.png)

  

- 如图所示，AppSmith 采用的是 proxy 服务器执行的架构。即查询会经过一次 AppSmith 的服务器，再到目的数据库或者服务器，实现跳转查询。

  

#### 二、安装和注册、登录

  

> AppSmith 本身是开源软件，但是它提供一个云端版本,可以直接注册使用。也可以本地私有部署，官方提供了多种部署方案。

  

##### 1、互联网云端版本使用

  

- 直接注册登录即可使用

  - [AppSmith 云端注册地址](https://app.appsmith.com/user/signup)

  - [AppSmith 云端登陆地址](https://app.appsmith.com/user/login)

  

##### 2、本地私有部署

  

> AppSmith支持Docker,Kubernetes,AWS AMI,AWS ECS,DigitalOcean,Heroku Guide,Ansible等多种部署方式，官方唯一支持的部署方式是docker方式。

  

##### 2.1、docker部署

  

- 使用Docker-compose部署，可以获取最新版本
	- 安装docker及docker-compose
	- [下载docker-compose.yml配置文件](https://3084404454-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Lzuzdhj8LjrQPaeyCxr-3757176148%2Fuploads%2Fgit-blob-8dbfe542f9d7baef30f94c2dfff84731b0612f92%2Fdocker-compose.yml?alt=media)
	- 创建安装目录，将docker-compose.yml配置文件 复制到目录下，运行命令启动```
 ```
	  docker-compose up -d
  ```
- 直接使用docker部署
	- 安装docker
	- 运行命令

```
docker run -d --name appsmith -p 80:80 -v "$PWD/stacks:/appsmith-stacks" appsmith/appsmith-ce
```


- 参考文档：<https://docs.appsmith.com/setup/docker>

##### 2.2、其他方式部署

请参考官方文档：<https://docs.appsmith.com/setup>

  
##### 2.3、私有服务器使用


> 与云端版本一致，部署后直接注册登录即可使用。


#### 三、使用流程

  
##### 1、创建应用

> 使用的第一步就是创建一个应用，页面数据源等一系列操作都归属到应用上。直接在管理页面点击new按钮既可以新增应用。


##### 2、创建页面


> 默认会创建一个页面，之后可以在pages菜单右侧+号新增页面。

- pages可以设置多个页面的显示和隐藏，若有多个页面显示，则部署后的页面上方会有tab页面切换功能

- 单个页面点击右键或上一步骤中pages设置页面，可以设置主页面

  
##### 3、创建数据源
  
> Appsmith 可以使用 15+ 种 DB、REST API 或 GraphQL 作为数据源，同时支持 OAuth 2.0 等多种鉴权协议。通过配置数据源以及编写该数据源所对应的查询语句，可以做到对与底层数据的增、删、改、查工作。


##### 3.1、支持的数据类型


- databases

  - PostgreSQL

  - MongoDB

  - MySQL

  - Elasticsearch

  - DynamoDB

  - Redis

  - Microsoft SQL Server

  - Firestore

  - Redshift

  - S3

  - Snowflake

  - ArangoDB

  - SMTP

- APIs

  - 自定义API

  - curl import

  - 带验证的API

  - Google Sheets

  - Airtable

  
##### 3.2、配置数据源

- 配置数据的连接方式（读、写），host，port，database及用户名和密码等
- 本地数据库的host需要使用norgk代理后的域名
- 配置好数据源，可以选择自动生成页面，选择对应表及字段配置后生成简单增删改查页面

##### 4、设计页面

> Appsmith 使用现成的组件构建工作流，将表格、图表、表单等常见元素直接拖入应用程序，包括文本、表单、输入、按钮、表格、图像、复选框、开关、单选按钮、日期选择器、下拉列表、文件选择器、容器、地图、模式、富文本编辑器、选项卡和视频等。

  
- 拖放组件设计页面
- 组件可以自由拖动，放大或缩小，并可以通过页面设置样式、配置响应
- 支持多种设备尺寸
  - 自适应
  - 桌面设备
  - 平板（大）
  - 平板
  - 移动设备
  
##### 5、创建queries或js

> queries在数据源配置好之后，可以创建对应的数据库查询或者配置相应api接口；js，可以新增js对象封装js方法；queries和js都可以绑定到组件上


- queries可以是数据库语句，也可以是api接口，或者curl import等，提供数据的来源和交互
- queries生成语句时有对应的增删改查的模板
- js方法可以在组件的任意可配置的地方使用
- queries和js需要绑定到对应的page

##### 6、发布部署及分享

> 在操作平台可以点击deploy一键部署应用，部署后的页面需要设置分享为public，否则需要登录才可以查看。
  
- deploy操作可以配置git仓库，将现有版本的代码同步到仓库中去；但同步的内容不是页面的源码，是平台应用的配置信息
- 分享页面可以邀请其他注册用户，分享用户角色分为管理员、开发者、应用查看

#### 四、部分概念补充
  
##### 1、页面
> 页面是承载组件和展现数据的主体，最终展现的内容就是设计之后的页面

- 页面之间可以通过Navigate To方法跳转

  

##### 2、组件框架

> AppSmith提供了组件库，和全局的对象和方法

- 组件库类型
  - Button
  - Chart
  - Checkbox
  - Container
  - Datepicker
  - Select
  - Filepicker
  - Form
  - Image
  - Input
  - Maps
  - Modal
  - Radio Group
  - Rich Text Editor
  - Switch
  - Tabs
  - Table
  - Text
  - Video

- 框架提供的对象和方法
  - 对象
    - appsmith
    - Query
  - 方法
    - Show Modal
    - Close Modal
    - Download
    - Show Alert
    - Navigate To
    - Store Value
    - Reset Widget
    - Copy To Clipboard

- 页面组件事件可以绑定的内容
  - 运行query查询
  - 运行js方法
  - 页面跳转 Navigate To
  - 展示信息
  - 打开模态框 Show Modal
  - 关闭模态框 Close Modal
  - 存储信息 Store Value
  - 下载 Download
  - 复制到剪贴板 Copy To Clipboard
  - 重置组件 Reset Widget
  - 设置循环定时任务
  - 清理循环定时任务
  - 获取地理位置
  - 查看地理位置
  - 停止查看地理位置

  

#### 五、预研中遇到的部分问题

##### 1、云端或本地部署使用时，若需要连接本地数据库，需要使用norgk内网穿透工具代理本地地址

> 目前猜测本地部署模式是因为以docker的方式部署的，容器的内外网没有打通；另参照数据的连接情况，api接口应该也存在相应的问题；猜测可以通过以host模式启动docker容器解决，但目前host模式启动报错，还没有测试通过。

##### 2、部署后的页面都在平台内，有统一的头部

> 部署后的页面是运行在平台内的，无法单独部署；应用页面都有一个统一的平台的头，查询相关资料后，该问题可以在发布的连接后增加embed=true，去除掉公共的头部


#### 六、优点与缺点

##### 1、优点
  
- 操作简单，大部分内容可以自动生成
- 任何地方编写 JS 来自定义应用程序
- 可以是对接多种数据源
- 一键部署，单击即可部署发布

##### 2、缺点
  
- 部署发布的代码，无法生成源码，修改只能在appsmith的平台修改
- 网络访问问题，内网数据源，需要使用norgk等内网穿透工具

#### 七、自研框架与AppSmith差异比较
  
- 自研框架与AppSmith的差异

| <div style="width:60px;">功能模块</div>| <div style="width:60px;">开发事项</div>| <div style="width:90px;">自研框架</div>| AppSmith低代码设备平台 | 差异及优缺点                  |
| ------------------------------------- | :------------------------------------- | ------------------------------------------ | ---------------------------------------------------- | ------------------------------------------------------------ |
| 前台    | 页面布局  | html标签及css样式设置   | 组件的拖放和配置  | 1、组件的拖放和配置，操作的门槛会低一些，但自由度较低一些；<br/>2、开发的时间上，简单的页面AppSmith开发上这部分页面布局会快一些，复杂页面上时间差距不大 |
| 前台                                  | 页面样式调整                           | css样式                                    | 组件配置及js设置                                     | 这部分自研的框架上自由度更高一些，开发效率也高一些，但需要一定的开发经验 |
| 前后台                                | 业务逻辑                               | java代码和js代码                           | js代码                                               | 1、前台js的处理方式大致差不多，可以新建js方法处理；<br/>2、但自研框架本身已有后台，所以可以在后台处理较复杂的业务逻辑；<br/>3、低代码平台,则需要引入java后台接口 |
| 后台                                  | 数据获取                               | java代码                                   | 连接数据库，使用sql语句获取                          | 1、简单的单表增删改查，使用AppSmith的查询效率更快一点；<br/>2、复杂的数据处理，低代码平台也需要编写复杂的sql，而这种简单的单表操作，在日常业务中并不常见 |
| 部署                              | 发布部署                                   | 常规nginx和java部署方式                            | 平台内部部署内                                             | 1、需要先部署低代码平台，部署后平台内部应用是在平台内发布，直接点击按钮部署，操作比较简单；<br/>2、低代码平台部署发布，是集成在平台之内，不利于后期的维护及问题定位 |

- 总结
  > 1、低代码平台降低了开发的门槛，使用者不用太深入的学习编程，也可以操作使用；
  > 2、对于简单的内部应用处理上有一定的优势，但对于复杂业务逻辑的处理，需要依赖外部的服务提供；
  > 3、另外前端页面设计上，针对一些复杂的页面的处理以及统一样式调整等上，与自研框架上使用的时间差距不大，不太适合复杂的页面设计；
  > 4、低代码平台简化了部署流程，但是部署后的页面是没有源码的，也不是独立部署；对于后期应用的维护和定位问题增加了难度，另外后期提升性能的瓶颈也会受限于这种模式


#### 相关资源
官方地址：<https://www.appsmith.com/>
参考官方文档：<https://docs.appsmith.com/>

源代码仓库：<https://github.com/appsmithorg/appsmith>