
## 前言


大家好，这里是白泽，拖更了一段时间，抱歉。在 DouTok2\.0 可以初步允许大家接入开发之后，这篇文章才得以出炉。



> DouTok：一个开源的 web 端的短视频应用，采用微服务架构，包含前后端（React \& Go），DouTok 正处在开发初期，目前已经完成了 基础的用户注册、登录、用户信息管理、视频上传、视频列表展示、评论、点赞、收藏等功能。


![image-20241030091521273](https://baize-blog-images.oss-cn-shanghai.aliyuncs.com/img/image-20241030091521273.png)


为什么要有 V2 版本：


例如：**DouTok** 是字节跳动青训营的参赛作品，但 **DouTok1\.0** 版本的微服务划分不够合理，拆的过于零碎，也许看起来很“微服务”，但与实际工作生产环境上的服务划分却背道而驰，微服务的划分不应过分追求“微”，而是适应项目发展，在完善基本设计的前提下进行拆分。


让 **DouTok** 继续扩张的另一个卡点是其本身没有前端，只能依赖青训营中提供的“抖声”APP。为了让 **DouTok** 顺利扩张，所以我们决定开发一个全新的V2版本。在V2版本中，**DouTok** 减少了服务的划分，增加了**前端项目**，虽然现阶段依然不够完整，但是已经具备了继续扩张的土壤。


对参与过 **DouTok1\.0** 维护的所有同学表示感谢！


![image-20241029233433568](https://baize-blog-images.oss-cn-shanghai.aliyuncs.com/img/image-20241029233433568.png)


## 后续规划


* 前端：


	+ 功能：页面布局协调，以及事件跳转完善等
	+ 性能：React 组件优化与提炼等
* 后端：


	+ 功能：聊天系统（IM）、视频推荐、消息推送、私信等功能
	+ 性能：可观测性、压力测试，缓存 or 消息队列接入等


## 参与贡献


无论你是**前端开发者**还是**后端开发者**，都可以参与到 **DouTok** 的开发中来，我们欢迎你的加入！


🌟 **仓库地址**：[https://github.com/cloudzenith/DouTok](https://github.com)


🔥 如何参与贡献：[https://cloudzenith.github.io/DouTok/community](https://github.com)


🐧 QQ群: 622383022


📺 B站讲解：[白泽talk](https://github.com)


🔑 开源学习仓库：[go\-learning](https://github.com)


## 快速开始


本教程将带领你从零开始，循序渐进搭建并启动 `DouTok` 项目，若读者已具备相关知识，可选择性阅读。


所有信息参考文档站（非常详细）： [https://cloudzenith.github.io/DouTok/docs/quickstart/](https://github.com)


## 项目架构


![image-20241029222657307](https://baize-blog-images.oss-cn-shanghai.aliyuncs.com/img/image-20241029222657307.png)


## 主要目录


这是一个巨仓项目，所有的服务都在这个仓库中，目录结构如下：


* backend: 后端服务
* frontend: 前端服务
* test: 测试
* deploy: 部署
* docs\-site: 文档站
* env: 依赖环境部署
* sql: 数据库脚本


## 页面展示


* 上传视频


![image-20241030000520491](https://baize-blog-images.oss-cn-shanghai.aliyuncs.com/img/image-20241030000520491.png)


* 视频


![image-20241029235126428](https://baize-blog-images.oss-cn-shanghai.aliyuncs.com/img/image-20241029235126428.png)


* 评论 \& 点赞 \& 关注


![image-20241029235154405](https://baize-blog-images.oss-cn-shanghai.aliyuncs.com/img/image-20241029235154405.png)


## 环境准备


1. Golang 1\.22\+



> * [https://golang.org/dl/](https://github.com)
> 	* [https://golang.google.cn/dl/](https://github.com)
2. Node 14\.17\+



> * [https://nodejs.org/en/download/](https://github.com):[wgetCloud机场](https://tabijibiyori.org)
3. React.js \+ Next.js



> * [https://reactjs.org/](https://github.com)
> 	* [https://nextjscn.org/](https://github.com)
4. JetBrains GoLand/WebStorem



> * [https://www.jetbrains.com/](https://github.com)
5. VSCode



> * [https://code.visualstudio.com/](https://github.com)
6. Docker



> * [https://www.docker.com/products/docker\-desktop](https://github.com)


## 必要组件配置及启动


* Consul: 通过`backend/gopkgs/launcher`提供能力，所有后端服务均自动注册到Consul中
* Redis: 缓存
* MySQL: 持久化存储
* MinIO: 对象存储
* RocketMQ: 消息队列（不是必须）


1. 找到`env/basic.yml`文件，通过命令`docker-compose -f ./env/basic.yml up -d`启动Consul, Redis, MySQL, MinIO
（2、3步不是必须）
2. 找到`env/rocketmq/broker.conf`文件，将`brokerIP1`修改为本地局域网IP
3. 找到`env/rocketmq.yml`文件，通过命令`docker-compose -f ./env/rocketmq.yml up -d`启动RocketMQ


## MySQL库表结构同步


1. 进入`sql`目录
2. 检查`sql/Makefile`文件，其中涉及的MySQL连接需注意应与本地环境一致
3. 安装 goose 工具，执行`go install github.com/pressly/goose/v3/cmd/goose@latest`
4. 执行`make up`命令，MySQL库表结构会同步到本地


## 启动后端服务


### 编译运行


1. 进入`backend`目录下除`gopkgs`外的所有服务目录，依次 `go run cmd/main.go` 启动服务


### 镜像运行


1. 进入`backend`目录下除`gopkgs`外的所有服务目录，执行`make build`以编译Docker镜像
2. 进入`env`目录，检查`configs`下各个配置文件，应与本地环境保持一致，特别是 `./baseservice/config.yaml` 中，`minio.default.host` 需要改成本机局域网IP
3. 进入`env`目录，执行`docker-compose -f backend.yml up -d`启动所有后端服务


## 启动前端服务


1. 进入`frontend/doutok`目录，执行`pnpm install`安装依赖
2. 执行`pnpm dev`启动前端服务，通过 [http://localhost:23000](https://github.com) 访问


## 小结


持续更新中，欢迎关注。


