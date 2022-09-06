---
typora-root-url: image
typora-copy-images-to: image
---




# 谷粒商城基础篇

## 一、项目简介

### 1.1 项目背景

### 1.2 电商模式

市面上有 5 种常见的电商模式 B2B、B2B、C2B、C2C、O2O

#### 1.2.1 B2B 模式

B2B(Business to Business)，是指商家和商家建立的商业关系，如 阿里巴巴，

#### 1.2.2 B2C 模式

B2C(Business to Consumer) 就是我们经常看到的供应商直接把商品买个用户，即 “商对客” 模式，也就是我们呢说的商业零售，直接面向消费销售产品和服务，如苏宁易购，京东，天猫，小米商城

#### 1.2.3 C2B 模式

C2B (Customer to Business),即消费者对企业，先有消费者需求产生而后有企业生产，即先有消费者提出需求，后又生产企业按需求组织生产

#### 1.2.4 C2C 模式

 C2C (Customer to Consumer) 客户之间吧自己的东西放到网上去卖 如 淘宝 咸鱼

#### 1.2.5 O2O 模式

O 2 O 即 Online To Offline，也即将线下商务的机会与互联网结合在一起，让互联网成为线下交易前台，线上快速支付，线下优质服务，如：饿了吗，美团，淘票票，京东到家

### 1.3 项目架构图

1、项目微服务架构图

![](谷粒商城-微服务架构图.jpg)

2、微服务划分图

![image-20201015112852613](image-20201015112852613.png)

### 1.4 项目技术&特色*

是一个 B2C 模式的电商平台，销售自营商品给客户。  采用 Docker 容器化部署，基于 SpringCloud + SpringCloudAlibaba + MyBatis-Plus等技术实现。 包含应用监控、数据校验、压力测试、限流、降级、网关、注册发现、配置中心等分布式方案 。 项目前后分离开发，包括前台商城系统以及后台管理系统，前台商城系统包括：用户认证中心、商品服务、检索服务、库存服务、购物车服务、订单服务、秒杀服务等模块；后台管理系统主要配合renren-fast快速开发平台实现商品的增删改查、商品的上下架等功能。单点登录，消息队列

![image-20210706171141157](/image-20210706171141157.png)

微服务结构：
gulimall
├── gulimall-common -- 工具类及通用代码
├── gulimall-auth-server -- 认证中心（登录、注册、社交登录、OAuth2.0、单点登录）
├── gulimall-cart -- 购物车服务：添加购物项、操作购物车
├── gulimall-coupon -- 优惠卷服务
├── gulimall-gateway -- 统一配置网关
├── gulimall-order -- 订单服务：订单增删改查
├── gulimall-product -- 商品服务：商品的增删改查、商品的上下架、商品详情
├── gulimall-search -- 检索服务：商品的检索ES
├── gulimall-seckill -- 秒杀服务
├── gulimall-third-party -- 第三方服务
├── gulimall-ware -- 仓储服务 ：商品的库存
└── gulimall-member -- 用户服务：用户的个人中心、收货地址

├── renren-generator -- 人人开源项目的代码生成器（由相应的数据库生成mybatisplus代码）
├── renren-fast -- 配合renren-fast-vue前端生成登录验证码

## 二、分布式基础概念

### 2.1 微服务

微服务架构风格，就像是把一个单独的应用程序开发成一套小服务，每个小服务运行在自己的进程中，并使用轻量级机制**通信**，通常是 HTTP API 这些服务围绕业务能力来构建，	并通过完全自动化部署机制来独立部署，这些服务使用不同的编程语言书写，以及不同数据存储技术，并保持最低限度的集中式管理

**简而言之，拒绝大型单体应用，基于业务边界进行服务拆分，每个服务独立部署运行。**

### 2.2 集群&分布式&节点

集群是个物理状态，分布式是个工作方式

分布式是指讲不通的业务分布在不同的地方，集群实质是将几台服务器集中在一起，实现同一业务

例如：京东是一个分布式系统，众多业务运行在不同的机器上，所有业务构成一个大型的业务集群，每一个小的业务，比如用户系统，访问压力大的时候一台服务器是不够的，我们就应该将用户系统部署到多个服务器，也就是每个一业务系统也可以做集群化

分布式中的每一个节点，都可以做集群，而集群并不一定就是分布式的

节点：集群中的一个服务器

### 2.3 远程调用

在分布式系统中，各个服务可能处于不同主机，但是服务之间不可避免的需要互相调用，我们称之为远程调用

SpringCloud 中使用 HTTP+JSON 的方式来完成远程调用

<img src="image-20201015103427667.png" alt="image-20201015103427667" style="zoom:80%;" />

### 2.4 负载均衡算法*

<img src="image-20201015103547907.png" alt="image-20201015103547907" style="zoom:80%;" />

概念：分布式系统中，A  服务需要调用 B 服务，B 服务在多台机器中都存在， A 调用任意一个服务器均可完成功能。为了使每一个服务器都不要太或者太闲，我们可以负载均衡调用每一个服务器，提升网站的健壮性

负载均衡算法：

> **1、轮询法**
>
> 按请求顺序轮流地分配到每一台服务器，不关心服务器当前的系统负载。
>
> 2、随机法
>
> 根据后端服务器的列表大小值来随机选取其中的一台服务器进行访问。由概率可知，随请求数增多，实际效果越来越接近于轮询。
>
> **3、源地址哈希法**
>
> IP地址哈希值对服务器数量取模，得到要访问服务器的序号。同一用户IP地址请求每次映射到同一台后端服务器（后端服务器列表不变时）。
>
> **4、一致性哈希法**
>
> 服务器列表hash值围绕成一个环形，将用户ip的hash值对2^32取模，顺时针查找环上最近的服务器。源地址法容易受服务器数量影响，任意一台机器down掉后，请求落点全变了，缓存的位置也要发生改变，而一致性算法不会。
>
> **5、加权轮询法**
>
> 给配置高、负载低的机器配置更高的权重，将请求顺序且按照权重分配到后端。
>
> 6、加权随机法
>
> 给配置高、负载低的机器配置更高的权重，按照权重随机请求后端服务器，而非顺序。
>
> **7、最小连接数法**
>
> 它是根据后端服务器当前的连接情况，动态地选取连接数最少的一台服务器，在会话较长的情况下可以考虑采取这种方式

负载均衡类型：

DNS方式实现负载均衡（域名对应了ip）

硬件负载均衡：F5和A10

软件负载均衡：Nginx、HAproxy、LVS。区别:

​	 Nginx: 七层负载均衡，支持 HTTP、E-mail协议（应用层，解析URl）。同时也支持4层负载均衡（传输层，由ip直接转）

​	HAproxy：支持七层规则的，性能也很不错。OpenStack默认使用的负载均衡软件就是HAproxy;

​	LVS：运行在内核态，性能是软件负载均衡中最高的，严格来说工作在三层，所以更通用一些，适用各种应用服务。

### 2.5 服务注册/发现&注册中心

A 服务调用 B 服务， A 服务不知道 B 服务当前在哪几台服务器上有，**那些正常的，哪些服务已经下线，解决这个问题可以引入注册中心集中管理**

<img src="image-20201015104330935.png" alt="image-20201015104330935" style="zoom:80%;" />

如果某些服务下线，我们其他人可以实时的感知到其他服务的状态，从而避免调用不可用的服务

### 2.6 配置中心

<img src="image-20201015104450711.png" alt="image-20201015104450711" style="zoom:80%;" />

每一个服务最终都有大量配置，并且每个服务都可能部署在多个服务器上，我们经常需要变更配置，我们可以让每个服务在配置中心获取自己的位置。

配置中心用来**集中管理微服务的配置信息**

### 2.7 服务熔断&服务降级

在微服务架构中，微服务之间通过网络来进行通信，存在相互依赖，当其中一个服务不可用时，有可能会造成雪崩效应，要防止这种情况，必须要有容错机制来保护服务

<img src="image-20201015105256207.png" alt="image-20201015105256207" style="zoom: 67%;" />

1、服务熔断

降级的一种方式

- 设置服务的超时，当被调用的服务经常失败到达某个阈值，我们可以开启断路保护机制，后来的请求不再去调用这个服务，本地直接返回默认的数据

2、服务降级

一种思想

- 当系统处于高峰期，系统资源紧张，我们可以让非核心业务降级运行：某些服务不处理或延迟处理，或者简单处理【抛异常，返回NULL，调用 Mock数据，调用 FallBack 处理逻辑】

### 2.8 API 网关

在微服务架构中，API Gateway 作为整体架构的重要组件，他 抽象类服务中需要的公共功能，同时它提供了客户端**负载均衡**，**服务自动熔断**，**灰度发布**，**统一认证**，**限流监控**，**日志统计**等丰富功能，帮助我们解决很多 API 管理的难题

![image-20201015110148578](image-20201015110148578.png)	

### 2.9 RPC和RMI

RPC：远程过程调用，可以跨语言实现。httpClient类

RMI：远程方法调用，面向对象的思维方式。java版本的 RPC，跨JVM调用对象的方法

直接或间接实现接口 java.rmi.Remote成为存在于服务器端的远程对象，供客户端访问并提供一定的服务

远程对象必须继承 java.rmi.server.UniCastRemoteObject类，这样才能保证客户端访问获得远程对象时，该远程对象将会把自身的一个拷贝以Socket的形式传输给客户端，此时客户端所获得的这个拷贝称为"存根"，而服务器端本身已存在的远程对象则称之为"骨架”。其实此时的存根是客户端的一个代理，用于与服务器端的通信，而骨架也可认为是服务器端的一个代理，用于接收客户端的请求之后调用远程方法来响应客户端的请求

### 2.10 分库分表/分布式 id 生成方案

 1、uuid

**当前时间戳+计数器+ IEEE 机器识别号（网卡MAC地址或其他方式获得）**

优点：简单，保证唯一不重复

缺点：长度过长，占用空间大；ID是无序不是递增的，聚簇索引在数据插入时会造成大量的数据移动，产生大量的内存碎片，造成插入性能的下降

2、数据库自增 id

缺点：依赖DB，若是单机版，存在单点问题：**数据库宕机，**则业务不可用

分库分表要求：不同库不同的初始值，相同的步长，步长大于节点数，但此方法定义好步长之后，增加机器调整步长困难。或者采用一个专门用于生成主键的库，但每次都要操作数据库获取id。
**集群模式，主从同步延迟问题**

3、美团的 Leaf-segment

**每次获取一个id 区间号段**，区间段用完之后再去数据库获取新的号段，这样一来可以大大减轻数据库的压力

核心字段：biz_tag（业务），max_id（最大id），step（步长）

优点：扩张灵活；ID号码是趋势递增的，满足数据库存储和查询性能要求

缺点：可能存在多个节点同时请求ID区间的情况，依赖DB

双buffer方案：获取两个号段，有一个缓存号段备用，在当前号段消耗了10%的时候就去准备好下一个号段。

优点：减少了数据库查询，减少了网络依赖，效率更高。

缺点：号段长度是固定的，如果号段长度设置的过短，业务量大时分配的号段会一下用完，频繁更新号段，设置过长，缓存中有号段没有消耗完。

4、基于redis、mongodb、zk等中间件生成

**5、雪花算法**

生成一个**64位的整性数字**：第一位符号位固定为0，41位时间戳，10位workId，**12位序列号位数（递增）**

优点：每个亳秒值包含的ID值很多，不够可以变动位数来增加，整个ID是趋势递增的。

缺点：强依赖于机器时钟，如果时钟回拨，会导致重复的ID生成。

### 2.11 SpringCloud

![image-20210910162011400](/image-20210910162011400.png)

### 2.12 zookeeper

作为注册中心：Eureka 是ap，你中有我我中有你，主节点宕机仍可提供服务，因节点宕机而丢失的注册信息会在节点重启后恢复，会有同步延时。zookeeper是cp，主节点宕机就不可用，要选举。nacos 默认ap，使用CP模式还是AP模式主要是看 服务注册指定为临时，那么走AP模式，否则走CP模式。（raft算法选举）

#### 1、选举



cp：主从节点数据都同步了才成功，保证强一致性

#### 2、ZAB协议

包括两种模式：崩溃恢复和消息广播。当整个 Zookeeper 集群启动、宕机、所有服务器进入崩溃恢复模式，首先选举产生新的 Leader 服务器，然后集群中 Follower 服务器开始与新的 Leader 服务器进行数据同步。当集群中**超过半数机器**与该 Leader 服务器完成数据同步之后，退出恢复模式进入消息广播模式，Leader 服务器开始接收客户端的事务请求生成事物提案（超过半数同意）来进行事务请求处理。

### 2.13 kafka




## 三、环境搭建

### 3.1 安装 linux虚拟机

1）**下载VirtualBox**—win：  https://www.virtualbox.org/ 	 **直接安装**，要开启 CPU 虚拟化：

![image-20201015125024611](image-20201015125024611.png)

​		CPU 查看虚拟化，内核和处理器数量：

<img src="image-20201015125053510.png" alt="image-20201015125053510" style="zoom:80%;" />

2） 普通安装linux虚拟机太麻烦，可以利用vagrant可以帮助我们快速地创建一个虚拟机，Vagrant下载win：https://www.vagrantup.com/downloads.html，

 **直接安装，测试，cmd输入：vagrant**   ， Vagrant 官方镜像仓库：	https://app.vagrantup.com/boxes/search

3）初始化下载centos7，**打开window cmd窗口，运行,加载中科大镜像快一点**

```
vagrant init centos7 https://mirrors.ustc.edu.cn/centos-cloud/centos/7/vagrant/x86_64/images/CentOS-7.box
```

**运行**

```
vagrant up
```

若报错：encode': "\x8D" followed by " " on GBK (Encoding，

改vagrant的安装包文件，\vagrant\embedded\gems\2.2.5\gems\vagrant-2.2.5\lib\vagrant\util\io.rb的

```
#data << io.readpartial(READ_CHUNK_SIZE).encode("UTF-8", Encoding.default_external)			//注释掉这行
data << io.readpartial(READ_CHUNK_SIZE).encode('UTF-8', invalid: :replace, undef: :replace, replace: '?')  //加上
```

卸载centos7的命令：

```
vagrant box list			vagrant box remove centos7       
```

4)ssh和网络连接

默认虚拟机的ip 地址不是固定ip 开发不方便，修改C盘用户文件夹下的： Vagrantfile，打开注释修改，修改完重启

```
config.vm.network "private_network", ip: "192.168.56.56"   //主机下cmd使用 ipconfig,找到属于virtualBox的网络，第三位相同
```

可将主机ipv4改为固定

<img src="/image-20210702143945596.png" alt="image-20210702143945596" style="zoom:80%;" />

（若是还连不上网执行如下操作：

```
cd /etc/sysconfig/network-scripts
ls 			#一般有ifcfg-eth+数字
ip addr   查看哪个inet是之前改的网址
vi ifcfg-eth1
不是VM，不用修改这两个了：           BOOTPROTO="static"
            					  	ONBOOT="yes"
IPADDR=192.168.56.56
NETMASK=255.255.255.0
GATEWAY=192.168.56.2
DNS1=114.114.114.114
DNS2=8.8.8.8
```

重启网络：service network restart	

如果主机同时装了visualbox和vm，也会导致其中一个虚拟机ping不通主机，因此使用vm或vb时，在主机上禁用另一个虚拟网卡即可。 ）

修改完重启：

```
vagrant reload
```

cmd里：ip addr	看虚拟机的地址，再互相ping通主机和虚拟机。

默认只允许ssh登录方式，为了后来操作方便，文件上传等，可以配置账号密码其他工具连接：

```sh
vagrant ssh    #cmd使用 vagrant 用户连接虚拟机：
转到root：su root  密码：vagrant
vim /etc/ssh/sshd_config		修改	PasswordAuthentication yes
重启,用其他工具连接
systemctl restart sshd.service 
账号root
密码vagrant
```

yum install net-tools	可以安装netstat -ntlp/ifconfig等命令

systemctl stop/disable firewalld，关闭防火墙并设置开机关闭，以后软件就不用开端口了 

### 3.2 安装 docker

Docker 安装文档参考官网：https://docs.docker.com/engine/install/centos/

#### 3.2.1 卸载系统之前安装的 docker

Uninstall old versions

```bash
1） yum -y install gcc
2）sudo yum remove docker \                            #卸载
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine                  
 3）yum install -y yum-utils device-mapper-persistent-data lvm2      #安装需要的软件包
```

设置stable镜像仓库

```bash
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

INSTALL DOCKER ENGINE

1. Install the *latest version* of Docker Engine and containerd, or go to the next step to install a specific version:

```bash
yum makecache fast		#生成缓存索引
sudo yum install docker-ce docker-ce-cli containerd.io
```

Start Docker

```bash
sudo systemctl start docker
docker -v      #测试
```

设置docker开机自启动

```bash
sudo systemctl enable docker 
```

设置 Docker 镜像加速：登录 aliyun.com 在控制台找到容器镜像服务：执行下面命令：建配置文件，将配置写入文件，重载，重启

<img src="image-20201015144524435.png" alt="image-20201015144524435" style="zoom:80%;" />

后面要改镜像的话：vi /etc/docker/daemon.json          中科院的镜像：https://docker.mirrors.ustc.edu.cn      阿里：  https://60ax955x.mirror.aliyuncs.com                                   

改后重启：sudo systemctl restart docker

### 3.3 docker 安装 mysql

#### 3.3.1 下载镜像文件

```bash
docker pull mysql:5.7
docker images  (docker要先start)
```

#### 3.3.2 创建实例并启动

```bash
# --name指定容器名字 -v目录挂载 -p指定端口映射  -e设置mysql参数 -d后台运行
sudo docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7
####
-v 将对应文件挂载到主机
-e 初始化对应
-p 容器端口映射到主机的端口
后台建容器还没进入，进入开启的容器：docker exec -it mysql /bin/bash
```

MySQL 配置

vi /mydata/mysql/conf/my.cnf 创建&修改该文件

```bash
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```

```
exit
docker ps
docker restart mysql
docker exec -it mysql /bin/bash
或者设置自启：docker update mysql --restart=always
```

**密码用户名都是root**

#### 3.3.3  mysql 命令行工具连接

![image-20201015151247976](image-20201015151247976.png)



### 3.4 docker 安装 redis

#### 3.4.1 下载镜像文件

docker pull redis

#### 3.4.2 创建实例并启动

```bash
mkdir -p /mydata/redis/conf
touch /mydata/redis/conf/redis.conf

# 启动 同时 映射到对应文件夹
# 后面 \ 代表换行
docker run -p 6379:6379 --name redis \
-v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf

设置自启：docker update redis --restart=always
```

#### 3.4.3 执行 redis-cli 命令连接

此时redis容器已经开启，以客户端进入：docker exec -it redis redis-cli           set a b     get  a

持久化 默认 appendonly on 没有开启

```bash
vim /mydata/redis/conf/redis.conf
# 插入下面内容
appendonly yes

重启：docker restart redis
```

### 3.5 开发环境配置***

#### 3.5.1 Maven

config/setting里配置阿里云镜像

```xml
<mirrors>
		<mirror>
		<id>nexus-aliyun</id>
		<mirrorOf>central</mirrorOf>
		<name>Nexus aliyun</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public</url>
		</mirror>
</mirrors>
	
```

配置 jdk 1.8 编译项目

```xml
<profiles>
		<profile>
			<id>jdk-1.8</id>
			<activation>
				<activeByDefault>true</activeByDefault>
				<jdk>1.8</jdk>
			</activation>
			<properties>
				<maven.compiler.source>1.8</maven.compiler.source>
				<maven.compiler.target>1.8</maven.compiler.target>
				<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
			</properties>
		</profile>
	</profiles>
```

#### 3.5.2 idea

设置软件启动时选择项目进入:

<img src="/image-20210616165511257.png" alt="image-20210616165511257" style="zoom:67%;" />

键位：ctrl+H->ctrl+shift+f  搜索全局，ctrl+alt+L格式化（不用改）

<img src="/image-20210616165518569.png" alt="image-20210616165518569" style="zoom:67%;" />

  Tag设置：

​										<img src="/image-20220214175151204.png" alt="image-20220214175151204" style="zoom:67%;" />

字符编码：

<img src="/image-20210616165601893.png" alt="image-20210616165601893" style="zoom:67%;" />

 fileType过滤：

<img src="/image-20210616170028461.png" alt="image-20210616170028461" style="zoom:67%;" />

 插件：

<img src="/image-20210616165621535.png" alt="image-20210616165621535" style="zoom:67%;" />

安装本地插件：

<img src="/image-20220519120317306.png" alt="image-20220519120317306" style="zoom: 67%;" />

 versioncontrol

<img src="/image-20210616165630542.png" alt="image-20210616165630542" style="zoom:67%;" />

maven:

<img src="/image-20210616165706226.png" alt="image-20210616165706226" style="zoom:80%;" />

注解生效;

<img src="/image-20210616165712538.png" alt="image-20210616165712538" style="zoom:67%;" />

 java编译版本选8：java compiler

取消IDEA打开默认项目配置：Appearance & Behavior>System Settings

<img src="/image-20211216203520377.png" alt="image-20211216203520377" style="zoom:80%;" />

取消自动更新：

<img src="/image-20211216203230167.png" alt="image-20211216203230167" style="zoom: 67%;" />

目录展示：

<img src="/image-20211221175307149.png" alt="image-20211221175307149" style="zoom:67%;" />

去除所有未引用的包（即灰色的import）；方法二：快捷键Ctrl + Alt + O;

<img src="/image-20220106113130959.png" alt="image-20220106113130959" style="zoom: 67%;" />

 再设置一遍：

​														 <img src="/image-20210616165732441.png" alt="image-20210616165732441" style="zoom:67%;" />



#### 3.5.2 VsCode

Vscode 安装开发必备插件

Vetur ——语法高亮，智能感知 包含格式化功能 	（VScode中Alt+Shift+F 格式化全文，浏览器F5重加载）
EsLint 一一 语法纠错	        		Live Server 解决跨越问题                              open in brower

Auto Close Tag —— 自动闭合HTML/XML标签				Auto Rename Tag —— 自动完成另一侧标签的同步修改

HTML CSS Support —— 让 html 标签上写class 智能提示当前项目所支持的样式
JavaScript(ES6) code snippets 一一 ES6 语法智能提示以及快速输入

Vue 3 Snippets 语法提示			HTML Snippets			谷歌浏览器中安装插件Vue.js  devtools

#### 3.5.4 安装配置 git**

1、下载git   

https://git-scm.com/

2、配置git ，进入 git bash

```bash
# 配置用户名
git config --global user.name "user.name"
# 配置邮箱
git config --global user.email "username@email.com" # 注册账号使用的邮箱
```

3、配置 ssh 免密登录

https://github.com/settings/keys

```bash
git bash 使用 ssh-keygen -t rsa -C "XXX@xxx.com" 命令 连续三次回车
一般用户目录下都会有
id_rsa 文件
id_rsa.pub 文件
或者 cat ~/.ssh/id_rsa.pub
登录进 github/gitee 设置 SSH KEY
使用 ssh -T git@gitee.com 测试是否成功
```

<img src="/image-20210601141509002.png" alt="image-20210601141509002" style="zoom:80%;" />



<img src="/image-20220228123907597.png" alt="image-20220228123907597" style="zoom: 67%;" />

**git clone=》git先更新-》add-》commit-》push**

改着同一个地方要及时更新，在别人基础上改，否则提交会报冲突**（要及时更新，及时提交）**

git clone 网址 ； 克隆在主支上代码，可以登录就有权限下载

git pull  （origin develop(远程分支名称)）	拉取更新代码，pull可改为fetch		或者使用工具

git add . 	提交所有变化（包括删除、新增、修改） 

git commit -m "注释"  	本地仓库提交

git push (-u origin master) 	推送



本地提交错了，可以在idea中undo commit

更新了别人（删除了文件）的代码会报错，重新build编译一下



git pull 后报错，自己改的和别人的冲突了。

先 git stash 备份当前的工作区的内容，让工作区保证和上次提交的内容一致。再git pull 拉取
再git stash pop git 恢复自己工作区的内容备份  或者直接 git stash clear 清空工作区





### 3.6 工程创建*

1、alibaba，cloud，boot版本：(先调整boot版本为2.3.2，后面common模块写cloud和alibaba版本号)

https://github.com/alibaba/spring-cloud-alibaba/wiki/%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E

<img src="/image-20210601153531497.png" alt="image-20210601153531497" style="zoom:80%;" />

![image-20210601153617177](/image-20210601153617177.png)

<img src="/image-20210601154115804.png" alt="image-20210601154115804" style="zoom:80%;" />

共同点：

1. 依赖 spring web、openfeign

2. 服务包名：com.example.gulimall.xxx(product/order/ware/coupon/member)  

3. 模块名：
   gulimall
   ├── gulimall-common -- 工具类及通用代码
   ├── renren-generator -- 人人开源项目的代码生成器
   ├── gulimall-auth-server -- 认证中心（登录、注册、社交登录、OAuth2.0、单点登录）
   ├── gulimall-cart -- 购物车服务：添加购物项、操作购物车
   ├── gulimall-coupon -- 优惠卷服务
   ├── gulimall-gateway -- 统一配置网关
   ├── gulimall-order -- 订单服务：订单增删改查
   ├── gulimall-product -- 商品服务：商品的增删改查、商品的上下架、商品详情
   ├── gulimall-search -- 检索服务：商品的检索ES
   ├── gulimall-seckill -- 秒杀服务
   ├── gulimall-third-party -- 第三方服务
   ├── gulimall-ware -- 仓储服务 ：商品的库存
   └── gulimall-member -- 用户服务：用户的个人中心、收货地址
   
   **创建工程其他的方式：**
   
   （1）用springboot：这样建的工程要在父类工程里加module
   
   <img src="/image-20211026170023947.png" alt="image-20211026170023947" style="zoom:80%;" />
   
   （2）用maven：要在自己模块下建包和主启动类。
   
   父工程创建完成执行mvn:install将父工程发布到仓库方便子工程继承
   
   pom文件加上：<packaging>pom</packaging> ，  pom表示父工程，这样子确定版本号 必须是本地仓库有的
   
   <img src="/image-20211026170730201.png" alt="image-20211026170730201" style="zoom:80%;" />
   
   
   
   （3）复制的：
   
   <img src="/../../分布式高级篇/image/image-20210704144044884.png" alt="image-20210704144044884" style="zoom:80%;" />
   
   （4）导入的：
   
   导入gitee中的代码：
   
   ![image-20210601144219204](/image-20210601144219204.png)
   
   ![image-20210601144407307](/image-20210601144407307.png)
   
   ![image-20210601144335318](/image-20210601144335318.png)

父工程：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example.gulimall</groupId>
    <artifactId>gulimall</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>gulimall</name>
    <description>聚合服务</description>
    <packaging>pom</packaging>
<modules>
    <module>gulimall-ware</module>
    <module>gulimall-coupon</module>
    <module>gulimall-product</module>
    <module>gulimall-order</module>
    <module>gulimall-member</module>
    <module>renren-fast</module>
    <module>renren-generator</module>
    <module>gulimall-common</module>
    <module>gulimall-gateway</module>
    <module>gulimall-third-party</module>
</modules>
<!--  版本锁定,声明依赖，并不实现引入，子模块不用再写 version ,按需加载。如果子项目中指定了版本号，会使用子项目中的版本-->
    <properties>
        <java.version>1.8</java.version>
        <spring.boot.version>2.3.2.RELEASE</spring.boot.version>
        <spring-cloud.version>Hoxton.SR8</spring-cloud.version>
    </properties>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.3.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
                <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.2.5.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
                </dependency>
        </dependencies>
    </dependencyManagement>
<!--      这里的依赖会被子模块直接继承-->
    <dependencies>
        <!--     test放common里子模块传递不到，要放父工程里-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.3.2.RELEASE</version>
            </plugin>
        </plugins>
    </build>
</project>
```

重构工程：为了方便可以将公共依赖放入common中，亦可以由微服务自治原则放入每个微服务中

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
<parent>
    <artifactId>gulimall</artifactId>
    <groupId>com.example.gulimall</groupId>
    <version>0.0.1-SNAPSHOT</version>
</parent>
<modelVersion>4.0.0</modelVersion>

<artifactId>gulimall-common</artifactId>
<description>每一个微服务的公共的依赖，bean，工具类等</description>
<dependencies>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.2.0</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.8</version>
    </dependency>
    <!-- httpcomponent包。发送http请求 -->
    <dependency>
        <groupId>org.apache.httpcomponents</groupId>
        <artifactId>httpcore</artifactId>
        <version>4.4.13</version>
    </dependency>
    <!--    从renren-fast中导入-->
    <dependency>
        <groupId>commons-lang</groupId>
        <artifactId>commons-lang</artifactId>
        <version>2.6</version>
    </dependency>
    <!--导入mysql驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.19</version>
    </dependency>
    <!--tomcat里一般都带,打包不带-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-openfeign</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
    <!--配置中心来做配置管理-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
    </dependency>
<!--    错误校验-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
</dependencies>
</project>
```

其中微服务：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.example.gulimall</groupId>
        <artifactId>gulimall</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>

    <artifactId>gulimall-product</artifactId>
    <description>谷粒商城-商品服务</description>
    <dependencies>
        <dependency>
            <groupId>com.example.gulimall</groupId>
            <artifactId>gulimall-common</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>
<!--    把依赖的包以及本工程代码都打进一个可执行的jar/war包。-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

2、复制一个pom放外面聚合 

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example.gulimall</groupId>
    <artifactId>gulimall</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>gulimall</name>
    <description>聚合服务</description>
    <packaging>pom</packaging>
<modules>
    <module>gulimall-ware</module>
    <module>gulimall-coupon</module>
    <module>gulimall-product</module>
    <module>gulimall-order</module>
    <module>gulimall-member</module>
</modules>
</project>
```

<img src="/image-20210601170019818.png" alt="image-20210601170019818" style="zoom:80%;" />

```
**/mvnw              #ignore里忽略的文件
**/mvnw.cmd
**/.mvn
**/target/
.idea
**/.gitignore
```

![image-20210601170505595](/image-20210601170505595.png)



3、**安装：PowerDesigner165  ，用户名中文问题：改环境变量temp和tmp值，安装好后改回来**

<img src="/image-20211026172414188.png" alt="image-20211026172414188" style="zoom: 67%;" />

数据库数据不建立外键，在电商系统里数据量大，做外键关联很耗性能。建表name是给我们看的，code才是数据库里真正的信息。

<img src="/image-20210601183605075.png" alt="image-20210601183605075" style="zoom:80%;" />

4、到码云下载人人开源后台管理系统脚手架工程

<img src="/image-20210601191658922.png" alt="image-20210601191658922" style="zoom:80%;" />

![image-20210601223827248](/image-20210601223827248.png)



5、将拷贝下来的“renren-fast”删除“.git”后，拷贝到“gulimall”工程根目录下，然后将它作为gulimall的一个module

有sql语句建表，建表时字符集选UTF8mb4

<img src="/image-20210601193818278.png" alt="image-20210601193818278" style="zoom:80%;" />

![image-20210601202533076](/image-20210601202533076.png)

<img src="/image-20210601195806946.png" alt="image-20210601195806946" style="zoom:80%;" />

连接的是gulimall_admin数据库，运行测试：启动，然后访问“http://localhost:8080/renren-fast/”，

端口占用问题，netstat -ano|findstr "8080" 看8080端口被占用，任务管理器关闭pid

#### vscode设置

用VSCode打开renren-fast-vue（如果自己搭建的话），安装node，百度搜nodejs，安装mostuser（node14），安装好环境变量自动配好

​	NPM是随同NodeJS一起安装的包管理工具。JavaScript-NPM类似于java-Maven。

​	cmd命令行输入node -v 检查配置，再配置npm的镜像仓库地址

```
查看版本命令使用  node -v和  npm -v.
npm config set registry http://registry.npm.taobao.org/ 	 ，公司内部有组件配置内部的
```

然后去VScode的项目终端中输入 命令，去拉取依赖（package.json类似于pom.xml的dependency）

<img src="/image-20210601213839329.png" alt="image-20210601213839329" style="zoom:80%;" />
主要是node-sass和node版本对应问题：https://github.com/sass/node-sass

![image-20210601214739198](/image-20210601214739198.png)

若安装过报错：先删除清理node_modules：npm rebuild node-sass			 npm uninstall node-sass

**npm install  node-sass@5.0		# 指定node-sass版本，这句等价于修改package.json文件了**
**npm install									#安装其他依赖：**
**npm run dev								# 启动项目：**
登录验证admin，admin，如果没验证码什么的，是因为java项目没有启动



使用angular脚手架：版本太高又删不掉就重新换文件

```apache
配置公司内部组件仓库：npm config set。。。          查看配置过的源：npm config get registry

有ng卸载之前的ng版本：npm uninstall -g @angular/cli     清缓存：npm cache verify
安装指定版本的angular/cli：npm install -g @angular/cli@12.2.0     查看版本：ng v
提示禁用ng脚本： 百度
前端node_modules包不全，用： npm install
运行angular项目：ng serve --open

更新了组件，删除node_modules,重新npm install
```



#### idea操作

右边没有maven，根据右下角提示：add as maven。或者右击pom文件，选择“add as maven project”

新导入的文件，**将maven中的root的clean，和install一下**，没依赖配mean仓库并刷新





## 四、 逆向工程使用

1、导入项目逆向工程

 git  clone    renren-generator的地址

克隆到桌面后，同样把里面的.git文件删除，然后移动到我们IDEA项目目录中，同样配置好pom.xml

  父工程：<module>renren-generator</module>

yaml:

```
url: jdbc:mysql://192.168.245.130:3306/gulimall_pms?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai
username: root
password: root            //若时数字要加“”
```

然后修改generator.properties（这里乱码的百度IDEA设置properties编码）

```
mainPath=com.example
#\u5305\u540D
package=com.example.gulimall
moduleName=product
#\u4F5C\u8005
author=henan
#Email
email=henan@gmail.com
#\u8868\u524D\u7F00(\u7C7B\u540D\u4E0D\u4F1A\u5305\u542B\u8868\u524D\u7F00)
tablePrefix=pms_
```

先将resources下的template的controller模板的@RequiresPermissions注释掉

开启renren-generator（不用vue）逆向生成main文件和sql，将生成的main粘贴到gulimall—pro下

![image-20210602140620722](/image-20210602140620722.png)

2、maven建gulimall-common，父工程的module自动添加了

pom：<description>每一个微服务的公共的依赖，bean，工具类等</description>

### 4.1 整合mybatisplus

#### 4.1.1、重构gulimall-common

```xml
 <description>每一个微服务的公共的依赖，bean，工具类等</description>
<!-- 整合mybatisplus -->
<dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.2.0</version>
 </dependency>
<!--导入mysql驱动，从maven仓库：https://mvnrepository.com，5.7没有对应依赖，8向下兼容-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.19</version>
        </dependency>
<dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.8</version>
    </dependency>
 <!-- httpcomponent包。发送http请求 -->
    <dependency>
        <groupId>org.apache.httpcomponents</groupId>
        <artifactId>httpcore</artifactId>
        <version>4.4.13</version>
    </dependency>
<!--    从renren-fast中导入-->
    <dependency>
        <groupId>commons-lang</groupId>
        <artifactId>commons-lang</artifactId>
        <version>2.6</version>
    </dependency>
<!--tomcat里一般都带,打包不带-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

根据gulimall-product中缺少的东西，导入依赖，根据product中报错的在common中添加类（从renren-fast中），然后看comment类中本身有的异常	

![image-20210602182805749](/image-20210602182805749.png)

#### 4.2.2、product中yml配置

```yml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://192.168.56.10:3306/gulimall_pms
    driver-class-name: com.mysql.jdbc.Driver
mybatis-plus:
	# mapper文件扫描
  mapper-locations: classpath:/mapper/**/*.xml
  global-config:
    db-config:
      id-type: auto # 数据库主键自增
```

classpath：只会到你的class路径中查找找文件;
classpath*：不仅包含class路径，还包括jar文件中(class路径)进行查找

**/* 是拦截所有的文件夹，不包含子文件夹，/** 是拦截所有的文件夹及里面的子文件夹

配置MyBatis-Plus包扫描：（dao包上有@Mapper的话可写可不写）

1. 主程序类中使用@MapperScanner，告诉MyBatis-Plus,Sql映射文件位置

   ```java
   @MapperScan("com.atguigu.gulimall.product.dao")
   @SpringBootApplication
   public class GulimallProductApplication {。。。}
   ```


test类中测试：报错的话改cloud版本**Hoxton.SR8**

同理逆向生成coupon模块，先改generator.properties,再改yml，启动生成

![](/image-20210602182057600.png)



## 五、SpringCloud Alibaba

### 5.1 SpringCloud Alibaba 简介

#### 5.1.1、简介

Spring Cloud Alibaba 致力于提供微服务开发的**一站式解决方案**。此项目包含开发分布式应用微服务的必需组件，方便开发者通过 Spring Cloud 编程模型轻松使用这些组件来开发分布式应用服务。

#### 5.1.2、为什么要使用 

eureka和hystrix停止维护了

**SpringCloud的几大痛点：**

SpringCloud部分组件停止维护和更新，给开发带来不便;

SpringCloud部分环境搭建复杂，没有完善的可视化界面，我们需要大量的二次开发和定制

SpringCloud配置复杂，难以上手，部分配置差别难以区分和合理应用

**SpringCloud Alibaba的优势：**

阿里使用过的组件经历了考验，性能强悍，设计合理，现在开源出来大家用

**结合SpringCloud Alibaba我们最终的技术搭配方案：**

**注册中心**：SpringCloud Alibaba Nacos

**配置中心**：SpringCloud Alibaba Nacos

声明式HTTP客户端：SpringCloud Feign ——调用远程服务，feign中已经整合SpringCloud Ribbon --负载均衡

**服务容错**：SpringCloud Alibaba Sentinel ——限流、降级、熔断

API网关：SpringCloud Gateway ——webflux 编程模式

调用链路监控：SpringCloud Sleuth

**分布式事务**：SpringCloud Alibaba Seata ——原Fescar

上传图像：SpringCloud Alibaba oss

### 5.2 Nacos [注册中心]

Nacos 是阿里巴巴开源的一个更易于构建云原生应用的动态服务发现，配置管理和服务管理平台，他是使用 java 编写的，需要依赖 java 环境。

Nacos 文档地址： https://nacos.io/zh-cn/docs/quick-start.html

#### 5.2.1、下载 nacos-server

https://github.com/alibaba/nacos/releases

#### 5.2.2、启动 nacos-server

- cmd 运行startup.cmd 文件
- 访问localhost:8848/nacos/
- 使用默认的 nacos/nacos 登录

#### 5.2.3、注册进入 nacos 中

1、首先，修改 pom.xml 文件，引入  Nacos Discovery Starter

```xml
 <dependency>
     <groupId>com.alibaba.cloud</groupId>
     <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
 </dependency>
```

2、在应用的 /resource /application.properties 中配置 Nacos Server地址,每一个应用都应该有名字，这样才能往册上去。

```PROPER
spring.cloud.nacos.discovery.server-addr=127.0.0.1:8848
spring.application.name= service provider
server.port=8000
```

3、使用@EnableDiscoveryClient 开启服务注册发现功能

```java
 @SpringBootApplication
 @EnableDiscoveryClient
 public class ProviderApplication {
```

4、通过cmd进入到nacos文件夹里面bin目录，然后nacos以单机模式运行输入命令：startup.cmd -m standalone 

测试地址：http://localhost:8848/nacos

#### 5.2.4、Nacos 使用三步

1、导包

2、写配置，指定 nacos 地址，指定应用的名字

3、开启服务注册发现功能 @EnableDiscoveryClient

### 5.3 Nacos [配置中心]

#### 5.3.1、pom.xml 引入依赖

```xml
  <!--配置中心来做配置管理-->
<dependency>
   <groupId>com.alibaba.cloud</groupId>
   <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

#### 5.3.2、 bootstrap.properties

```properties
spring.application.name=gulimall-coupon
spring.cloud.nacos.config.server-addr=127.0.0.1:8848
```

#### 5.3.3、在 nacos 中添加配置

选择右上角添加配置（以前配置在application.properties里，不方便）

<img src="/1615009802388-ca380a81-1941-459a-9aa6-78e7db16d7f4.png" alt="img" style="zoom:50%;" />

#### 5.3.4、@Value 和 @RefreshScope

```java
@RefreshScope // 刷新对应controller
@RestController
@RequestMapping("coupon/coupon")
public class CouponController {
    @Value("${coupon.user.name}")//从application.properties中获取,不要写user.name(系统变量)
    private String name;
    @Value("${coupon.user.age}")//${配置项的名}，获取配置里的变量
    private Integer age;
    @RequestMapping("/test")
    public R test(){
        return R.ok().put("name",name).put("age",age);}
```

请求 `/coupon/coupon/test` 可以看到配置中心的数据，修改配置中心的配置，刷新可自动更新

nacos的配置中心的内容优先于项目本地的配置内容。

#### 5.3.5、进阶

> 1、核心概念

**命名空间：**namespace 用作配置隔离

- 默认public。默认新增的配置都在public空间下
- 常用做不同环境的配置的区分隔离，例如开发测试环境和生产环境的资源（如配置、服务）隔离等。也可以为每个微服务配置一个命名空间，微服务互相隔离（一般每个微服务一个命名空间）
- 在bootstrap.properties里配置（测试完去掉，学习不需要），不在里面指定就用默认的public和默认的分组（没有默认的且没有指定就用本地配置的.properties文件，不找没配置的dev）

**配置集：   **一组相关或者不相关的配置项的集合称为配置集。在系统中，一个配置文件通常就是一个配置集。

**配置集ID:**	类似于配置文件名，**即Data ID**

**配置分组：**默认所有的配置集都属于`DEFAULT_GROUP`。双十一，618的优惠策略改分组即可

**最终方案：每个微服务创建自己的命名空间，然后使用配置分组区分环境（dev/test/prod）**

**加载多配置集**:

我们要把原来application.yml里的内容都分文件抽离出去,在nacos里创建好后,在.extension-configs[]写明每个配置集

**bootstrap.properties 配置**（**注意：文件里不要在值后面写注释**）application.yml /application.properties

```properties
spring.application.name=gulimall-coupon # 服务的名称
spring.cloud.nacos.config.server-addr=127.0.0.1:8848  # 服务注册地址
spring.cloud.nacos.config.namespace=ae34901c-9215-4409-ae61-c6b8d6c8f9b0  # 命名空间地址
#spring.cloud.nacos.config.group=111 # 对应分组
#配置分组，新版本不建议用下面的了，低版本可能也不能用yml，要用yaml
#spring.cloud.nacos.config.ext-config[0].data-id=datasource.yml  
#spring.cloud.nacos.config.ext-config[0].group=dev              
#spring.cloud.nacos.config.ext-config[0].refresh=true          

spring.cloud.nacos.config.extension-configs[0].data-id=datasource.yml   # 配置集指定data_id
spring.cloud.nacos.config.extension-configs[0].group=dev				 # 配置集指定 group 分组
spring.cloud.nacos.config.extension-configs[0].refresh=true	  # 是否动态刷新 在配置中心修改后 微服务自动刷新
spring.cloud.nacos.config.extension-configs[1].data-id=mybatis.yml
spring.cloud.nacos.config.extension-configs[1].group=dev
spring.cloud.nacos.config.extension-configs[1].refresh=true
spring.cloud.nacos.config.extension-configs[2].data-id=other.yml
spring.cloud.nacos.config.extension-configs[2].group=dev
spring.cloud.nacos.config.extension-configs[2].refresh=true
```

#### 5.3.6、nacos配置持久化

Nacos默认自带的是嵌入式数据库derby

derby到mysql切换步骤：linux下，下载nacos

1)、找到nacos\conf\nacos-mysql.sql 的sql脚本，加上如下并执行

```
CREATE DATABASE nacos_config;
USE nacos_config;
```

2）、配置：nacos\conf目录下找到application.properties

```
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456
```

8以上的mysql，在nacos目录下建plugins/mysql，将对应的8版本jar包入

Linux服务器上nacos的集群地址：配置 nacos/conf/cluster.conf

其他参照spring-cloud笔记配置。

### 5.4 SpringCloud Alibaba-Sentinel

#### 1、简介

##### 1、熔断降级限流

**服务熔断：降级的一种方式**

- 设置服务的超时，当被调用的服务经常失败到达某个阈值，我们可以开启断路保护机制，后来的请求不再去调用这个服务，本地直接返回默认的数据

**服务降级：一种思想**

- 当系统处于高峰期，系统资源紧张，我们可以让非核心业务降级运行：某些服务不处理或延迟处理，或者简单处理【抛异常，返回NULL，调用 Mock数据，调用 FallBack 处理逻辑】

​	相同点：**保证集群大部分服务的可用性**和可靠性，**防止崩溃，牺牲小我**	

**什么是限流**：

​	对打入的服务的请求流量进行控制，使服务能够承担不超过自己能力的流量压力

##### 2、Sentinel 简介

官方文档：https://github.com/alibaba/Sentinel/wiki/%E4%BB%8B%E7%BB%8D

随着微服务的流行，服务和服务之间的稳定性变得越来越重要。Sentinel 以流量为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。

**Sentinel 具有以下特征:**

- **丰富的应用场景**：Sentinel 承接了阿里巴巴近 10 年的双十一大促流量的核心场景，例如秒杀（即突发流量控制在系统容量可以承受的范围）、消息削峰填谷、集群流量控制、实时熔断下游不可用应用等。
- **完备的实时监控**：Sentinel 同时提供实时的监控功能。您可以在控制台中看到接入应用的单台机器秒级数据，甚至 500 台以下规模的集群的汇总运行情况。
- **广泛的开源生态**：Sentinel 提供开箱即用的与其它开源框架/库的整合模块，例如与 Spring Cloud、Dubbo、gRPC 的整合。您只需要引入相应的依赖并进行简单的配置即可快速地接入 Sentinel。
- **完善的 SPI 扩展点**：Sentinel 提供简单易用、完善的 SPI 扩展接口。您可以通过实现扩展接口来快速地定制逻辑。例如定制规则管理、适配动态数据源等。

Sentinel 的主要特性：

<img src="/image-20201125115407751.png" alt="image-20201125115407751" style="zoom: 67%;" />

**Sentinel 分为两个部分:**

- **核心库（Java 客户端**）不依赖任何框架/库，能够运行于所有 Java 运行时环境，同时对 Dubbo / Spring Cloud 等框架也有较好的支持。
- **控制台（Dashboard）**基于 Spring Boot 开发，打包后可以直接运行，不需要额外的 Tomcat 等应用容器。

**Sentinel 基本概念:**

- 资源
  - 它可以是 Java 应用程序中的任何内容，例如，由应用程序提供的服务，或由应用程序调用的其它应用提供的服务，甚至可以是一段代码。
  - **资源能够被 Sentinel 保护起来**。大部分情况下，可以使用方法签名，URL，甚至服务名称作为资源名来标示资源。
- 规则
  - 围绕资源的实时状态设定的规则，可以包括**流量控制规则、熔断降级规则以及系统保护规则。所有规则可以动态实时调整。**

#### 2、Hystrix 与 Sentinel 比较

<img src="/image-20201125120634969.png" alt="image-20201125120634969" style="zoom: 80%;" />

#### 3、Sentinel-整合SpringBoot

1、整合sentinel

  1）、导入依赖 spring-cloud-starter-alibaba-sentinel：common模块

```xml
          <!--sentinel熔断、限流-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <!--统计审计信息,实时监控的图像-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```

  2）、Sentinel的控制台：	

​		去官网下载该版本：https://github.com/alibaba/Sentinel，本项目下载V1.8.0的

​		启动：jar包在的文件夹cmd： java -jar sentinel-dashboard-1.8.0.jar

​					若是Tomcat的8080端口被占用，则指明其他端口：java -jar sentinel-dashboard-1.8.0.jar --server.port=8081

​		打开http://localhost:8080

​		用户名：sentinel，密码：sentinel

  3）、配置sentinel控制台地址信息：每个微服务建一个 .properties

```yml
#Sentinel控制台地址
spring.cloud.sentinel.transport.dashboard=localhost:8080
#Sentinel传输端口,可不写
spring.cloud.sentinel.transport.port=8719
#Sentinel Endpoint暴露的路径为/actuator/sentinel，暴露当前应用的IP、所有规则信息、日志目录、Dashboard地址，Block Page与Sentinel Dashboard的心##跳频率等信息，yml不能用 include： *
management.endpoints.web.exposure.include=*  
#打开 Sentinel 对 Feign 的监控
feign.sentinel.enabled=true
```

#### 4、按资源名限流

#####  1、控制台调整流控参数

【默认所有的流控设置保存在项目内存中，重启失效】。先调用某个服务的方法刷新才能有显示

![image-20210719204051964](/image-20210719204051964.png)

![image-20210720163318695](/image-20210720163318695.png)

匀速排队：让请求以均匀速率通过，QPS=2则每500ms过一个

##### 2、自定义流控响应

```Java
//seckill.config.SeckillSentinelConfig：
@Configuration
public class SeckillSentinelConfig implements BlockExceptionHandler {
    /** 2.2.0以后的版本实现的是BlockExceptionHandler；以前的版本实现的是WebCallbackManager */
@Override
public void handle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, BlockException e) throws Exception {
        R error = R.error(BizCodeEnume.TOO_MANY_REQUESTS.getCode(), BizCodeEnume.TOO_MANY_REQUESTS.getMsg());
        httpServletResponse.setCharacterEncoding("UTF-8");
        httpServletResponse.setContentType("application/json");
        httpServletResponse.getWriter().write(JSON.toJSONString(error));
    }
//    /** springBoot2.2.0以后无法引入 WebCallbackManager
//     */
//    public SeckillSentinelConfig() {
//        WebCallbackManager.setUrlBlockHandler((request, response, ex) -> {
//            R error = R.error(BizCodeEnum.TOO_MANY_REQUESTS.getCode(), BizCodeEnum.TOO_MANY_REQUESTS.getMsg());
//            response.setCharacterEncoding("UTF-8");
//            response.setContentType("application/json");
//            response.getWriter().write(JSON.toJSONString(error));
//        });
//    }
}
//common/BizCodeEnume
 TOO_MANY_REQUESTS(10002,"请求流量过大"),
```

##### 3、自定义受保护资源

1、定义方式：一定要配置被限流以后的默认返回     而 url请求可以统一返回 SeckillSentinelConfig

```java
1)、代码：   
	try(Entry entry = SphU.entry("seckillSkus")){
         // 业务逻辑
   }catch(BlockException e){    
   log.error("资源被限流，{}",e.getMessage());
   }
2)、基于注解
   @SentinelResource(value = "getCurrentSeckillSkusResource",blockHandler = "blockHandler")     
 blockHandler 函数会在原方法被限流/降级/系统保护的时候调用；
 而fallback函数会针对所有类型的异常，在同一个类方法上：fallback = ；不是一个类：fallback = ,fallbackClass =类.class,里面的方法要static
```

2、测试

```java
  /**  返回当前时间可以参与的秒杀商品信息，测试sentinel限流
     */
    @SentinelResource(value = "getCurrentSeckillSkus",blockHandler = "blockHandler",fallback = ,fallbackClass = )
    @Override
    public List<SeckillSkuRedisTo> getCurrentSeckillSkus() {
        try(Entry entry = SphU.entry("seckillSkus")){  //测试sentinel限流，资源名
            Set<String> keys = stringRedisTemplate.keys(SESSION_CACHE_PREFIX + "*");  //查出redis中符合条件的key
          。。。。
        }catch(BlockException e){
            log.error("资源被限流，{}",e.getMessage());
        }
        return null;
    }
    public List<SeckillSkuRedisTo> blockHandler(BlockException e){  //自定义限流降级返回方法，在同一个类中，方法的修饰和返回值要一样
        log.error("getCurrentSeckillSkusResource被限流了。。。。");
        return null;
    }
```

#### 5、远程调用的熔断和降级

熔断降级官网解释：https://github.com/alibaba/Sentinel/wiki/%E7%86%94%E6%96%AD%E9%99%8D%E7%BA%A7

##### 1、fegin的熔断

Spring-  Cloud整合Sentinel和Feign：https://github.com/alibaba/spring-cloud-alibaba/wiki/Sentinel

使用Sentinel来保护feign远程调用：熔断。保证依赖中有：sentinel和openFeign的依赖

1）、默认情况下，sentinel是不会对feign进行监控的，在远程调用的调用方需要开启配置

gulimall-product类配置文件添加配置:   product调用seckill

```
#sentinel是不会对feign进行监控的，需要开启配置
feign.sentinel.enabled=true
```

2)、product 远程调用 seckill。调用前把 seckill服务宕机，以前调用会报错，现在使用sentinel的熔断保护机制，调用方可以自定义返回方法和信息。
修改product.feign.SeckillFeignService：

```java
@FeignClient(value = "gulimall-seckill",fallback = SeckillFeignServiceFallBack.class)
public interface SeckillFeignService {。。。}
```
自定义远程调用失败的实现类，添加 product.fallback.SeckillFeignServiceFallBack：

```java
@Component  //放入容器供调用
public class SeckillFeignServiceFallback  implements SeckillFeignService {
    @Override
    public R getSkuSeckillInfo(Long skuId) {
        System.out.println("熔断方法调用.....getSkuSecKillInfo");
        return R.error(BizCodeEnume.TOO_MANY_REQUESTS.getCode(), BizCodeEnume.TOO_MANY_REQUESTS.getMsg());//一个枚举是一个实例
    }
}
```

##### 2、fegin的降级

降级是一种的思想，熔断是降级里的一种策略

1.80以前：没有降级恢复，慢调用的比例阈值默认为50%（平均），最小请求数默认5

![image-20210720171232437](/image-20210720171232437.png)

1.80之后改版。最大RT(响应时间)：请求的响应时间大于该值则统计为慢调用。当单位统计时长（statIntervalMs）内**请求数目大于最小请求数目并且慢调用的比例大于阈值**，则接下来的熔断时长内请求会自动被熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求响应时间小于设置的慢调用 RT 则结束熔断，若大于设置的慢调用 RT 则会再次被熔断。

<img src="/image-20210720171240557.png" alt="image-20210720171240557" style="zoom: 80%;" />

每个服务都开启 sentinel 对 feign 的监控：feign.sentinel.enabled=true，都配置自定义流控响应：  SeckillSentinelConfig

#### 6、网关流控

官网文档：https://github.com/alibaba/Sentinel/wiki/%E7%BD%91%E5%85%B3%E9%99%90%E6%B5%81

**如果能在网关层就进行流控，可以避免请求流入业务，减小服务压力**

gulimall-gateway引入依赖：

```xml
<!-- 引入sentinel网关限流 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-sentinel-gateway</artifactId>
</dependency>
```

要重启一下sentinel控制台：cmd

![image-20210720191406556](/image-20210720191406556.png)

自定义流控响应：

```java
@Configuration
public class SentinelGatewayConfig {
    public SentinelGatewayConfig() {     // GatewayCallbackManager2.2.0后保留了
        GatewayCallbackManager.setBlockHandler(new BlockRequestHandler() {     // 网关限流了 就会回调
            @Override   //Mono Flux响应式编程
            public Mono<ServerResponse> handleRequest(ServerWebExchange serverWebExchange, Throwable throwable) {
                R error = R.error(BizCodeEnume.TOO_MANY_REQUESTS.getCode(), BizCodeEnume.TOO_MANY_REQUESTS.getMsg());
                String errorJson = JSON.toJSONString(error);
                Mono<ServerResponse> body = ServerResponse.ok().body(Mono.just(errorJson), String.class);
                return body;
            }
        });
    }
}
```

#### 7、规则持久化

服务一关配置就没有了，要持久化，引入整合nacos配置：

```
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

yml：

```yml
spring:
  datasource:
    ds1:
      nacos:
        server-addr: localhost:8848
        dataId: cloudalibaba-sentinel-service
        groupId: DEFAULT_GROUP
        data-type: json
```

### 5.5 SpringCloud Alibaba-Seata

#### 1、简介

Seata 是一款开源的分布式事务解决方案，致力于提供高性能和简单易用的分布式事务服务。Seata 将为用户提供了 AT、TCC、SAGA 和 XA 事务模式，为用户打造一站式的分布式解决方案。

文档：https://seata.io/zh-cn/docs/overview/what-is-seata.html

#### 2、核心概念

seata它是由1+3的套件所组成，就是全局唯一事务的id，只要在同一ID下不管几个库，都是全局下面的统一体；3大组件，主要是指TC，TM，RM三个概念。

![image-20210710230823040](/image-20210710230823040.png)

**TC (Transaction Coordinator) - 事务协调者**

维护全局和分支事务的状态，驱动全局事务**提交或回滚**。

**TM (Transaction Manager) - 事务管理器**

定义全局事务的范围：开始全局事务、提交或回滚全局事务。

**RM (Resource Manager) - 资源管理器**

管理分支事务处理的资源，与TC交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。

#### 3、Seata使用

1）、每一个微服务先创建undo_logo回滚日志表; 每个数据库：

```\
CREATE TABLE `undo_log` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `branch_id` bigint(20) NOT NULL,
  `xid` varchar(100) NOT NULL,
  `context` varchar(128) NOT NULL,
  `rollback_info` longblob NOT NULL,
  `log_status` int(11) NOT NULL,
  `log_created` datetime NOT NULL,
  `log_modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `ux_undo_log` (`xid`,`branch_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

2）、安装事务协调器；seata-server: https://github.com/seata/seata/releases  ，本项目下载1.3.0版本的

 3）、整合

* 1、给**使用分布式事务的服务导入依赖**：内有seata-all-1.X.X.jar，版本对应表正确的话就不用exclusion

  ```xml
     <dependency>
          <groupId>com.alibaba.cloud</groupId>
          <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
         
         <exclusions>
             <exclusion>
                 <groupId>io.seata</groupId>
                 <artifactId>seata-all</artifactId>
             </exclusion>
         </exclusions>
     </dependency>
      <dependency>
          <groupId>io.seata</groupId>
          <artifactId>seata-all</artifactId>
          <version>1.3.0</version>
      </dependency>   
  ```

* 解压并修改conf/registry.conf(先备份)，把nacos作为seata的注册中

  ```
  registry {
    # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
    type = "nacos" 
    nacos {
      serverAddr = "localhost:8848"
      namespace = ""
      cluster = "default"
      username = "nacos"
      password = "nacos"
    }
  //下面的config不改的话，默认配置在file.conf中，本项目不改
  ```

* 修改conf/file.conf：本项目采用file配置，不用db配置，不改

  ```
  ## transaction log store, only used in seata-server
  store {
    ## store mode: file、db、redis
    mode = "file"
  ```

* 双击文件夹bin/.bat启动seata-server，查看nacos有没有服务

* **所有**使用分布式事务的微服务使用seata DatasourceProxy代理自己的数据源，添加MySeataConfig配置类：

  ```java
  @Configuration
  public class MySeataConfig {
      @Autowired
      DataSourceProperties dataSourceProperties;
      @Bean
      public DataSource dataSource(DataSourceProperties dataSourceProperties){   //javax.sql.DataSource;
          //得到数据源
       HikariDataSource dataSource = dataSourceProperties.initializeDataSourceBuilder().type(HikariDataSource.class).build();
       if (StringUtils.hasText(dataSourceProperties.getName())){ dataSource.setPoolName(dataSourceProperties.getName()); }
          return new DataSourceProxy(dataSource);    //io.seata包;
      }
  }
  ```

* seata也得配置在注册中心，去seata配置registry.conf，（使用分布式事务的微服务复制registry.conf和file.conf到 resources目录下），修改yml服务分组：

  ```yml
  seata:
    tx-service-group: gulimall-order-fescar-service-group
    enabLed: true
    enable-auto-data-source-proxy: true   #开启自动代理数据源
    appLication-id: gulimall-order
    registry:
      type: nacos
      nacos:
        server-addr: localhost:8848
        namespace:
          userName: "nacos"
          password: "nacos"
        group: SEATA_GROUP           #与server端registry.conf里的group的值相同
        application: seata-server    #与server端registry.conf里的application的值相同
    service:
      vgroup-mapping:
        gulimall-order-fescar-service-group: default
      grouplist:
        default: localhost:8091
    config:
      type: file                     #此处使用file类型进行存储，可更改为db,还需..
      file:
        name: file.conf
  ```

* 给分布式大事务的路口标注**@GlobalTransactional，**分支事务只需要@Transactional

  **注:要是数据库配置因mysql8.0启动失败的情况：**将8.0的驱动jar包自行复制到`lib/jdbc`下

  **高并发场景下:2PC和TCC都不适用的，采用消息队列方案，不采用seata，效率太低，seata的AT模式只适合简单的分布式事务（后台管理系统）**

  

## 六、SpringCloud

### 6.1 Feign 声明式远程调用

#### 6.1.1、简介

Feign 是一个声明式的 HTTP 客户端，他的目的就是让远程调用更加简单，Feign提供了 HTTP请求的模板，**通过编写简单的接口和插入注解**，就可以定义好的 HTTP 请求参数、格式、地址等信息

Feign 整合了 **Ribbon（负载均衡**）和 **Hystrix（服务熔断**），可以让我们不再需要显示地使用这两个组件

Hystrix 服务降级，熔断（QPS数量/异常比例，半熔断，断路器用滑动窗口原理做的），隔离（信号量限流/线程池：每个远程调用放在自己的线程池中，只会影响自己的服务）

SpringCloud - Feign，在 NetflixFeign 的基础上扩展了对 SpringMVC 注解的支持，在其实现下，我们只需创建一个接口并用注解的方式来配置它。

#### 5.2.1引入Feign远程服务调用

**调用者和被调用者都注册进nacos，yml,主启类开启注册发现 @EnableDiscoveryClient，从注册中心找**

##### 1. common引入Feign依赖

```
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

##### 2. 调用者主启动类开启Feign

```java
@EnableDiscoveryClient
@SpringBootApplication
@EnableFeignClients(basePackages = "com.example.gulimall.member.feign")  //包名类上写了可不用了
public class GulimallMemberApplication {
    public static void main(String[] args) {
        SpringApplication.run(GulimallMemberApplication.class, args);
    }
}
```

##### 3、远程调用

**当前测试member模块远程调用coupon模块**，调用者建一个feign包和 feign/CouponFeignService 接口，**指定调用哪个服务的哪个方法**

```java
//调用者gulimall-membber
@FeignClient("gulimall-coupon")     //服务名，从注册中心找
public interface CouponFeignService {
    @RequestMapping("coupon/coupon/member/list")    //controller方法地址
    public R membercoupons();
}
//gulimall-coupon：被调用者
@RestController
@RequestMapping("coupon/coupon")
public class CouponController {
    @RequestMapping("/member/list")
    public R membercoupons() {}
```

##### 4、Feign 使用三步

1、导包 openfeign

2、调用者开启 @EnableFeignClients 功能(主类)，接口@FeignClient("服务名")

3、调用者编写接口，进行远程调用

### 6.2 Gateway作为API网关

#### 6.2.1、简介

API 网关是介于客户端和服务器端之间的中间层，是请求流量的入口，功能包括**动态路由转发，权限校验，限流控制等。**springcloud `gateway`取代了`zuul`网关

Spring Cloud Gateway依赖Spring Boot和**Spring Webflux提供的Netty runtime（异步非阻塞响应式编程）**。它不能在传统的Servlet容器（Tomcat）中工作或构建为WAR，gateway要注册到nacos中。

参考手册：https://cloud.spring.io/spring-cloud-gateway/2.2.x/reference/html/

三大核心概念：

**Route路由:** 发请求给网关，网关将请求路由到指定的服务。路由有id，目的地uri，断言的集合，匹配了断言（true）就能到达指定位置。
**Predicate断言**: 就是java里的断言函数，匹配请求里的任何信息，包括请求头等。根据请求头路由哪个服务
**Filter过滤器**: 请求和响应都可以被修改。客户端发请求给服务端，中间有网关，先交给映射器，如果能处理就交给handler处理，然后交给一系列filer，然后给指定的服务，再返回回来给客户端。

<img src="/fdc2f8288ce78208ae8283ba3badc009.png" alt="img" style="zoom:67%;" />

#### 6.2.2 mall-gateway

maven建子模块，pom.xml引入依赖：**（要排除web依赖，且web不直接在父工程全局依赖中）**

```
<dependencies>
        <dependency>
            <groupId>com.zsy</groupId>
            <artifactId>mall-common</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-web</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

**主启动类（排除数据源连接依赖）**

```
@EnableDiscoveryClient
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
public class MallGatewayApplication {
```

bootstrap.yaml，读取配置中心的配置，在配置中心为每个分组创建各自的命名空间。

```
spring.cloud.nacos.server-addr=localhost:8848
spring.application.name=gulimall-gateway
spring.cloud.nacos.config.namespace=d36287cb-8e66-40df-b2ab-727f607e7136
```

yml，配置gateway路由规则

```
spring:
  application:
    name: gulimall-gateway
  cloud:
    nacos:
      server-addr: localhost:8848
    gateway:
      routes:
        - id: test_route
          uri: https://www.baidu.com
          predicates:
            - Query=url,baidu

        - id: qq_route
          uri: https://www.qq.com
          predicates:
            - Query=url,qq
            
        - id: admin_route
          uri: lb://renren-fast
          predicates:
            - Path=/api/**
          filters:  # 这段过滤器和验证码有关，api内容缓存了/renren-fast，还得注意/renren-fast也注册到nacos中
            - RewritePath=/api/(?<segment>.*),/renren-fast/$\{segment}
server:
  port: 88
```

测试 ：localhost:8080/?url=baidu

### 6.3 Sleuth+Zipkin 服务链路追踪

官方文档：https://docs.spring.io/spring-cloud-sleuth/docs/2.2.5.RELEASE/reference/html/

#### 1、为什么用

微服务架构是一个分布式架构，必须实现分布式链路追踪，去跟进一个请求有哪些服务参与和参与的顺序，从而达到出了问题，很快定位。

链路追踪的开源组件有Google 的Dapper, Twitter 的Zipkin,以及阿里的Eagleeye(鹰眼)等。

#### 2、基本术语

- Span (跨度) :基本工作单元，发送一个远程调度任务就会产生一个Span, Span 是一个64位ID唯一标识的，Trace 是用另一个64ID唯一标识的，Span 还有其他数据信息，比如摘要、时间戳事件、Span的ID、 以及进度ID。
- Trace (跟踪) :一系列Span组成的一个树状结构。请求一个微服务系统的API接口，这个API接口，需要调用多个微服务，调每个微服务都会产生一个新的Span,所有由这个请求产生的Span组成了这个Trace
- Annotation (标注) :用来及时记录一个事件的，一些核心注解用来定义一个请求的开始和结束。这些注解包括以下:
  - cs- Client Sent客户端发送一 个请求，这个注解描述了这个Span的开始
  - sr- Server Received -服务端获得请求，如果将其sr诚去cs时间戳便可得到网络传输的时间。
  - ss- Server Sent(服务端 发送响应)-如果ss的时间戳诚去sr时间戳，就可以得到服务器请求的时间。
  - cr- Client Received (客 户端接收响应)此时Span的结束，如果cr的时间戳诚去cs时间戳便可以得到整个请求所消耗的时间。

#### 3、整合 Sleuth

1、服务提供者与消费者导入依赖，common：

```xml
<!--    链路追踪sleuth-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-sleuth</artifactId>
    </dependency>
```

2、打开 debug 日志，测试加在product和senkill中，

```xml
#开启debug日志
logging.level.org.springframework.cloud.openfeign=debug
logging.level.org.springframework.cloud.sleuth=debug
```

3、发起一次远程调用，观察idea控制台

#### 4、整合 zipkin 可视化观察

 Sleuth 可产生的调用链监控信息，需要一个图形化的工具 -Zipkin ，Zipkin 是 Twitter 开源的分布式跟踪系统，zipkin 官网地址如下：https://zipkin.io/

<img src="/image-20201127111241744.png" alt="image-20201127111241744" style="zoom: 80%;" />

1、docker 安装 zipkin服务器

```
docker run -d -p 9411:9411 openzipkin/zipkin
```

2、common/pom.xml

```xml
<!--   zipkin 可视化观察,里面包含了spring-cloud-starter-sleuth，可不再导入sleuth了  -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-zipkin</artifactId>
    </dependency>
```

3、application.yml，所有微服务

```properties
# zipkin服务器的URL
spring.zipkin.base-url=http://192.168.56.56:9411/
# 关闭服务发现，否则spring-cloud把zipkin的地址当服务名
spring.zipkin.discovery-client-enabled=false
# 设置http方式传输数据
spring.zipkin.sender.type=web
# 设置抽样采集率为1，默认0.1，即10%
spring.sleuth.sampler.probability=1
```

Zipkin图形化地址：http://192.168.56.56:9411/zipkin/

<img src="/image-20210720222042345.png" alt="image-20210720222042345" style="zoom:80%;" />

#### 5、Zipkin 数据持久化

Zipkin默认是将监控数据存储在内存的，如果Zipkin停掉，监控数据就会丢失。所以如果想要搭建生产可用的Zipkin，就需要实现监控数据的持久化。

Zipkin可储存在：内存（默认）、MySQL（数据量大时慢）、Elasticsearch （推荐）、Cassandra（Twitter官方）

Elasticsearch ：通过docker方式：

```
docker run --enw STORAGE_TYPE=elasticsearch --env ES_HOSTS=192.168.56.56:9200 openzipkin/zipkin-dependencies
```



## 七、前端开发基础知识

前端技术栈类比：

<img src="/image-20210710225446089.png" alt="image-20210710225446089" style="zoom:80%;" />

**js（JavaScript）是网页脚本语言**

**Ajax是一门技术，它提供了异步更新的机制，实现页面的局部更新而非整个页面文档**

**jQuery 和 Vue是框架，js库；**

**js和jQuery区别**：js是利用DOM方法创建元素节点，jQuery使用选择器（$）选取创建DOM对象

**1 定位元素** 
JS ：document.getElementById("abc") 

jQuery ：$("#abc") 通过id定位 ；$(".abc") 通过class定位 ；$("div") 通过标签定位 

需要注意的是JS返回的结果是这个元素，jQuery返回的结果是一个JS的对象。以下例子中假设已经定位了元素abc。 

**2 改变元素的内容** 
JS ：abc.innerHTML = "test";         //现在的项目中有用到
jQuery ：abc.html("test"); 

**3 显示隐藏元素** 
JS ：abc.style.display = "none";        //现在的项目中有用到				abc.style.display = "block"; 

jQuery ：abc.hide();  	 abc.show();		abc.toggle();     //在显示和隐藏之间切换、
**4 获得焦点** 

JS和jQuery是一样的，都是abc.focus(); 

**5 为表单赋值** 
JS ：	abc.value = "test"; 
jQuery ：	abc.val("test"); 

**6 获得表单的值** 
JS ：alert(abc.value); 
jQuery ：alert(abc.val()); 

**7 设置元素不可用** 
JS ：abc.disabled = true; 
jQuery ：abc.attr("disabled", true);

**8 修改元素样式**
JS：abc.style.fontSize=size;			abc.className="test";
jQuery：abc.css('font-size', 20);  			abc.removeClass(); 			abc.addClass("test");

**9** **判断复选框是否选中**

jQuery：if(abc.attr("checked") == "checked")

**vue和 jQuery区别**：Vue 对象将数据和 View 完全分离开来并 VM 实现相互的绑定，不再需要引用相应的DOM对象，这就是MVVM模式了。

```css
#vue：列表中添加一个元素和控制按钮的显示隐藏功能
<body>
    <div id="app">
        <ul>
            <!--根据数组数据自动渲染页面-->
            <li v-for="item in message">{{item}}</li>
        </ul>
        <button @click="add" v-show="isShow">添加数据</button>
        <button @click="showButton">隐藏按钮</button>
    </div>
</body>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            message: ["第1条数据"],
            i:2,
            isShow:true
        },
        methods:{
            //向数组添加一条数据即可
            add:function(){
                this.i++
                this.message.push("第"+this.i+"条数据")
            },
            //控制isShow的值即可
            showButton:function(){
                this.isShow=false;
            }
        }
    })
</script>

#jQuery
<body>
    <div id="app">
        <ul id="list">
            <li>第1条数据</li>
            <li>第2条数据</li>
        </ul>
        <button id="add">添加数据</button>
        <button id="showButton">隐藏按钮</button>
    </div>
</body>
<script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>
<script>
    $(document).ready(function() {  
    var i=2;
    $('#add').click(function() { 
       i++;
       //通过dom操作在最后一个li元素后手动添加一个标签
      $("#list").children("li").last().append("<li>第"+i+"条数据</li>");
    });
    //需要手动隐藏dom元素
    $("#showButton").click(function(){
        $("#add").hide();
    })
  });
</script>
```

### 7.1 ES6

ECMAScript6.0（以下简称ES6，ECMAScript是一种由Ecma国际通过ECMA-262标准化的脚本），是JavaScript语言的下一代标准，2015年6月正式发布，从ES6开始的版本号采用年号，如

ES2015，就是ES6。
ES2016，就是ES7。
ES2017，就是ES8。
**ECMAScript是规范，JS的规范的具体实现。**

打开VSCode—打开文件夹—新建es6文件夹—新建文件1、let.html—**shift+!+Enter生**成模板。填入下面内容后，右键open with live server（保存即可在网页更新）

#### 1、let作用域.html

let不会作用到{}外，var会越域跳到{}外
var可以多次声明同一变量，let,const声明之后不允许改变, 一但声明必须初始化，否则会报错

var 会变量提升（打印和定义可以顺序反）。let 不存在变量提升（顺序不能反）

#### 2、解构表达式.html

支持let arr = [1,2,3]; let [a,b,c] = arr;这种语法  (arr[0]=1)
支持对象解析：const { name: abc, age, language } = person; 冒号代表改名，旧:新   (person.name)    
字符串函数
支持一个字符串为多行``
占位符功能 ${}

```
 <script>
        const person = {
            name: "jack",
            age: 21,
            language: ['java', 'js', 'css']
        }
        //对象解构 // 把name属性变为abc，声明了abc、age、language三个变量
        const { name: abc, age, language } = person;
        console.log(abc, age, language)
        //4、字符串扩展
        let str = "hello.vue";
        console.log(str.startsWith("hello"));//true
        console.log(str.endsWith(".vue"));//true
        console.log(str.includes("e"));//true
        console.log(str.includes("hello"));//true
        //字符串模板 ``可以定义多行字符串
        let ss = `<div>
                    <span>hello world<span>
                </div>`;
        console.log(ss);
        function fun() {      //f:function(){}  或  f(){}
            return "这是一个函数"
        }
        // 2、字符串插入变量和表达式。变量名写在 ${} 中，${} 中可以放入 JavaScript 表达式。
        let info = `我是${abc}，今年${age + 10}了, 我想说： ${fun()}`;
        console.log(info);
    </script>
```

#### 3、**函数优化**.html

支持函数形参默认值 function add2(a, b = 1) {    		//没传就会自动使用默认值
支持不定参数 function fun(...values) {
支持箭头函数 var print = obj => console.log(obj);
箭头函数+结构 var hello2 = ({name}) => console.log("hello," +name);，本来应该是person.name

```
		 const person = {
            name: "jack",
            age: 21,
            language: ['java', 'js', 'css']
        }
        function hello(person) {
            console.log("hello," + person.name)
        }
        //箭头函数+解构
        var hello2 = ({name}) => console.log("hello," +name);
        hello2(person);
```

#### 4、对象优化.html

可以获取map的键值对等Object.keys()、values、entries
Object.assign(target,source1,source2) 合并
如果属性名和属性值的变量名相同，可以省略const person2 = { age, name } 代表age属性的值是变量age的值
…代表取出该对象所有属性拷贝到当前对象。let someone = { …p1 }

```
<script>
        const person = {
            name: "jack",
            age: 21,
            language: ['java', 'js', 'css']}
        console.log(Object.keys(person));//["name", "age", "language"]
        console.log(Object.values(person));//["jack", 21, Array(3)]
        console.log(Object.entries(person));//[Array(2), Array(2), Array(2)]

        const target  = { a: 1 };
        const source1 = { b: 2 };
        const source2 = { c: 3 };
        // 合并
        Object.assign(target, source1, source2);
        console.log(target);  //{a:1,b:2,c:3}
        //2）、声明对象简写
        const age = 23
        const name = "张三"
        const person1 = { age: age, name: name }
        // 等价于
        const person2 = { age, name }//声明对象简写
        //3）、对象的函数属性简写
        let person3 = {
            name: "jack",
            // 以前：
            eat: function (food) {
                console.log(this.name + "在吃" + food);
            },
            //箭头函数this不能使用，要使用的话需要使用：对象.属性
            eat2: food => console.log(person3.name + "在吃" + food)
        }
        //4）、对象拓展运算符
        // 1、拷贝对象（深拷贝）
        let p1 = { name: "Amy", age: 15 }
        let someone = { ...p1 }
        console.log(someone)  //{name: "Amy", age: 15}
        // 2、合并对象
        let age1 = { age: 15 }
        let name1 = { name: "Amy" }
        let p2 = {name:"zhangsan"}
        p2 = { ...age1, ...name1 } 
    </script>
```

#### 5、map和reduce.html

`map()`：接收一个函数，将原数组中的所有元素用这个函数处理后放入新数组返回。
`reduce()` 为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元素，

```
//let arr=[2, 40, -10, 6]
arr = arr.map(item=> item*2);
arr.reduce(callback,[initialValue])    
/**
    1、previousValue （上一次调用回调返回的值，或者是提供的初始值（initialValue））
    2、currentValue （数组中当前被处理的元素）
    3、index （当前元素在数组中的索引）
    4、array （调用 reduce 的数组）*/
     let result = arr.reduce((a,b)=>{
            console.log("上一次处理后："+a);
            console.log("当前正在处理："+b);
            return a + b;
        },100);
        console.log(result)
```

#### 6、promise.html

优化异步操作。封装ajax

```javascript
<script>
        //1、查出当前用户信息
        //2、按照当前用户的id查出他的课程
        //3、按照当前课程id查出分数
        // $.ajax({
        //     url: "mock/user.json",
        //     success(data) {
        //         console.log("查询用户：", data);
        //         $.ajax({
        //             url: `mock/user_corse_${data.id}.json`,
        //             success(data) {
        //                 console.log("查询到课程：", data);
        //                 $.ajax({
        //                     url: `mock/corse_score_${data.id}.json`,
        //                     success(data) {
        //                         console.log("查询到分数：", data);
        //                     },
        //                     error(error) {
        //                         console.log("出现异常了：" + error);
        //                     }
        //                 });
        //             },
        //             error(error) {
        //                 console.log("出现异常了：" + error);
        //             }
        //         });
        //     },
        //     error(error) {
        //         console.log("出现异常了：" + error);
        //     }
        // });
    
    //		let p = new Promise((resolve, reject) => {
 //		p.then((obj) => { //成功以后做什么
        
		//1、Promise可以封装异步操作,提取出公共代码，自己定义一个方法整合一下
        function get(url, data) { 
            return new Promise((resolve, reject) => {		//传入成功解析，失败拒绝
                $.ajax({										//1、异步操作
                    url: url,
                    data: data,
                    success: function (data) {
                        resolve(data);
                    },
                    error: function (err) {
                        reject(err)
                    }
                })
            });
        }
        //return返回封装异步操作后执行的成功或失败数据对象，then是成功后继续操作，d表示对象中的数据
        get("mock/user.json")
            .then((data) => {  		
                console.log("用户查询成功~~~:", data)
                return get(`mock/user_corse_${data.id}.json`);
            })
            .then((data) => {
                console.log("课程查询成功~~~:", data)
                return get(`mock/corse_score_${data.id}.json`);
            })
            .then((data)=>{
                console.log("课程成绩查询成功~~~:", data)
            })
            .catch((err)=>{ //失败的话catch
                console.log("出现异常",err)
            });
    </script>
```

#### 7、模块化import/export

模块化就是把代码进行拆分，方便重复利用。类似于java中的导包，而JS换了个概念，是导模块。

模块功能主要有两个命令构成 export 和import

- export用于规定模块的对外接口,不仅可以导出对象，一切JS变量都可以导出。比如：基本类型变量、函数、数组、对象
- import用于导入其他模块提供的功能

user.js

```js
var name = "jack"
var age = 21
function add(a,b){
    return a + b;
}
// 导出变量和函数
export {name,age,add}
```

hello.js

```
// export const util = {
//     sum(a, b) {
//         return a + b;
//     }
// }
// 导出后可以重命名
export default {
    sum(a, b) {
        return a + b;
    }
}
// export {util}
```

main.js

```
import abc from "./hello.js"
import {name,add} from "./user.js"
abc.sum(1,2);
console.log(name);
add(1,3);
```

### 7.2、Vue

#### 1、MVVM思想

M：model 包括数据和一些基本操作
V：view 视图，页面渲染结果
VM：View-model，模型与视图间的双向操作（无需开发人员干涉）
视图和数据通过VM绑定起来，model里有变化会自动地通过Directives填写到视view中，视图表单中添加了内容也会自动地通过DOM Listeners保存到模型中。

教程：https://cn.vuejs.org/v2/guide/
只要我们Model发生了改变，View上自然就会表现出来。当用户修改了View，Model中的数据也会跟着改变。
把开发人员从琐的D0M操作中解放出来，那关注点放在如何操作Model上。
<img src="/image-20210606200330717.png" alt="image-20210606200330717" style="zoom:80%;" />

 使用：

- new Vue，创建vue实例，关联页面的模板，将自己的数据（data）渲染到关联的模板，响应式的
- 在dom中{{name}}代表从模型中放到view中
- v-model实现双向绑定

index.html

```
<body>
    <div id="app">
        <input type="text" v-model="num">			//v-model实现双向绑定。此处代表输入框和vue里的data 
        <button v-on:click="num++">点赞</button>		 // v-on:click绑定事件，实现自增。
		  <button v-on:click="cancel">取消</button>		 // 回调自定义的方法。 此时字符串里代表的函数   
        <h1> {{name}} ,非常帅，有{{num}}个人为他点赞{{hello()}}</h1>
      //  先从vue中拿到值填充到dom，input再改变num值，vue实例更新，然后此处也更新 ,还可以在html控制台vm.name
    </div>
    <!-- 导入依赖 -->
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        //1、vue声明式渲染
        let vm = new Vue({ //生成vue对象
            el: "#app",//绑定元素 div id="app" // 可以指定恰标签，但是不可以指定body标签
            data: {  //封装数据
                name: "张三",  // 也可以使用{} //表单中可以取出
                num: 1
            },
            methods:{  //封装方法来做更复杂的操作
                cancel(){
                    this.num -- ;
                },
                hello(){
                    return "1"
                }
            }
        });
    </script>
</body>
```

#### 2、v-text、v-html、v-ref

这两个可以使用data数据。而`<div>123{{}}</div>`这种写法叫**插值表达式**，可以计算，可以取值，可以调用函数
这里还介绍v-html v-text区别
注意取的大多数不是请求域了，而是vue对象里的data

插值闪烁：

使用{{}}方式在网速较慢时会出现问题。在数据未加载完成时，页面会显示出原始的`{{}}`，加载完毕后才显示正确数据，我们称为插值闪烁。

```js
 <div id="app">
        {{msg}}  {{1+1}}  {{hello()}} 前面的内容如果网速慢的话会先显示括号，然后才替换成数据。
        v-html 和v-text能解决这个问题
        <br/>
        用v-html取内容
        <span v-html="msg"></span>
        <br/>
        原样显示
        <span v-text="msg"></span>  
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        new Vue({
            el:"#app",
            data:{
                msg:"<h1>Hello</h1>",
                link:"http://www.baidu.com"
            },
            methods:{
                hello(){
                    return "World"
                }
            }
        })
    </>
```

#### 3、单向绑定v-bind

插值表达式{{}}只能用在标签体里，如果我们这么用`<a href="{{}}">`是不起作用的，所以需要 `<a v-bind:href="link">跳转</a>`这种用法

解决：用**`v-bind:`，简写为`:`**。表示把model绑定到view。可以设置src、title、class等

class,style  {class名：vue值}  特殊字符要用飘号``,或者用驼峰命名法

```
 <div id="app"> 
        <a v-bind:href="link">跳转</a>
    <span v-bind:class="{active:isActive,'text-danger':hasError}":style="{color: color1,fontSize: size}">你好</span>
  </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        let vm = new Vue({
            el:"#app",
            data:{
                link: "http://www.baidu.com",
                isActive:true,
                hasError:true,
                color1:'red',
                size:'36px'
            }
        })
    </script>
```

#### 4、**双向绑定v-model**

v-bind只能从model到view。v-model能从view到model

```
 <div id="app">
        精通的语言：如果是多选框，那么会把每个value值赋值给vue数据
            <input type="checkbox" v-model="language" value="Java"> java<br/>
            <input type="checkbox" v-model="language" value="PHP"> PHP<br/>
            <input type="checkbox" v-model="language" value="Python"> Python<br/>
        选中了 {{language.join(",")}}
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        let vm = new Vue({
            el:"#app",
            data:{
                language: []
            }
        })
    </script>
```

**表单、复选框：**

多选checkbox：虽然v-model指定了同个值，但是会收集成数组。单选radio。 下拉select

修饰符:

**.lazy**					<input v-model.lazy="msg" >

在默认情况下， v-model 在 input 事件中同步输入框的值与数据，但你可以添加一个修饰符 lazy ，从而转变为在 change 事件中同步：

**.number**

想自动将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值），可以添加一个修饰符 number 给 v-model 来处理输入值，这通常很有用，因为在 type=“number” 时 HTML 中输入的值也总是会返回字符串类型。

**.trim**

如果要自动过滤用户输入的首尾空格，可以添加 trim 修饰符到 v-model 上过滤输入：

#### 5、v-on事件

事件监听可以使用 v-on 指令,	v-on:事件类型="方法"` ，**简写成	@事件类型="方法"**

事件冒泡：大小div都有单机事件，点了内部div相当于外部div也点击到了。

- `.stop` - 阻止冒泡
- `.prevent` - 阻止默认事件
- `.once` - 只触发一次
- `.capture` - 阻止捕获
- `.self` - 只监听触发该元素的事件

用法是`v-on:事件类型.事件修饰符="方法"`

```
 	<div id="app">
        <button v-on:click="num++">点赞</button>         
        <!--事件指定一个回调函数，必须是Vue实例中定义的函数-->
        <button @click="cancel">取消</button>
        <h1>有{{num}}个赞</h1>
        <!-- 事件修饰符 -->
        <div style="border: 1px solid red;padding: 20px;" v-on:click.once="hello">		//大div
            <div style="border: 1px solid blue;padding: 20px;" @click.stop="hello"> <br />  // 小div 
                <a href="http://www.baidu.com" @click.prevent.stop="hello">去百度</a>
            </div>
        </div>
        <!--还可以绑定按键修饰符, 按键修饰符： -->
        <input type="text" v-model="num" v-on:keyup.up="num+=2" @keyup.down="num-=2" @click.ctrl="num=10"><br />
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        new Vue({
            el:"#app",
            data:{
                num: 1
            },
            methods:{
                cancel(){
                    this.num--;
                },
                hello(){
                    alert("点击了")
                }
            }
        })
    </script>
```

#### 6、v-for遍历

可以遍历 数组[] 字典{} 。对于字典`<li v-for="(value, key, index) in object">`

```js
<div id="app">
        <ul>
            <!-- 4、遍历的时候都加上:key来区分不同数据，提高vue渲染效率 -->
            <li v-for="(user,index) in users" :key="user.name" v-if="user.gender == '女'">
               当前索引：{{index}} ==> {{user.name}}  ==> {{user.gender}} ==>{{user.age}} <br>
                <!-- 3、遍历对象：
                        v-for="value in object"
                        v-for="(value,key) in object"
                        v-for="(value,key,index) in object"  -->
            </li>           
        </ul>
        <ul>
            <li v-for="(num,index) in nums" :key="index"></li>   <!-- :key来区分不同数据，提高vue渲染效率 -->
        </ul>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>         
        let app = new Vue({
            el: "#app",
            data: {
                users: [
                { name: '柳岩', gender: '女', age: 21 },
                { name: '张三', gender: '男', age: 18 },
                { name: '范冰冰', gender: '女', age: 24 },
                { name: '刘亦菲', gender: '女', age: 18 }],
                nums: [1,2,3,4,4]
            },
        })
    </script>
```

#### 7、v-if和v-show

区别：show的标签F12一直都在，if的标签会移除，if操作dom树对性能消耗大

v-if和v-show.html

```
 <!-- 
        v-if，顾名思义，条件判断。当得到结果为true时，所在的元素才会被渲染。
        v-show，当得到结果为true时，所在的元素才会被显示。 
    -->
    <div id="app">
        <button v-on:click="show = !show">点我呀</button>
        <h1 v-if="show">if=看到我....</h1>
        <h1 v-show="show">show=看到我</h1>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>    
    <script>
        let app = new Vue({
            el: "#app",
            data: {
                show: true
            }
        })
    </script>
```

#### 8、v-else和v-else-if

```
<div id="app">
        <button v-on:click="random=Math.random()">点我呀</button>
        <span>{{random}}</span>
        <h1 v-if="random>=0.75">
            看到我啦? &gt;= 0.75
        </h1>
        <h1 v-else-if="random>=0.5">
            看到我啦? &gt;= 0.5
        </h1>
        <h1 v-else-if="random>=0.2">
            看到我啦? &gt;= 0.2
        </h1>
        <h1 v-else>
            看到我啦? &lt; 0.2
        </h1>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>   
    <script>         
        let app = new Vue({
            el: "#app",
            data: { random: 1 }
        })     
    </script>
```

#### 9、计算属性和侦听器

**计算属性compute**:	**属性**不是具体值，而是通过一个函数计算出来的，随时变化(l类似methods)

```
<div id="app">
		<ul>
            <li>西游记； 价格：{{xyjPrice}}，数量：<input type="number" v-model="xyjNum"> </li>
            <li>水浒传； 价格：{{shzPrice}}，数量：<input type="number" v-model="shzNum"> </li>
            <li>总价：{{totalPrice}}</li>
            {{msg}}
        </ul>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>

    <script> 
        new Vue({
            el: "#app",
            data: {
                xyjPrice: 99.98,
                shzPrice: 98.00,
                xyjNum: 1,
                shzNum: 1,
                msg: ""
            },
            computed: {		 <!-- 某些结果是基于之前数据实时计算出来的，我们可以利用计算属性。来完成 -->
                totalPrice(){
                    return this.xyjPrice*this.xyjNum + this.shzPrice*this.shzNum
                }
            },
            watch: {     //watch可以让我们监控一个值的变化。从而做出相应的反应。
                xyjNum: function(newVal,oldVal){
                    if(newVal>=3){
                        this.msg = "库存超出限制";
                        this.xyjNum = 3
                    }else{
                        this.msg = "";
                    }
                }
            },
        })
    </script>
```

**过滤器filter**:	过滤器常用来处理文本格式化的操作。可以用在两个地方：双花括号插值和 v-bind 表达式。管道符后面跟具体过滤器`{{user.gender | gFilter}}`

```
 <div id="app">
        <ul>
            <li v-for="user in userList">
 {{user.id}} ==> {{user.name}} ==> {{user.gender == 1?"男":"女"}} ==> {{user.gender | genderFilter}} ==> {{user.gender | gFilter}}
            </li>
        </ul>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        // 全局过滤器
        Vue.filter("gFilter", function (val) {
            if (val == 1) {
                return "男~~~";
            } else {
                return "女~~~";
            }
        })
        let vm = new Vue({
            el: "#app",
            data: {
                userList: [
                    { id: 1, name: 'jacky', gender: 1 },
                    { id: 2, name: 'peter', gender: 0 }
                ]
            },
            filters: { // 局部过滤器，只可以在当前vue实例中使用
                genderFilter(val) {
                    if (val == 1) {
                        return "男";
                    } else {
                        return "女";
                    }
                }
            }
        })
    </script>
```

#### 10、组件化

往往不同的页面，也会有相同的部分。例如可能会有相同的头部导航。所以我们会把页面的不同分拆分成立的组件，然后在不同页面就可以共享这些组件，避免重复开发。

**组件其实也是一个vue实例**，因此它在定义时也会接收：data、methods、生命周期函数等
不同的是组件不会与页面的元素绑定（所以不写el），否则就无法复用了，因**此没有el属性。**
但是组件渲染需要html模板，所以增加了**template属**性，值就是HTML模板
**data必须是一个函数**，不再是一个对象。
全局组件定义完毕，任何vue实例都可以直接在HTML中通过组件名称来使用组件了
**1) 全局组件**

**2) 局部组件**：注意：局部注册的组件在其子组件中不可用

```js
 <div id="app">
        <button v-on:click="count++">我被点击了 {{count}} 次</button>
       // 每个对象都是独立统计的
        <counter></counter>
        <counter></counter>
        <counter></counter>
        <button-counter></button-counter>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        //1、全局声明注册一个组件,组件名为counter,代表模板里的button
        // 把页面中<counter>标签替换为指定的template，而template中的数据用data填充
        Vue.component("counter", {
            template: `<button v-on:click="count++">我被点击了 {{count}} 次</button>`,     // 渲染的模板
            data() {	// 如果 Vue 没有这条规则，点击一个按钮就会一样影响到其它所有实例：
                return {
                    count: 1 // 数据
                }
            }
        });
        //2、局部声明一个组件 ,这个东西可以在当前文件中写，也可以import
        const buttonCounter = {
            template: `<button v-on:click="count++">我被点击了 {{count}} 次~~~</button>`,
            data() {		// 不定义el，可以定义data函数
                return {
                    count: 1
                }
            }
        };
        new Vue({
            el: "#app",
            data: {
                count: 1
            },
            components: { // 要用的组件
                'button-counter': buttonCounter		//局部组件在div中感应不到，所以要绑定一下，
            }
        })
    </script>
```

#### 11、生命周期钩子函数

每个vue实例在被创建时都要经过一系列的初始化过程：创建实例，装载模板、渲染模板等等vue为生命周期中的每个状态都设置了钩子函数（监听函）。每当vue实列处于不同的生命周期时，对应的函数就会被触发调用。

<img src="https://cn.vuejs.org/images/lifecycle.png" alt="img" style="zoom:50%;" />

```
 <div id="app">
        <span id="num">{{num}}</span>
        <button @click="num++">赞！</button>
        <h2>{{name}}，有{{num}}个人点赞</h2>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        let app = new Vue({
            el: "#app",
            data: {
                name: "张三",
                num: 100
            },
            methods: {
                show() {
                    return this.name;
                },
                add() {
                    this.num++;
                }
            },
            beforeCreate() {
                console.log("=========beforeCreate=============");
                console.log("数据模型未加载：" + this.name, this.num);
                console.log("方法未加载：" + this.show());
                console.log("html模板未加载：" + document.getElementById("num"));
            },
            created: function () {
                console.log("=========created=============");
                console.log("数据模型已加载：" + this.name, this.num);
                console.log("方法已加载：" + this.show());
                console.log("html模板已加载：" + document.getElementById("num"));
                console.log("html模板未渲染：" + document.getElementById("num").innerText);
            },
            beforeMount() {
                console.log("=========beforeMount=============");
                console.log("html模板未渲染：" + document.getElementById("num").innerText);
            },
            mounted() {
                console.log("=========mounted=============");
                console.log("html模板已渲染：" + document.getElementById("num").innerText);
            },
            beforeUpdate() {
                console.log("=========beforeUpdate=============");
                console.log("数据模型已更新：" + this.num);
                console.log("html模板未更新：" + document.getElementById("num").innerText);
            },
            updated() {
                console.log("=========updated=============");
                console.log("数据模型已更新：" + this.num);
                console.log("html模板已更新：" + document.getElementById("num").innerText);
            }
        });
    </script>
```



### 7.3、vue项目快速搭建

观察renren-fast-vue，只有一个index.html，其他都是vue文件夹
**win的cmd下下载工具依赖**：						**#防止点击停止，右键cmd属性去掉快速编辑**
**npm install webpack -g								# 安装webpack**
**npm install -g @vue/cli  							# 安装vue脚手架**

**建文件夹，在项目文件夹里执行， 初始化vue项目**：

**vue init webpack vue-demo					#vue-demo是项目名，一个劲回车，ESlint及后面的都选择no**
**npm run dev 											# 运行，访问8080端口**

![image-20210606230153840](/image-20210606230153840.png)


#### 1、**关系结构**

1) index.html

index.html只有一个`<div id="app">`

```
<body>
  <div id="app"></div>
</body>
```

2) main.js

main.js中import，并且new Vue，		（app.vue为也页面的展示，到main.js，再到路由匹配，路由再到组件）

```
import Vue from 'vue'
import App from './App'
import router from './router'
import ElementUI  from 'element-ui'     ////加的ElementUI依赖
import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI);					//加的ElementUI依赖
Vue.config.productionTip = false
new Vue({		
  el: '#app',		 		// 他是index.html中的div
  router,					//router:router，是从上面的./router里来的 // 会替换<router-view/>
  components: { App },		//App:App  属性名和属性值一样时可以简写 // 要用的组件
  template: '<App/>' 		// 导入的<template> <div id="app">
})
```

3) App.vue（单文件组件：三部分）

```js
<template>
  <el-container style="height: 500px; border: 1px solid #eee">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
      <el-menu :default-openeds="['1', '3']" router="true">
        <el-submenu index="1">
          <template slot="title">
            <i class="el-icon-message"></i>导航一
          </template>
          <el-menu-item-group>
            <template slot="title">分组一</template>
            <el-menu-item index="/table">用户列表</el-menu-item>
            <el-menu-item index="/hello">Hello</el-menu-item>
          </el-menu-item-group>.......
</template>
<script>
  export default {		 // 导出vue实例。相当于合并了以前的new vue和export导出组件
  data() {
    const item = {
      date: "2016-05-02",
      name: "王小虎",
      address: "上海市普陀区金沙江路 1518 弄"
    };
    return {
      tableData: Array(20).fill(item)
    };
  }
};
</script>
<style>
</style>
```

4)route下的index.js,**@表示在src目录下**

```
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Hello from '@/components/Hello'
import MyTable from '@/components/MyTable'

Vue.use(Router)
export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',     
      component: HelloWorld
    },
    {
      path: '/hello',
      name: "Hello",
      component: Hello
    },
    {
      path: '/table',
      name: 'MyTable',
      component: MyTable
    }
  ]
})
```

5）components下的自定义的hello

```
<template>
  <div>
    <h1>你好，Hello，{{name}}</h1>
    <el-radio v-model="radio" label="1">备选项1</el-radio>
    <el-radio v-model="radio" label="2">备选项2</el-radio>
  </div>
</template>
<script>
export default {
  data() {
    return {
      name: "张三",
      radio: "2"
    };
  }
};
</script>
<style >
</style>
```

#### 2、构建模板vue.json

**文件–首选项–用户代码片段–新建代码片段（新建成全局代码片段）–取名vue.json**， 

```
{  "Print to console": {
        "prefix": "vue",
        "body": [
            "<!-- $1 -->",
            "<template>",
            "<div class='$2'>$5</div>",
            "</template>",
            "",
            "<script>",
            "//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）",
            "//例如：import 《组件名称》 from '《组件路径》';",
            "",
            "export default {",
            "//import引入的组件需要注入到对象中才能使用",
            "components: {},",
            "data() {",
            "//这里存放数据",
            "return {",
            "",
            "};",
            "},",
            "//监听属性 类似于data概念",
            "computed: {},",
            "//监控data中的数据变化",
            "watch: {},",
            "//方法集合",
            "methods: {",
            "",
            "},",
            "//生命周期 - 创建完成（可以访问当前this实例）",
            "created() {",
            "",
            "},",
            "//生命周期 - 挂载完成（可以访问DOM元素）",
            "mounted() {",
            "",
            "},",
            "beforeCreate() {}, //生命周期 - 创建之前",
            "beforeMount() {}, //生命周期 - 挂载之前",
            "beforeUpdate() {}, //生命周期 - 更新之前",
            "updated() {}, //生命周期 - 更新之后",
            "beforeDestroy() {}, //生命周期 - 销毁之前",
            "destroyed() {}, //生命周期 - 销毁完成",
            "activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发",
            "}",
            "</script>",
            "<style scoped>",
            "$4",
            "</style>"
        ],
        "description": "生成vue模板"
    },
    "http-get请求": {
	"prefix": "httpget",
	"body": [
		"this.\\$http({",
		"url: this.\\$http.adornUrl(''),",
		"method: 'get',",
		"params: this.\\$http.adornParams({})",
		"}).then(({ data }) => {",
		"})"
	],
	"description": "httpGET请求"
    },
    "http-post请求": {
	"prefix": "httppost",
	"body": [
		"this.\\$http({",
		"url: this.\\$http.adornUrl(''),",
		"method: 'post',",
		"data: this.\\$http.adornData(data, false)",
		"}).then(({ data }) => { });" 
	],
	"description": "httpPOST请求" }}
```

### 7.4 整合 Element UI

> 1、安装，在项目中执行

```vu
npm i element-ui -S
```

在package.json里可以看到新加的配置依赖

> 2、引入 Element

main.js 中引入以下内容

```vue
import ElementUI  from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css';

// 让vue使用ElementUI组件
Vue.use(ElementUI);
```



## 八、商品服务&三级分类

### 8.1 基础概念

#### 1、三级分类

<img src="image-20201017105714352.png" alt="image-20201017105714352" style="zoom:80%;" />

一级分类查出二级分类数据，二级分类中查询出三级分类数据

**数据库设计**

![image-20201017085122214](image-20201017085122214.png)

### 8.2 三级分类的实现list

#### 1、生成分类表

由sql文件生成pms_category表，代表商品的分类，表中cat_id：分类id		parent_cid：在哪个父目录下		sort：同层级同父目录下显示顺序，

#### 2、分类视图模板

开启renrenfast（有admin表） 和vue和gateway

<img src="https://img-blog.csdnimg.cn/img_convert/fbb8434bb556db0c66a04c4cc82a02a2.png" alt="img" style="zoom: 67%;" />

刷新，看到左侧多了商品系统，添加的这个菜单其实是添加到了数据库`guli-admin.sys_menu`表里

<img src="https://img-blog.csdnimg.cn/img_convert/6c67b73c8075604ff5b71aa4f95cdc43.png" alt="image-20200425164509143" style="zoom:67%;" />

`guli-admin.sys_menu`表又多了一行，父id是刚才的商品系统id

#### 3、编写查询分类代码*

```java
// 返回查询所有分类以及子子分类，以树形结构组装起来    
List<CategoryEntity> listWithTree();
```

实现类CategoryServiceImpl（在CategoryService和CategoryController也要加方法）：**baseMapper是mp源码里明确泛型之后注入的**

```java
  @Override
    public List<CategoryEntity> listWithTree() {
        // 1、查出所有分类 设置为null查询全部
        List<CategoryEntity> entities = baseMapper.selectList(null);
        // 2、组装成父子的树形结构
  List<CategoryEntity> levelList = entities.stream().filter(entity ->entity.getParentCid() == 0) // 找出父元素
       .map(menu -> { menu.setChildren(getChildrens(menu,entities));	// 设置二三级分类 递归
            return menu; }).sorted((menu1, menu2) ->     		  //  排序 Sort字段排序,防止空指针异常       
 (menu1.getSort() == null ? 0 : menu1.getSort()) - (menu2.getSort() == null ? 0 : menu2.getSort()) ).collect(Collectors.toList());
        return levelList;
    }
  /**
     *  递归查询子分类
     * @param root 当前category对象
     * @param all  全部分类数据
     */
    private List<CategoryEntity> getChildrens(CategoryEntity root, List<CategoryEntity> all) {
        List<CategoryEntity> collect = all.stream().filter(entity -> 
			entity.getParentCid() == root.getCatId();//entity对象的父类id = 等于root的分类id 说明是他的子类
        ).map(menu -> { menu.setChildren(getChildrens(menu, all)); return menu; }).sorted((menu1, menu2) -> 
(menu1.getSort() == null ? 0 : menu1.getSort()) - (menu2.getSort() == null ? 0 : menu2.getSort()) ).collect(Collectors.toList());
        return collect;
    }
```

#### 4、前后端代码修改

renren-fast代码修改：

- 填写的菜单路由是product/category（url是`http://localhost:8001/#/product-category`）

- 对应的视图是renren-fast-vue里的src/view/modules/product/category.vue

  看如何使用多级目录，文档： **https://element.eleme.cn/#/zh-CN/component/tree**

  可以通过两种方法对树节点内容自定义：`render-content`和 scoped slot（使用）。scoped slot（插槽）：在el-tree标签里把内容写到span标签栏里即可
  

在element-ui的tree中，**有2个非常重要的属性node和data：**

node代表当前结点（是否展开等信息，element-ui自带属性，默认规则）
  data是结点数据，是自己的数据。data从哪里来：前面ajax发送请求，拿到data，赋值给menus属性，而menus属性绑定到标签的data属性。

  ​	props 配置选项		children 指定子树为节点对象的某个属性值			label 指定节点标签为节点对象的某个属性值
     disabled 节点选择框是否禁用为节点对象的某个属性值						@node-click 节点被点击时的回调

  自定义我们的product/category视图的话，就是创建`mudules/product/category.vu,输入vue快捷生成模板

  ```js
  <template>
    <el-tree	:data="menus" :props="defaultProps" @node-click="handleNodeClick" ></el-tree>
  </template>
  <script>
  //这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
  //例如：import 《组件名称》 from '《组件路径》';
  export default {
    //import引入的组件需要注入到对象中才能使用
    components: {},
    props: {},
    data() {
      return {
        menus: [],
        defaultProps: {
          children: "children",
          label: "name",
        },
      };
    },
    computed: {}, 		  //监听属性 类似于data概念
    watch: {},		//监控data中的数据变化
    //方法集合
    methods: {
      handleNodeClick(data) {
        console.log(data);
      },
      getmenus() {
        this.$http({
          url: this.$http.adornUrl("/product/category/list/tree"),
          method: "get",
        }).then(({ data }) => {     		//data解构，加上{}
          console.log("成功", data.data);
          this.menus = data.data;  	 // 数组内容，把数据给menus，就是给了vue实例，最后绑定到视图上
        });
      },
    },
    //生命周期 - 创建完成（可以访问当前this实例）
    created() {
        this.getmenus();           //页面加载完后，自动调用，设置"menus"变量的值
    },
    //生命周期 - 挂载完成（可以访问DOM元素）
    mounted() {},
    beforeCreate() {}, //生命周期 - 创建之前
    beforeMount() {}, //生命周期 - 挂载之前
    beforeUpdate() {}, //生命周期 - 更新之前
    updated() {}, //生命周期 - 更新之后
    beforeDestroy() {}, //生命周期 - 销毁之前
    destroyed() {}, //生命周期 - 销毁完成
    activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
  </script>
<style scoped>
  </style>
  ```

此时要请求`localhost:8001/renren-fast/product/category/list/tree`这个url，但是报错404找不到

  他要给8001发请求读取数据，但是数据是在10000端口上，如果找到了这个请求改端口那改起来很麻烦。方法是搭建个网关，让网关路由到10000（**即将vue项目里的请求都给网关，去nacos里找到对应的微服务名，就可以找到对应的端口了，这样我们就无需管理端口。**

  在`static/config/index.js`里修改前端发来请求的路径

  ```
  window.SITE_CONFIG['baseUrl'] = 'http://localhost:88/api';
  ```

接着重新登录，现在的验证码请求路径为，http://localhost:88/api/captcha.jpg，原始的请求路径：http://localhost:8001/renren-fast/captcha.jpg

1)	让renren-fast注册到服务注册中心，这样请求88网关转发到8080fast，fast里加依赖

```
<dependency>
    <!-- 里面有nacos注册中心 -->
    <groupId>com.atguigu.gulimall</groupId>
    <artifactId>gulimall-common</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency> 
```

```
spring:
  application:
    name: renren-fast  # 把renren-fast项目也注册到nacos中
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 # nacos
```

renren-fast启动类上加上注解`@EnableDiscoveryClient`，重启，如果报错gson依赖，就导入google的gson依赖

2)	映射后的路径为localhost:8001/renren-fast/product/category/list/tre，但是只有通过localhost:10000/product/category/list/tree路径才能够正常访问。
在product项目的application.yml：(服务注册和下面的服务发现不一样，但是可以只写一次服务名)

```
 cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
```

在product项目中新建bootstrap.properties，

```
spring.application.name=gulimall-product
spring.cloud.nacos.config.server-addr=127.0.0.1:8848
spring.cloud.nacos.config.namespace=e6cd36a8-81a2-4df2-bfbc-f0524fa17664
```

为了让product注册到主类上加上注解`@EnableDiscoveryClient`

#### 5、网关路由设置*

网关配置文件（参考文档：https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/#the-rewritepath-gatewayfilter-factory）

gateway的yml中配置负载均衡（lb://)，一定写的是nacos中自定义的服务名称，并不依靠本地工程的名称去找。**（注册名称写错报503错误）**

在路由规则的顺序上，将精确的路由规则放置到模糊的路由规则的前面，否则的话，精确的路由规则将不会被匹配到，类似于异常体系中try catch子句中异常的处理顺序。

```yaml
        - id: product_route
          uri: lb://gulimall-product       	 #路由到商品
          predicates:
            - Path=/api/product/**
          filters:
            - RewritePath=/api/(?<segment>/?.*),/$\{segment}

        - id: admin_route
          uri: lb://renren-fast  # lb负载均衡     #路由到renren-fast获取验证码
          predicates:
            - Path=/api/** 		 # path指定对应路径  
          filters: 				# 重写路径
            - RewritePath=/api/(?<segment>/?.*), /renren-fast/$\{segment}
```

#### 6、跨域问题**

**跨域：指的是浏览器不能执行跨域名其他网站的脚本。浏览器对javascript/ajax请求 施加的安全限制。**

同源策略：是指**协议，域名，端口**都要相同，其中有一个不同都会产生跨域；

下图详细说明了 URL  的改变导致是否允许通信

<img src="image-20201017090210286.png" alt="image-20201017090210286" style="zoom:80%;" />

> 跨域流程

<img src="image-20201017090318165.png" alt="image-20201017090318165" style="zoom: 50%;" />

跨域请求的实现是通过预检请求实现的，**先发送一个OPSTIONS探路，收到响应允许跨域后再发送真实请求：**

![image-20201017090546193](/image-20201017090546193.png)



相关资料参考：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS

解决跨越（ 一 ）前端通过JSONP来解决，但是JSONP只可以发送GET请求。

> 解决跨越（ 二）利用nginx反向代理把跨域为不跨域，支持各种请求方式

开发过于麻烦，需要在nginx进行额外配置，语义不清晰

<img src="image-20201017090434369.png" alt="image-20201017090434369" style="zoom:80%;" />

> 解决跨域 （ 三 ）CORS在服务端设置规则控制是否允许跨域，推荐

1、添加响应头

- Access-Control-Allow-Origin: 支持哪些来源的请求跨域

- Access-Control-Allow-Methods: 支持哪些方法跨域

- Access-Control-Allow-Credentials: 跨域请求默认不包含cookie,设置为true可以包含cookie

- Access-Control-Expose-Headers: 跨域请求暴露的字段

  ​	CORS请求时, XML .HttpRequest对象的getResponseHeader()方法只能拿到6个基本字段: CacheControl、Content-L anguage、Content Type、Expires、 

  Last-Modified、 Pragma。 如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。

- Access-Control-Max- Age: 表明该响应的有效时间为多少秒。在有效时间内，浏览器无须为同一-请求再次发起预检请求。请注意，浏览器自身维护了一个最大有效时间，如果该首部字段的值超过了最大有效时间，将不会生效。

跨越设置

请求先发送到网关，网关在到nacos找服务名，可在**gateway配置文件**设置跨域过滤器，预检请求返回带上了跨域设置

把cors放在filter里，就可以优先于权限拦截器执行。

```java
@Configuration
public class GulimallCorsConfiguration {
    @Bean// 添加过滤器
    public CorsWebFilter corsWebFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();// 基于url跨域，选择reactive包下的
        CorsConfiguration corsConfiguration = new CorsConfiguration();// 跨域配置信息
        // 配置跨越
        corsConfiguration.addAllowedHeader("*"); // 允许那些头
        corsConfiguration.addAllowedMethod("*"); // 允许那些请求方式
        corsConfiguration.addAllowedOrigin("*"); //  允许请求来源,springBoot2.4以上为addAllowedOriginPattern("*");
        corsConfiguration.setAllowCredentials(true); // 是否允许携带cookie跨越
        // 注册跨越配置
        source.registerCorsConfiguration("/**",corsConfiguration);
        return new CorsWebFilter(source);
    }
}
```

就这样访问会报错，原因：不允许有多个 ‘Access-Control-Allow-Origin’ CORS 头

renren-fast中也配置了跨域设置，需要修改renren-fast项目，注释掉“io.renren.config.CorsConfig”类。然后再次进行访问。



### 8.3 逻辑删除delete

#### 1、后端

开启gateway和product和nacos

测试删除数据，打开postman输入“ http://localhost:88/api/product/category/delete ”，请求方式设置为POST，为了比对效果，可以在删除之前，查询数据库的pms_category表：

<img src="https://img-blog.csdnimg.cn/img_convert/e32a42ca2c4fc97ba9fc364c1e8b7127.png" alt="image-20200426113003531" style="zoom:80%;" />

pms_category中cat_id为1000的数据已经被删除了。但是删除前提：没有子菜单(前端)、没有被其他菜单引用（后端）

后端修改CategoryServiceImpl（在CategoryService和CategoryController也要加方法）

```
@RequestMapping("/delete")
public R delete(@RequestBody Long[] catIds){
    //		categoryService.removeByIds(Arrays.asList(catIds));//方法在mybatisplus里
    categoryService.removeMenuByIds(Arrays.asList(catIds));
    return R.ok();
}
```

```
@Override // CategoryServiceImpl 新加的方法
public void removeMenuByIds(List<Long> asList) {
    //TODO 1 检查当前的菜单是否被别的地方所引用

    baseMapper.deleteBatchIds(asList);
}
```

然而多数时候，我们并不希望删除数据，而是标记它被删除了，这就是逻辑删除（即删除转变为更新）

<img src="https://img-blog.csdnimg.cn/img_convert/70d3467fa4f9c2138c721c8c9b0a5043.png" alt="image-20200426115332899" style="zoom:67%;" />

mybatis-plus的逻辑删除,文档：https://baomidou.com/guide/logic-delete.html#

修改product.entity.CategoryEntity实体类，添加上@TableLogic，表明使用逻辑删除：

```
	/**
	 * 是否显示[0-不显示，1显示]
	 */
	@TableLogic(value = "1",delval = "0")
	private Integer showStatus;
```

另外在“src/main/resources/application.yml”文件中，设置日志级别，打印出SQL语句：

```
logging:
  level:
    com.example.gulimall.product: debug
```

然后在POSTMan中测试一下是否能够满足需要。

#### 2、前端

1、发送的请求：delete；	发送的数据：this.$http.adornData(ids, false)				查看/src/utils/httpRequest.js中，封装了一些拦截器：

```
/**
 * get请求参数处理
 * ajax的get请求会被缓存，就不会请求服务器了。所以我们在url后面拼接个时间戳（使每个url不一致），让他每次都请求服务器
 */
http.adornParams = (params = {}, openDefultParams = true) => {
  var defaults = {
    't': new Date().getTime()
  }
  return openDefultParams ? merge(defaults, params) : params
}

/**
 * post请求数据处理
 * @param {*} data 数据对象
 * @param {*} openDefultdata 是否开启默认数据?
 * @param {*} contentType 数据格式
 */
http.adornData = (data = {}, openDefultdata = true, contentType = 'json') => {
  var defaults = {
    't': new Date().getTime()
  }
  data = openDefultdata ? merge(defaults, data) : data
  return contentType === 'json' ? JSON.stringify(data) : qs.stringify(data)
}
```

2、抽取代码片段 "http-get请求"， "http-post请求"到vue.json，前面已经做了

3、修改mudules/product/category.vu，文档： https://element.eleme.cn/#/zh-CN/component	(message)

```js
<template>
<el-tree
         :data="menus" 
         :props="defaultProps"  
         :expand-on-click-node="false" 			//只有点击箭头才会展开收缩
         node-key="catId"   					//设置唯一键
		 show-checkbox  						//显示复选框
		 :default-expanded-keys="expandedKey" >   //默认展开 
    <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
            <el-button v-if="node.level <= 2"   //非叶结点
                       type="text" 
                       size="mini" 
                       @click="() => append(data)">  	Append
            </el-button>
            <el-button  							//叶子结点
                       v-if="node.childNodes.length == 0"
                       type="text"
                       size="mini"
                       @click="() => remove(node, data)">  Delete
            </el-button>
        </span>
    </span>
</el-tree>
</template>
<script>
export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  props: {},
  data() {
    return {
      menus: [],
      expandedKey: [],
      defaultProps: {
        children: "children",
        label: "name",
      },
    };
  },
  //监听属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    getMenus() {
      this.$http({
        url: this.$http.adornUrl("/product/category/list/tree"),
        method: "get",
      }).then(({ data }) => {
        console.log("成功", data.data);
        this.menus = data.data;
      });
    },
    remove(node, data) {
      var ids = [data.catId];
        // 删除时弹窗确认
      this.$confirm(`是否删除【${data.name}】菜单?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => { 
          this.$http({
            url: this.$http.adornUrl("/product/category/delete"),
            method: "post",
            data: this.$http.adornData(ids, false)
          }).then(({ data }) => {
              // 删除成功弹窗
            this.$message({
              message: "菜单删除成功",
              type: "success"
            });
            //刷新出新的菜单
            this.getMenus();
            //删除后重新展开父节点
            this.expandedKey = [node.parent.data.catId];
          });
        }) .catch(
        () => {});
        console.log("remove",node,data);   //控制台打印删除节点信息
    },
```

UPDATE pms_category SET show_status=1，删除后可用sql语句在sqlyog里重新修改回来



### 8.4 新增&修改&拖拽&批量

#### 1、前端

文档： https://element.eleme.cn/#/zh-CN/component	(dialog/form)

**1、新增，dialog对话框**

前端用到的组件：Dialog 对话框、可拖拽节点和Switch 开关、批量操作Button 按钮、Tree组件

```vue
 <template>
  <div>
    <el-switch v-model="draggable" active-text="开启拖拽" inactive-text="关闭拖拽"></el-switch>      //防止误操作，
    <el-button v-if="draggable" @click="batchSave">批量保存</el-button>
    <el-button type="danger" @click="batchDelete">批量删除</el-button>
    <el-tree
      :data="menus"
      :props="defaultProps"                      
      :expand-on-click-node="false" 			//只有点击箭头才会展开收缩
       node-key="catId"   					   //设置唯一键
       show-checkbox  						  //显示复选框
       :default-expanded-keys="expandedKey" >    //指定展开的内容
      :draggable="draggable"					//开关拖拽功能，和批量保存绑定
      :allow-drop="allowDrop"					//是否允许拖拽到目标结点的函数					
      @node-drop="handleDrop"					//拖拽成功处理函数
      ref="menuTree"							//给批量选择的组件设置标志，给batchDelete()引用
    > 
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
 <el-button  v-if="node.level <=2" type="text" size="mini" @click="() => append(data)">Append</el-button>    //非叶结点，添加
 <el-button type="text" size="mini" @click="edit(data)">edit</el-button>										//修改
 <el-button v-if="node.childNodes.length==0" type="text" size="mini" @click="() => remove(node, data)">Delete</el-button>   //叶子结点，删除
        </span>
      </span>
    </el-tree>
  <!--弹出对话框，显示现有内容-->   
<el-dialog :title="title" :visible.sync="dialogVisible" width="30%" :close-on-click-modal="false"><!--点击框不关 :变布尔类型-->
      <el-form :model="category">
        <el-form-item label="分类名称">
          <el-input v-model="category.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="图标">
          <el-input v-model="category.icon" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="计量单位">
          <el-input v-model="category.productUnit" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitData">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  props: {},
  data() {
    return {
      pCid: [],        			//批量删除的catid数组
      draggable: false,			//默认不拖拽
      updateNodes: [],			//更新sort和catLevel(和preId)的节点数组，handleDrop（）方法
      maxLevel: 0,				//当前拖拽节点子节点的最大层级，countNodeLevel()方法
      title: "",						//对话框标题：修改/添加分类，append（）和edit（）
      dialogType: "", //edit,add      //提交方法的时根据变量赋值决定调用那个方法, submitData()
      category: {name: "",parentCid: 0,catLevel: 0,showStatus: 1,sort: 0,productUnit: "",icon: "",catId: null},
      dialogVisible: false,      //点击确认或者取消后的逻辑都是关闭会话，默认为关闭
      menus: [],					//菜单数组
      expandedKey: [],				//指定展开的内容数组
      defaultProps: { children: "children",label: "name",},   //菜单上的属性
    };
  },
  //监听属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    getMenus() {    //显示功能
      this.$http({
        url: this.$http.adornUrl("/product/category/list/tree"),
        method: "get",
      }).then(({ data }) => {
        console.log("查询成功", data.data);
        this.menus = data.data;
      });
    },
      
      //批处理功能
    batchDelete() {			
      let catIds = [];
      let checkedNodes = this.$refs.menuTree.getCheckedNodes();  //默认得到被全选中的元素数组，不含半选的
      console.log("被选中的元素", checkedNodes);
      for (let i = 0; i < checkedNodes.length; i++) {
        catIds.push(checkedNodes[i].catId);       //批量删除的元素的catid数组
      }
      this.$confirm(`是否批量删除【${catIds}】菜单?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          this.$http({
            url: this.$http.adornUrl("/product/category/delete"),  
            method: "post",
            data: this.$http.adornData(catIds, false)
          }).then(({ data }) => {
            this.$message({
              message: "菜单批量删除成功",
              type: "success"
            });
            this.getMenus();
          });
        }) .catch(() => {}); 
    },
      
  //拖拽功能和拖拽后的批量保存
    batchSave() {
      this.$http({
        url: this.$http.adornUrl("/product/category/update/sort"),
        method: "post",
        data: this.$http.adornData(this.updateNodes, false)
      }).then(({ data }) => {
        this.$message({
          message: "菜单顺序等修改成功",
          type: "success"
        });
        //刷新出新的菜单
        this.getMenus();
        //设置需要默认展开的菜单
        this.expandedKey = this.pCid;     //使用全局变量数组
        this.updateNodes = [];    //修改完一次要置空
        this.maxLevel = 0;        
      });
    },
    handleDrop(draggingNode, dropNode, dropType, ev) {   //当前拖拽节点，参考节点，before/after/inner，ev不用管
      console.log("handleDrop: ", draggingNode, dropNode, dropType); 
      let pCid = 0;   //1、当前节点最新的父节点id
      let siblings = null;   //兄弟节点
      if (dropType == "before" || dropType == "after") {   //和参考节点同级
        pCid = dropNode.parent.data.catId == undefined?0:dropNode.parent.data.catId;  //根参考节点没parent
        siblings = dropNode.parent.childNodes;
      } else {       //在参考节点下
        pCid = dropNode.data.catId;
        siblings = dropNode.childNodes;
      }
      this.pCid.push(pCid);   //注入全局变量中
      //2、对当前拖拽节点和其兄弟节点排序，修改层级和当前拖拽节点的父节点id
      for (let i = 0; i < siblings.length; i++) {
        if (siblings[i].data.catId == draggingNode.data.catId) {   //如果遍历的是当前正在拖拽的节点 
          let catLevel = draggingNode.level;
          if (siblings[i].level != draggingNode.level) {
            //当前节点的层级发生变化
            catLevel = siblings[i].level;
            //修改他子节点的层级
            this.updateChildNodeLevel(siblings[i]);
          }
          this.updateNodes.push({catId: siblings[i].data.catId,sort: i,parentCid: pCid,catLevel: catLevel });
        } else {
          this.updateNodes.push({ catId: siblings[i].data.catId, sort: i });
        }
      }
      console.log("updateNodes", this.updateNodes);    //3、当前拖拽节点的最新层级
    },
    updateChildNodeLevel(node) {
      if (node.childNodes.length > 0) {
        for (let i = 0; i < node.childNodes.length; i++) {
          this.updateNodes.push({
            catId: node.childNodes[i].data.catId,  //数据库中的要有data
            catLevel: node.childNodes[i].level     //不是数据库中的，真实的
          });
          this.updateChildNodeLevel(node.childNodes[i]);   //递归
        }
      }
    },  
    allowDrop(draggingNode, dropNode, type) {
      //当前正在拖动节点向下还有的层数+目标节点层级不大于3即可
      //1）、（当前节点层级，目标节点层级，inner/pre/next）
      console.log("allowDrop:", draggingNode, dropNode, type);
        this.maxLevel = 0;     //每次调用前置为0
      this.countNodeLevel(draggingNode);
      //当前正在拖动的节点层数
      let deep =Math.abs(this.maxLevel - draggingNode.level） + 1;
      console.log("深度：", deep);
      //   this.maxLevel
      if (type == "inner") {
        return deep + dropNode.level <= 3;
      } else {
        return deep + dropNode.parent.level <= 3;
      }
    },
    countNodeLevel(node) {
      //找到所有子节点，求出最大深度，childNodes为数组
      if (node.childNodes != null && node.childNodes.length > 0) {
        for (let i = 0; i < node.childNodes.length; i++) {
          if (node.childNodes[i].level > this.maxLevel) {    //不要用数据库的catlevel
            this.maxLevel = node.childNodes[i].level;
          }
          this.countNodeLevel(node.childNodes[i]);
        }
      }else{this.maxLevel=node.level;}
    },
      
      //增改功能
    edit(d) {
      console.log("要修改的数据", d);
      this.dialogType = "edit";
      this.title = "修改分类";
      this.dialogVisible = true;
      //后端停了还是会有数据，回显时要获取最新数据,故发送请求直接查数据库当前节点最新的数据
      this.$http({
        url: this.$http.adornUrl(`/product/category/info/${d.catId}`),    //getById方法
        method: "get"
      }).then(({ data }) => {
        //请求成功
        console.log("要回显的数据", data);
      this.category = data.category;  //全部替换，不然没有更新的字段会用默认值替换了，category为controller里的键
      });
    },
     append(data) {
      console.log("append", data);
      this.dialogType = "add";
      this.title = "添加分类";
      this.dialogVisible = true;
      this.category.parentCid = data.catId;
      this.category.catLevel = data.catLevel * 1 + 1;
      this.category.catId = null;     
      this.category.name = "";      //edit后的更新
      this.category.icon = ""; 
      this.category.productUnit = "";
      this.category.sort = 0;
      this.category.showStatus = 1;
    },
   submitData() {       //在提交方法的时根据变量赋值决定调用那个方法
      if (this.dialogType == "add") {
        this.addCategory();
      }
      if (this.dialogType == "edit") {
        this.editCategory();
      }
    },
    //修改三级分类数据
    editCategory() {
      var { catId, name, icon, productUnit } = this.category;     //解构
      this.$http({
        url: this.$http.adornUrl("/product/category/update"),
        method: "post",
        data: this.$http.adornData({ catId, name, icon, productUnit }, false)  //对象数据，对象的属性名和属性值一样简写
      }).then(({ data }) => {
        this.$message({
          message: "菜单修改成功",
          type: "success"
        });
        //关闭对话框
        this.dialogVisible = false;
        //刷新出新的菜单
        this.getMenus();
        //设置需要默认展开的菜单
        this.expandedKey = [this.category.parentCid];
      });
    },   
    //添加三级分类
    addCategory() {
      console.log("提交的三级分类数据", this.category);
      this.$http({
        url: this.$http.adornUrl("/product/category/save"),
        method: "post",
        data: this.$http.adornData(this.category, false)
      }).then(({ data }) => {
        this.$message({
          message: "菜单保存成功",
          type: "success"
        });
        //关闭对话框
        this.dialogVisible = false;
        //刷新出新的菜单
        this.getMenus();
        //设置需要默认展开的菜单
        this.expandedKey = [this.category.parentCid];
      });
    },
```

#### 2、后端

批量保存后端,添加方法：

```java
 //批量保存
@RequestMapping("/update/sort")
//springmvc自动将json数组修改为数组
public R updateSort(@RequestBody CategoryEntity[] category){  //调整为数组
    categoryService.updateBatchById(Arrays.asList(category));
    return R.ok();
}
```

#### 3、总结

效果演示

<img src="image-20201018093124893.png" alt="image-20201018093124893" style="zoom:80%;" />



**总结：**

开启/关闭拖拽：

通过 draggable 属性可让节点变为可拖拽。

![image-20201018115219350](image-20201018115219350.png)

<img src="image-20201018115326664.png" alt="image-20201018115326664" style="zoom:80%;" />





## 九、商品服务&品牌管理

### 9.1、新增品牌管理

开启renrenfast（有admin表）和vue和gateway，后台：系统管理/菜单管理/新增

<img src="https://fermhan.oss-cn-qingdao.aliyuncs.com/guli/image-20200428164054517.png" alt="image-20200428164054517" style="zoom:50%;" />

将逆向工程product得到的resources\src\views\modules\product下面的两个文件拷贝到gulimall/renren-fast-vue/src/views/modules/product目录下，即

brand.vue ： 显示的表单		brand-add-or-update.vue：添加和更改功能
开启product，查看新增的品牌管理，但是显示的页面没有新增和删除功能，这是因为权限控制的原因

<img src="https://img-blog.csdnimg.cn/img_convert/eda83774b0cd28eb5d756d601080ed73.png" alt="image-20200428170644511" style="zoom:50%;" />

查看“isAuth”的定义位置：它是在“index.js”中定义，将它设置为返回值为true，即可显示添加和删除功能

<img src="/image-20210609140847124.png" alt="image-20210609140847124" style="zoom:80%;" />

build/webpack.base.conf.js 中注释掉createLintingRule()函数体，不进行lint语法检查，启动vue就不会警告了

### 9.2、快速显示开关

![image-20201019201328358](image-20201019201328358.png)

文档：https://element.eleme.cn/#/zh-CN/component/switch

brand.vue在显示状态下面：（brand-add-or-update只用el-switch）

```vue
  <template slot-scope="scope">   //用插槽，的整行数据
          <el-switch
            v-model="scope.row.showStatus"
            active-color="#13ce66"
            inactive-color="#ff4949"
            :active-value="1"
            :inactive-value="0"
            @change="updateBrandStatus(scope.row)"
          ></el-switch>
</template>
 <!-- 
active-color	switch 打开时的背景色
inactive-color	switch 关闭时的背景色
active-value	switch 打开时的值
inactive-value	switch 关闭时的值
@change			得到开关的状态  scope.row 拿到整行的数据-->
```

用户点击 switch 开关就会调用后台的接口更改对应数据库的字段 ( 决定是否显示)，定义了 @change 事件 只要修后就会触发对应方法

```javascript
 updateBrandStatus(data) {
      console.log("整行数据",data);
     // 单独就封装两个字段
      let {brandId,showStatus} = data
      this.$http({
        url: this.$http.adornUrl('/product/brand/update'),
        method: 'post',
        data: this.$http.adornData({brandId,showStatus}, false)
      }).then(({ data }) => {
        this.$message({
          type:"success",
          message:"状态更新成功"
        })
       });
    },
```



### 9.3、表单自定义

#### 1、阿里云上传

和传统的单体应用不同，这里我们选择将数据上传到分布式文件服务器上。这里我们选择将图片放置到阿里云上，使用**对象存储（oss）**。

<img src="https://img-blog.csdnimg.cn/img_convert/48f62d8e0427a5f730c75039f9c68bae.png" alt="image-20200428182755992" style="zoom:67%;" />

<img src="https://fermhan.oss-cn-qingdao.aliyuncs.com/img/20210214120736.png" alt="img" style="zoom:80%;" />

<img src="https://fermhan.oss-cn-qingdao.aliyuncs.com/img/20210214121759.png" alt="img" style="zoom:67%;" />

这种方式是手动上传图片，实际上我们在程序中设置自动上传图片到阿里云对象存储。

- 上传先找应用服务器要一个policy上传策略，生成防伪签名

<img src="https://fermhan.oss-cn-qingdao.aliyuncs.com/guli/image-20200428184029655.png" alt="image-20200428184029655" style="zoom:50%;" />

#### 2、代码

##### 1）获得上传地址和权限

endpoint的取值：点击概览就可以看到你的endpoint信息

accessKeyId和accessKeySecret需要创建一个RAM账号：

<img src="https://img-blog.csdnimg.cn/img_convert/31c6f1b6bd5fe4638416c400a97ad384.png" alt="image-20200428190532924" style="zoom: 67%;" />

- 选上`编程访问`

创建用户完毕后，会得到一个“AccessKey ID”和“AccessKeySecret”，然后复制这两个值到代码的“AccessKey ID”和“AccessKeySecret”，**关闭页面后就找不到了**。

另外还需要添加访问控制权限：

<img src="https://img-blog.csdnimg.cn/img_convert/3ab3813c7d97e03b00e00423447fd19e.png" alt="image-20200428191518591" style="zoom: 67%;" />

##### 2）后端代码

对象存储页面可以查看阿里云关于文件上传的api帮助文档

若将上传任务都交给gulimall-product显然**耦合度高**。代替：新建一个Module来完成文件上传任务，且可使用**SpringCloud Alibaba来**管理oss。

新建gulimall-third-party微服务,pom依赖：

```
<artifactId>gulimall-third-party</artifactId>
    <description>第三方服务</description>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alicloud-oss</artifactId>
            <version>2.2.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>com.example.gulimall</groupId>
            <artifactId>gulimall-common</artifactId>
            <version>0.0.1-SNAPSHOT</version>
            <exclusions>
                <exclusion>
                    <groupId>com.baomidou</groupId>
                    <artifactId>mybatis-plus-boot-starter</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
    <!--    把依赖的包以及本工程代码都打进一个可执行的jar/war包。-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

bootstrap.properties:配置中心，可以把**对象存储（oss）**配置到配置中心里，开发阶段先不配了

```
spring.application.name=gulimall-third-party
spring.cloud.nacos.config.server-addr=localhost:8848
spring.cloud.nacos.config.namespace=
```

application.yml：配置名在bootstrap中配好了

```
spring:
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    alicloud:
      access-key: LTAI5tFyQNKg6Dj4FU62461v
      secret-key: kvP8Z1DM6PV9OqvoWPp9takckHhfq7
      oss:
        endpoint: oss-cn-shenzhen.aliyuncs.com
        bucket: gulimall77
server:
  port: 30000
```

主启动类@EnableDiscoveryClient开启服务的注册和发现

编写测试类：

```
@Autowired
    OSSClient ossClient;
@Test
public void testUpload() throws FileNotFoundException {
// 上传文件流。不能有中文名
    InputStream inputStream = new FileInputStream("E:\\languages2\\project\\5b5e74d0978360a1.jpg");
    ossClient.putObject("gulimall77", "a2.jpg", inputStream);
// 关闭OSSClient。
    ossClient.shutdown();
    System.out.println("上传完成");}
```

在“gulimall-gateway”中配置路由规则（不能对外暴露端口或者说为了统一管理）：

```
- id: third_party_route
            uri: lb://gulimall-third-party
            predicates:
              - Path=/api/thirdparty/**
            filters:
              - RewritePath=/api/thirdparty/(?<segment>/?.*),/$\{segment}
```

编写“com.atguigu.gulimall.controller.`OssController`”类：

```java
@RestController
public class OssController {
    @Autowired
    OSS ossClient;  //源码按接口oss注入，不能写实现类了
    @Value ("${spring.cloud.alicloud.oss.endpoint}")
    String endpoint ;
    @Value("${spring.cloud.alicloud.oss.bucket}")
    String bucket ;
    @Value("${spring.cloud.alicloud.access-key}")
    String accessId ;
    @Value("${spring.cloud.alicloud.secret-key}")
    String accessKey;
    @RequestMapping("/oss/policy")
    public R policy(){
       String host = "https://" + bucket + "." + endpoint; // host的格式为 bucketname.endpoint
       String format = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
       String dir = format+"/"; // 用户上传文件时指定的前缀。前端的拼接路径就不要加/了，不然多一层目录
        Map<String, String> respMap=null;
        try {
            // 签名有效事件
            long expireTime = 300;     //密钥超过30秒就过期
            long expireEndTime = System.currentTimeMillis() + expireTime * 1000;
            Date expiration = new Date(expireEndTime);

            PolicyConditions policyConds = new PolicyConditions();
            policyConds.addConditionItem(PolicyConditions.COND_CONTENT_LENGTH_RANGE, 0, 1048576000);
            policyConds.addConditionItem(MatchMode.StartWith, PolicyConditions.COND_KEY, dir);

            String postPolicy = ossClient.generatePostPolicy(expiration, policyConds);
            byte[] binaryData = postPolicy.getBytes("utf-8");
            String encodedPolicy = BinaryUtil.toBase64String(binaryData);
            // 签名
            String postSignature = ossClient.calculatePostSignature(postPolicy);

            respMap= new LinkedHashMap<String, String>();
            respMap.put("accessid", accessId);
            respMap.put("policy", encodedPolicy);
            respMap.put("signature", postSignature);
            respMap.put("dir", dir);
            respMap.put("host", host);
            respMap.put("expire", String.valueOf(expireEndTime / 1000));
        } catch (Exception e) {
            // Assert.fail(e.getMessage());
            System.out.println(e.getMessage());
        } finally {
            ossClient.shutdown();
        }
        return R.ok().put("data",respMap);//前端的对应取数据就要加一个data，取出键值对里的map
    }
}
```

##### 3）前端代码

我们需要一些前端代码，通过url请求得到一个policy签名，要拿这个东西直接传到阿里云，字节流就别来服务器了

放置项目提供的upload文件夹到renren-fast-vue\src\components\upload目录下，一个是单文件上传，另外一个是多文件上传

policy.js封装一个Promise，发送/thirdparty/oss/policy请求。会自动加上api前缀，ultiUpload.vue多文件上传。singleUpload.vue单文件上传。要替换里面的action中的内容。

用的为singleUpload.vue：

```
 beforeUpload(file) {
        let _self = this;
        return new Promise((resolve, reject) => {
          policy().then(response => {
            console.log("响应的数据",response);
            _self.dataObj.policy = response.data.policy;      		 //有data，后端要传R对象而不是一个map
            _self.dataObj.signature = response.data.signature;
            _self.dataObj.ossaccessKeyId = response.data.accessid;
            _self.dataObj.key = response.data.dir +getUUID()+'_${filename}';    //路径拼接和后端连起来
```

brand-add-or-update.vue中：

```
  <single-upload v-model="dataForm.logo"></single-upload>
<script>
import SingleUpload from "@/components/upload/singleUpload";
export default {
  components: { SingleUpload },
  data() {
    return {
      visible: false,
      dataForm: {
        brandId: 0,
        name: "",
        logo: "",
        descript: "",
        showStatus: 1,
        firstLetter: "",
        sort: 0
      },
```

服务去请求oss服务，我们前面说过了，跨域不是浏览器限制了你，而是新的服务器限制的问题，所以得去阿里云设置，概览->基础设置->设置

![img](https://img-blog.csdnimg.cn/img_convert/0105ae7146c8205270e53a8b8057140e.png)

跨域，第一次opstions预检请求，第二次才真正访问带数据

![image-20210610000903956](/image-20210610000903956.png)

优化：上传后显示图片而不是地址，文档找到：表单-自定义列，在品牌logo地址之间插入：

```
<el-table-column prop="logo" header-align="center" align="center" label="品牌logo地址">
        <template slot-scope="scope">
          <img :src="scope.row.logo" style="width: 100px; height: 80px" />
        </template>
 </el-table-column>
```

修改vue项目的element-ui脚手架的问题，没有导入src/element-ui/index.js里的image组件，去文档复制粘贴即可

### 9.4、校验 &异常处理*

#### 1、前端校验

**Form 组件提供了表单验证的功能，只需要通过 `rules` 属性传入约定的验证规则，**并将 Form-Item 的 `prop` 属性设置为需校验的字段名即可

<img src="image-20201019204335899.png" alt="image-20201019204335899" style="zoom:80%;" />

```vue
<el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      @keyup.enter.native="dataFormSubmit()"
      label-width="140px"
    >
```

data中自定义的规则 ，用来对数据进行判断

```javascript
dataRule: {
        name: [{ required: true, message: "品牌名不能为空", trigger: "blur" }],
        // 自定义的规则  
        firstLetter: [
          { validator: (rule, value, callback) => {
            if(value == '') {
              callback(new Error("首字母必须填写"))
            } else if(! /^[a-zA-Z]$/.test(value)) {
              callback(new Error("首字母必须a-z或者A-Z之间"))
            } else {
              callback()
            }
          },trigger:'blur'}
        ],
        sort: [{ validator: (rule, value, callback) => {
            if(value == '') {
              callback(new Error("排序字段必须填写"));
            } else if(!Number.isInteger(value) || value < 0) {
              callback(new Error("排序必须是一个大于等于0的整数"));
            } else {
              callback();
            }
          }, trigger: "blur" }]
      }
```

#### 2、后端 JSR303校验

前端数据校验成功了，就会把json数据传递到后端，但是有人利用接口 比如 postman 乱发送请求 那会怎么办，于是**后端也会利用 JSR303进行数据校验**

新版本需要validation启动器，在common的pom文件加入依赖：

```
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

在Java中提供了一系列的校验方式，在“`javax.validation.constraints`”包中

```
javax.validation.constraints.AssertFalse.message     = 只能为false
javax.validation.constraints.AssertTrue.message      = 只能为true
javax.validation.constraints.DecimalMax.message      = 必须小于或等于{value}
javax.validation.constraints.DecimalMin.message      = 必须大于或等于{value}
javax.validation.constraints.Digits.message          = 数字的值超出了允许范围(只允许在{integer}位整数和{fraction}位小数范围内)
javax.validation.constraints.Email.message           = 不是一个合法的电子邮件地址
javax.validation.constraints.Future.message          = 需要是一个将来的时间
javax.validation.constraints.FutureOrPresent.message = 需要是一个将来或现在的时间
javax.validation.constraints.Max.message             = 最大不能超过{value}
javax.validation.constraints.Min.message             = 最小不能小于{value}
javax.validation.constraints.Negative.message        = 必须是负数
javax.validation.constraints.NegativeOrZero.message  = 必须是负数或零
javax.validation.constraints.NotBlank.message        = 不能为空
javax.validation.constraints.NotEmpty.message        = 不能为空
javax.validation.constraints.NotNull.message         = 不能为null
javax.validation.constraints.Null.message            = 必须为null
javax.validation.constraints.Past.message            = 需要是一个过去的时间
javax.validation.constraints.PastOrPresent.message   = 需要是一个过去或现在的时间
javax.validation.constraints.Pattern.message         = 需要匹配正则表达式"{regexp}"
javax.validation.constraints.Positive.message        = 必须是正数
javax.validation.constraints.PositiveOrZero.message  = 必须是正数或零
javax.validation.constraints.Size.message            = 个数必须在{min}和{max}之间

org.hibernate.validator.constraints.CreditCardNumber.message        = 不合法的信用卡号码
org.hibernate.validator.constraints.Currency.message                = 不合法的货币 (必须是{value}其中之一)
org.hibernate.validator.constraints.EAN.message                     = 不合法的{type}条形码
org.hibernate.validator.constraints.Email.message                   = 不是一个合法的电子邮件地址
org.hibernate.validator.constraints.Length.message                  = 长度需要在{min}和{max}之间
org.hibernate.validator.constraints.CodePointLength.message         = 长度需要在{min}和{max}之间
org.hibernate.validator.constraints.LuhnCheck.message               = ${validatedValue}的校验码不合法, Luhn模10校验和不匹配
org.hibernate.validator.constraints.Mod10Check.message              = ${validatedValue}的校验码不合法, 模10校验和不匹配
org.hibernate.validator.constraints.Mod11Check.message              = ${validatedValue}的校验码不合法, 模11校验和不匹配
org.hibernate.validator.constraints.ModCheck.message                = ${validatedValue}的校验码不合法, ${modType}校验和不匹配
org.hibernate.validator.constraints.NotBlank.message                = 不能为空
org.hibernate.validator.constraints.NotEmpty.message                = 不能为空
org.hibernate.validator.constraints.ParametersScriptAssert.message  = 执行脚本表达式"{script}"没有返回期望结果
org.hibernate.validator.constraints.Range.message                   = 需要在{min}和{max}之间
org.hibernate.validator.constraints.SafeHtml.message                = 可能有不安全的HTML内容
org.hibernate.validator.constraints.ScriptAssert.message            = 执行脚本表达式"{script}"没有返回期望结果
org.hibernate.validator.constraints.URL.message                     = 需要是一个合法的URL

org.hibernate.validator.constraints.time.DurationMax.message        = 必须小于${inclusive == true ? '或等于' : ''}${days == 0 ? '' : days += '天'}${hours == 0 ? '' : hours += '小时'}${minutes == 0 ? '' : minutes += '分钟'}${seconds == 0 ? '' : seconds += '秒'}${millis == 0 ? '' : millis += '毫秒'}${nanos == 0 ? '' : nanos += '纳秒'}
org.hibernate.validator.constraints.time.DurationMin.message        = 必须大于${inclusive == true ? '或等于' : ''}${days == 0 ? '' : days += '天'}${hours == 0 ? '' : hours += '小时'}${minutes == 0 ? '' : minutes += '分钟'}${seconds == 0 ? '' : seconds += '秒'}${millis == 0 ? '' : millis += '毫秒'}${nanos == 0 ? '' : nanos += '纳秒'}
```

1、给product里brand的Bean添加校验注解并定义自己的的message提示，不加message就是默认的

```java
@NotNull 								//该属性不能为null
@NotEmpty(messsage = "logo不能为空") 	//该字段不能为null或""    
@NotBlank								//不能为空，不能仅为一个空格
```

2、开启校验功能 @Valid（加了才会起作用），效果：校验错误以后有默认的响应，Controller代码：

```java
@RequestMapping("/save")
public R save(@Valid@RequestBody BrandEntity brand){
    brandService.save(brand);
    return R.ok();
}
```

3、**分组校验** (多场景复杂校验)

如果新增和修改需要验证的字段不同，比如id字段，新增可以不传递，但是修改必须传递id，不可能写两个vo来满足不同的校验规则。所以就需要用到分组校验来实现。

在common下建一个valid包，创建分组接口UpdateGroup， AddGroup，不用写方法，只起标注作用

在controller加了分组注解的情况下，Entity里没有指定分组的校验注解，默认是不起作用的。想要起作用就必须要加groups。

```java
/**
 * 品牌id
 */
@Null(message = "新增不能指定Id",groups = {AddGroup.class})
@NotNull(message = "修改必须指定品牌id",groups = {UpdateGroup.class})
@TableId
private Long brandId;
/**
 * 品牌名
 */
@NotBlank(message = "品牌名不能为空",groups = {AddGroup.class,UpdateGroup.class})
private String name;
/**
 * 品牌logo地址
 */
@NotEmpty(groups = {AddGroup.class})
@URL(message = "logo必须是一个合法的url地址",groups = {AddGroup.class,UpdateGroup.class}) //必须指定组要不然不起作用
private String logo;
/**
 * 介绍
 */
private String descript;
/**
 * 显示状态[0-不显示；1-显示]
 */
@NotNull(groups = {AddGroup.class, UpdateGroup.class})
@ListValue(vals = {0,1}, groups = {AddGroup.class, UpdateGroup.class})
private Integer showStatus;
/**
 * 检索首字母
 */
@NotEmpty(groups = {AddGroup.class})
@Pattern(regexp = "^[a-zA-Z]$",message = "检索首字母必须是一个字母",groups = {AddGroup.class,UpdateGroup.class})
private String firstLetter;
/**
 * 排序
 */
@NotNull(groups = {AddGroup.class})
@Min(value=0,message = "排序必须大于等于0",groups = {AddGroup.class,UpdateGroup.class})
private Integer sort;
```

controller的方法上或者方法参数上写要处理的分组的接口信息:

```
@RequestMapping("/save")
    //@RequiresPermissions("product:brand:save")
    public R save(@Validated({AddGroup.class}) @RequestBody BrandEntity brand){
        brandService.save(brand);
        return R.ok();
    }
@RequestMapping("/update")
public R update(@Validated(UpdateGroup.class) @RequestBody BrandEntity brand){
	brandService.updateById(brand);
      return R.ok();
  }
```

4、**自定义校验**

给private Integer showStatus属性设置**自定义校验**，在common/vaild包下编写一个自定义的校验注解

```java
@Documented
@Constraint(validatedBy = {ListValueConstraintValidator.class})//【可以指定多个不同的校验器，自动适配不同类型的校验】
@Target({ METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE })//可以标注在那些地方
@Retention(RUNTIME)   //校验时机
public @interface ListValue {
    // 参考别的注解，必须有3个属性，message()错误信息去哪里去取，groups()分组校验，payload()自定义负载信息
    String message() default "{com.example.gulimall.product.valid.ListValue.message}"; 

    Class<?>[] groups() default { };

    Class<? extends Payload>[] payload() default { };

    int[] vals() default { };
}
```

message值对应的字符串需要去配置文件中获得（也可写在注解中），所以在common/resource下建ValidationMessages.properties自定义异常消息，此时idea要设置为UTF-8

```
com.example.gulimall.product.valid.ListValue.message=必须提交指定的值 [0,1]
```

<img src="/image-20210610151021493.png" alt="image-20210610151021493" style="zoom:50%;" />

上面只是定义了异常消息和注解，在valid包下定义一个ConstraintValidator校验器类验证是否异常

那么就通过重写initialize()方法，限定某个属性值可以有哪些元素。而controller接收到的数据用isValid()方法验证

```java
public class ListValueConstraintValidator implements ConstraintValidator<ListValue,Integer> {   //<注解,校验值类型>   可以自动适配
    private Set<Integer> set = new HashSet<>();     // 存储所有可能的值
    // 初始化方法，可以获取注解上的内容并进行处理
    @Override
    public void initialize(ListValue constraintAnnotation) {
        int[] vals = constraintAnnotation.vals();   // 获取写好的限制
        for(int val : vals) {
            // 将结果添加到set集合
            set.add(val);
        }
    }
    /**
     *	判断校验是否成功
     * @param value 需要校验的值
     */
    @Override
    public boolean isValid(Integer value, ConstraintValidatorContext context) {
        // 判断是包含该值
        return set.contains(value);
    }
}
```

修改view/module/porduct/brand：带上name和brand则改的时候可以满足要带上属性的要求，可以不用在后端新加一个分组（接口）和controller方法

```
updateBrandStatus(data) {
      console.log("最新信息", data);
      let { name,brandId, showStatus } = data;
      //发送请求修改状态
      this.$http({
        url: this.$http.adornUrl("/product/brand/update"),
        method: "post",
        data: this.$http.adornData({ name,brandId, showStatus }, false)
      }).then(({ data }) => {
```

#### 4、异常处理 

##### 1、局部异常处理

在postman种发送http://localhost:10000/product/brand/save请求，可以看到返回的甚至不是R对象，在controller对应方法上加BindResult，

```java
 public R save(@Valid @RequestBody BrandEntity brand,BindingResult result){
  if(result.hasErrors()){
            Map<String,String> map = new HashMap<>();
            //1、获取校验的错误结果
            result.getFieldErrors().forEach((item)->{
                //FieldError 获取到错误提示
                String message = item.getDefaultMessage();
                //获取错误的属性的名字
                String field = item.getField();
                map.put(field,message);
            });
            return R.error(400,"提交的数据不合法").put("data",map);
        }else {
        }
```

<img src="/image-20210610104102979.png" alt="image-20210610104102979" style="zoom: 80%;" />

这种是针对于该请求设置了一个内容校验，如果针对于每个请求都单独进行配置，显然不是太合适，实际上可以统一的对于异常进行处理。

##### 2、全局异常处理

1、**编写异常处理类使用SpringMvc的@ControllerAdvice**

 2、**使用@ExceptionHandler标记方法可以处理异常**

```java
@Slf4j
@RestControllerAdvice(basePackages =  "com.example.gulimall.product.controller")
public class GulimallExceptionControllerAdvice {
    /**
     * 捕获定义的异常
     */
    @ExceptionHandler(value = MethodArgumentNotValidException.class)
    public R handleVaildException(MethodArgumentNotValidException e) {
        log.error("数据校验出现问题{},异常类型:{}",e.getMessage(),e.getClass());
        Map<String,String> errorMap = new HashMap<>();
        BindingResult bindingResult = e.getBindingResult();
        bindingResult.getFieldErrors().forEach(fieldError -> {
            errorMap.put(fieldError.getField(),fieldError.getDefaultMessage());   //获取错误的属性的名字，获取到错误提示
        });
        return R.error(BizCodeEnume.VAILD_EXCEPTION.getCode(),BizCodeEnume.VAILD_EXCEPTION.getMsg()).put("data",errorMap);
    }
    /**
     * 兜底异常
     */
    @ExceptionHandler(value = Throwable.class)
    public R handleException(Throwable throwable) {
        return R.error(BizCodeEnume.UNKNOW_EXCEPTION.getCode(),BizCodeEnume.UNKNOW_EXCEPTION.getMsg());
    }
}
```

**异常错误码定义 （重点）**

后端将定义的错误码写入到开发手册，前端出现对于的错误，就可以通过手册查询到对应的异常

```java
/***
 * 错误码和错误信息定义类
 * 1. 错误码定义规则为5为数字
 * 2. 前两位表示业务场景，最后三位表示错误码。例如：100001。10:通用 001:系统未知异常
 * 3. 维护错误码后需要维护错误描述，将他们定义为枚举形式
 * 错误码列表：
 *  10: 通用
 *      001：参数格式校验
 *  11: 商品
 *  12: 订单
 *  13: 购物车
 *  14: 物流
 *  15: 用户
 */ 
public enum BizCodeEnume {     //注意单纯的Enum是抽象类，biz继承了enum
    UNKNOW_EXCEPTION(10000,"系统未知异常"),         //值一般是大写的字母，多个值之间以逗号分隔，取值是必须有限的，理解为类的一个实例（对象）
    VAILD_EXCEPTION(10001,"参数格式校验失败");       //类.对象；	数组=类.values();
    private int code;
    private String msg;
    BizCodeEnume(int code,String msg){
        this.code = code;
        this.msg = msg;
    }
    public int getCode() {
        return code;
    }
    public String getMsg() {
        return msg;
    }
}
```



## 十、商品服务&属性分组

### 10.1、基本概念

#### 1、SPU & SKU

**SPU：Standard Product Unit （标准化产品单元）**是商品信息聚合的最小单位，是一组可复用、易检索的标准化信息的集合，该集合描述了一个产品的特性。

**SKU: Stock KeepingUnit (库存量单位)**库存进出计量的基本单元，可以是件/盒/托盘等单位，每种产品对应有唯一的SKU号。

IPhoneX 是 SPU,MI8 是 SPU			IPhoneX 64G 黑曜石 是 SKU				MIX8 + 64G 是 SKU						（类似类和对象的关系）



#### 2、基本属性 & 销售属性

基础属性：同一个SPU拥有的特性叫**基本属性**（属性、规格参数）；

销售属性：能决定库存量的叫**销售属性**。如颜色

​	每个分类下的商品共享规格参数，与销售属性。只是有些商品不一定要用这个分类下全部的属性；

​	三级分类----属性的分组----属性

​	规格参数（属性）中有些是可以提供检索的，他们具有自己的分组

​	属性名确定的，但是值是每一个商品不同来决定的

> 【属性分组-规格参数-销售属性-三级分类】关联关系  		catelogId三级分类id。attrGroupId属性分组

<img src="/image-20201020150957557.png" alt="image-20201020150957557" style="zoom: 50%;" />

SPU-SKU-属性表

<img src="/image-20201020151022330.png" alt="image-20201020151022330" style="zoom: 50%;" />



<img src="/image-20210613123714982.png" alt="image-20210613123714982" style="zoom: 67%;" />

#### 3、接口文档位置

https://easydoc.xyz/s/78237135



### 10.2 、前端组件抽取 & 父子组件交互

#### 1 、属性分组 

**在gulimall-admin里导入sql表单sys_menu；**

现在想要实现点击菜单的左边，能够实现在右边展示数据

<img src="https://img-blog.csdnimg.cn/img_convert/b54bb02ae813a74fed74f6236ac40fdf.png" alt="image-20200430215649355" style="zoom:67%;" />

根据其他的请求地址 http://localhost:8001/#/product-attrgroup 访问，所以应该有product/attrgroup.vue。

之前写过product/cateory.vue，现在在 modules 中新建 common 文件夹，公共部分抽象到common/cateory.vue（也就是左侧的tree单独成一个vue组件）

category.vue 核心代码，抽取需要的结果，多余的结果进行删除

```vue
<template>
  <el-tree :data="menus" :props="defaultProps" node-key="catId" ref="menuTree" @node-click="nodeClick">
  </el-tree>
</template>
```

#### 2 、Layout 布局

1）左侧：要在左面显示菜单，右面显示表格。复制`<el-row :gutter="20">。。。`，放到attrgroup.vue的`<template>`。20表示列间距（类似bootstrap） 

```vue
<el-row :gutter="20">
  <el-col :span="6">表格</el-col>
  <el-col :span="18">菜单</el-col>
</el-row>     //分为2个模块，分别占6列和18列（分别是tree和当前spu等信息）
```

在attrgroup.vue（父）中导入common/category 组件

```javascript
 <el-col :span="6">
       <!-- 直接使用-->
      <category @tree-node-click="treenodeclick"></category>
    </el-col>
<script>
import Category from "../common/category";
export default {
  //import引入的组件需要注入到对象中才能使用。组件名:自定义的名字，一致可以省略
  components: { Category},
```

2）右侧表格内容：

复制代码生成器的gulimall-product\src\main\resources\src\views\modules\product\attrgroup.vue中的部分内容div到attrgroup.vue

批量删除是弹窗add-or-update，导入data、结合components


#### 3、 父子组件如何进行交互

要实现功能：点击左侧，右侧表格对应内容显示。（类似嵌套div事件后冒泡（是指一次点击调用了两个div的点击函数）

1）子组件中的 Tree 的一个事件  ,节点被点击时的回调方法 ，在category中绑定node-click事件，this.$emit发送一个事件并携带上数据

```
<el-tree :data="menus" :props="defaultProps" node-key="catId" ref="menuTree"  @node-click="nodeClick"	></el-tree>   //@为点击事件

nodeClick(data,Node,component){
    console.log("子组件被点击",data,Node,component);
    this.$emit("tree-node-click",data,Node,component);
},                // this.$emit("事件名",携带的数据...)
```

2）父组件中父组件可以感知到，获取发送的事件

```vue
// 在attr-group中写自定义的函数接收传递过来的参数
<category @tree-node-click="treeNodeClick"></category>     //@为被点击事件
 
//感知到并获取发送的事件数据
    treenodeclick(node, data, component) {
      console.log("attrgroup感知到category节点被点击");
      console.log("刚才被点击的菜单id:" + data.catId);
      if (node.level == 3) {
        this.catId = data.catId;
        this.getDataList();
      }
    },
```



### 10.3 、获取属性分类分组

查看提供的接口文档https://easydoc.xyz/s/78237135

![image-20201020153526835](/image-20201020153526835.png)

查询功能：GET /product/attrgroup/list/{catelogId}，请求参数需要我们带对应的分页参数，去product项目下的`attrgroup-controller`里修改

```java
@RequestMapping("/list/{catelogId}")
public R list(@RequestParam Map<String, Object> params, @PathVariable Long catelogId){
    //        PageUtils page = attrGroupService.queryPage(params);
    PageUtils page = attrGroupService.queryPage(params,catelogId);
    return R.ok().put("page", page);
}
```

增加接口与实现 AttrGroupServiceImpl类中：

Query里面就有个方法getPage()，传入map，将map解析为mybatis-plus的IPage<T>对象
自定义PageUtils类用于传入IPage对象，得到其中的分页信息
AttrGroupServiceImpl extends ServiceImpl，其中ServiceImpl的父类中有方法page(IPage, Wrapper)。对于wrapper而言，没有条件的话就是查询所有
queryPage()返回前还会return new PageUtils(page);，把page对象解析好页码信息，就封装为了响应数据

```java
@Override// 根据分类返回属性分组 // 按关键字或者按id查
    public PageUtils queryPage(Map<String, Object> params, Long catelogId) {
        String key = (String) params.get("key");
        QueryWrapper<AttrGroupEntity> wrapper = new QueryWrapper<>();
        // select * from AttrGroup where attr_group_id=key or attr_group_name like key
        if(!StringUtils.isEmpty(key)){      
      wrapper.and(obj->obj.eq("attr_group_id", key).or().like("attr_group_name", key) );}  // 传入consumer 
        if(catelogId == 0){//  0的话查所有
            // Query可以把map封装为IPage // this.page(IPage,QueryWrapper)
    IPage<AttrGroupEntity> page = this.page(new Query<AttrGroupEntity>().getPage(params),wrapper);// Query自己封装方法返回Page对象
            return new PageUtils(page);
        }else {
            wrapper.eq("catelog_id", catelogId);
            IPage<AttrGroupEntity> page = this.page(new Query<AttrGroupEntity>().getPage(params), wrapper);
            return new PageUtils(page);
        }
    }
```

然后调整前端，发送请求时url携带id信息，${this.catId}，get请求携带page信息，点击第3级分类时才查，修改attr-group.vue中的函数即可

```js
//感知树节点被点击
treenodeclick(data, node, component) {
    if (node.level == 3) {
        this.catId = data.catId;
        this.getDataList(); //重新查询
    }
},    
// 获取数据列表
getDataList() {
    this.dataListLoading = true;
    this.$http({
        url: this.$http.adornUrl(`/product/attrgroup/list/${this.catId}`),
        method: "get",
        params: this.$http.adornParams({
            page: this.pageIndex,
            limit: this.pageSize,
            key: this.dataForm.key
        })
    }).then(({ data }) => {
        if (data && data.code === 0) {
            this.dataList = data.page.list;
            this.totalPage = data.page.totalCount;
        } else {
            this.dataList = [];
            this.totalPage = 0;
        }
        this.dataListLoading = false;
    });
},
```



### 10.4、新增属性分组 & 级联选择器

上面演示了查询功能，下面写insert分类，因为分类可以对应多个属性分组，所以我们新增的属性分组时要指定分类

<img src="https://fermhan.oss-cn-qingdao.aliyuncs.com/img/20210216172146.png" alt="img" style="zoom:67%;" />

**Cascader 级联选择器**:	当一个数据集合有清晰的层级结构时，可通过级联选择器逐级查看并选择。文档：https://element.eleme.cn/#/zh-CN/component/cascader

级联选择的下拉同样是个options（选项）数组，把data()里的数组categorys绑定到options上即可，更详细的设置可以用props绑定

attrgroup-add-or-update.vue 中 加入该组件

```vue
  <el-cascader
          v-model="dataForm.catelogPath"
           placeholder="试试搜索：手机"
          :options="categorys"
          :props="props"
          filterable
        ></el-cascader>
<!--
placeholder="试试搜索：手机"默认的搜索提示
:options="categorys" 可选项数据源，键名可通过 Props 属性配置
:props="props" 配置选项	
 filterable 是否可搜索选项-->
 props:{
        value:"catId",
        label:"name",
        children:"children"
      },
//组件加载完成后，调用方法 ( getCategorys() ) 获取菜单数据，在设置到options
 getCategorys(){
      this.$http({
        url: this.$http.adornUrl("/product/category/list/tree"),
        method: "get"
      }).then(({ data }) => {
        this.categorys = data.data;
      });
    },
```

优化：没有下级菜单时不要有下一级空菜单，在java端product/entity/CategoryEntity把children属性空值去掉，空集合时去掉children字段，

```
@TableField(exist =false)
@JsonInclude(JsonInclude.Include.NON_EMPTY) // 不为空时返回的json才带该字段
private List<CategoryEntity> children;
```

提交完后返回页面也刷新了，是用到了父子组件。在`$message`弹窗结束回调`$this.emit`

```js
 //attrgroup.vue（父）：
 <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>    // 获取数据列表
  //attrgroup-add-or-update.vue：
  this.$message({
                message: "操作成功",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.visible = false;
                  this.$emit("refreshDataList");   //子组件传递事件给父组件
```



### 10.5 、分类修改 & 回显级联选择器

修改回显应该回显3级

<img src="https://img-blog.csdnimg.cn/img_convert/281a417f46ec3c8d6778a4921d1ef477.png" alt="img" style="zoom:80%;" />

在 AttrGroup：

```javascript
<el-button
           type="text"
           size="small"
           @click="addOrUpdateHandle(scope.row.attrGroupId)"
           >修改</el-button>
<script>
    // 新增 / 修改
    addOrUpdateHandle(id) {
        // 先显示弹窗
        this.addOrUpdateVisible = true;
        // .$nextTick(代表渲染结束后再接着执行
        this.$nextTick(() => {
 // this是attrgroup.vue, 点击修改后会触发addOrUpdateHandle方法，引用import里的addOrUpdate组件（attrgroup-add-or-update.vue ）并调用他的 init初始化方法，
            this.$refs.addOrUpdate.init(id);    ///$refs是它里面的所有组件，父组件调用子组件；$emit是子组件传递事件给父组件
        });
    },
</script>
```

根据属性分组id查到属性分组，init执行完成会回调，在init方法里进行回显，但是分类的id还是不对，应该是用数组封装的路径

```java
init(id) {
    this.dataForm.attrGroupId = id || 0;
    this.visible = true;
    this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.attrGroupId) {
            this.$http({
                url: this.$http.adornUrl(`/product/attrgroup/info/${this.dataForm.attrGroupId}`  ),     //有路径变量用飘号      
                method: "get",
                params: this.$http.adornParams()
            }).then(({ data }) => {
                if (data && data.code === 0) {
                    this.dataForm.attrGroupName = data.attrGroup.attrGroupName;
                    this.dataForm.sort = data.attrGroup.sort;
                    this.dataForm.descript = data.attrGroup.descript;
                    this.dataForm.icon = data.attrGroup.icon;
                    this.dataForm.catelogId = data.attrGroup.catelogId;
                    //查出catelogId的完整路径
                    this.catelogPath =  data.attrGroup.catelogPath;
                }
            });
        }
    });
```

添加属性，在product/entity/AttrGroupEntity

```java
/**
	 * 三级分类修改的时候回显路径
	 */
@TableField(exist = false) // 数据库中不存在
private Long[] catelogPath;
```

修改attrGroupController，找到属性分组id对应的分类，然后把该分类下的所有属性分组都填充好

```
@RequestMapping("/info/{attrGroupId}")
public R info(@PathVariable("attrGroupId") Long attrGroupId){
    AttrGroupEntity attrGroup = attrGroupService.getById(attrGroupId);
    // 用当前当前分类id查询完整路径并写入 attrGroup
    Long[] paths= categoryService.findCateLogPath(attrGroup.getCatelogId())
    attrGroup.setCatelogPath(paths);
    return R.ok().put("attrGroup", attrGroup);
}
```

添加categoryService接口和实现类：

```java
@Override
public Long[] findCatelogPath(Long catelogId) {
    List<Long> paths = new ArrayList<>();
    List<Long> parentPath = findParentPath(catelogId, paths);
    // 反转
    Collections.reverse(parentPath);
    //转成Long[] 数组返回
    return parentPath.toArray(new Long[parentPath.size()]);
}
/**
 * 递归查找
 * @param catelogId 三级分类的id
 * @param paths 路径
 * @return
 */
private List<Long> findParentPath(Long catelogId,List<Long> paths) {
    // 1、收集当前节点id
    paths.add(catelogId);
    // 2、通过分类id拿到 Category 对象
    CategoryEntity byId = this.getById(catelogId);
    // 3、如果不是根节点 就一直递归下去查找
    if (byId.getParentCid() != 0) {
        findParentPath(byId.getParentCid(),paths);
    }
    return paths;
}
```

优化：会话关闭时清空内容，防止下次开启还遗留数据

```
<el-dialog@closed="dialogClose" >
  methods: {
    dialogClose(){
      this.catelogPath = [];
    },
```



### 10.6、品牌分类关联&级联更新

#### 1、分页插件

mybatis-plus用法，官网：https://mp.baomidou.com/guide/page.html

mp分页使用，需要先添加个mybatis的拦截器:在product下建config包

```
@EnableTransactionManagement
@MapperScan("com.atguigu.gulimall.product.dao")
@Configuration
public class MybatisConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
        paginationInterceptor.setOverflow(true);
        // 设置最大单页限制数量，默认 500 条，-1 不受限制
        paginationInterceptor.setLimit(1000);
        return paginationInterceptor;
    }
}
```

修改BrandServiceImpl，分页并加模糊查询功能:

```java
//wrapper.orderByDesc("age");wrapper.orderByAsc("age");wrapper.having("id > 8");eq；ne；or；in；notIn
@Service("brandService")
public class BrandServiceImpl extends ServiceImpl<BrandDao, BrandEntity> implements BrandService {
    @Override
    public PageUtils queryPage(Map<String, Object> params) {
        String key= (String) params.get("key");
        QueryWrapper<BrandEntity> queryWrapper = new QueryWrapper<>();    //查询条件
        if (!StringUtils.isEmpty(key)){
            queryWrapper.eq("brand_id",key).or().like("name",key);
        }
 IPage<BrandEntity> page = this.page(new Query<BrandEntity>().getPage(params), queryWrapper); //不是自己创建page对象，而是根据url的参数自动封装
        return new PageUtils(page);
    }
}
```

#### 2、级联更新

新增的华为、小米、oppo是手机下的品牌，即分类对品牌一对多，品牌对分类也是一对多，比如小米手机和小米电视，，看出两者即是多对多，多对多的关系应该有relation表。

API：https://easydoc.xyz/doc/75716633/ZUqEdvA4/SxysgcEF，看接口文档传的参数为brandId，添加方法CategoryBrandRelationController：

```
@GetMapping("/catelog/list")   //查询用get方法
public R list(@RequestParam("brandId") Long brandId){   
    // 根据品牌id获取其分类信息
    List<CategoryBrandRelationEntity> data = categoryBrandRelationService.list(
        new QueryWrapper<CategoryBrandRelationEntity>().eq("brand_id",brandId)
    );
    return R.ok().put("data", data);
}
```

1）关联表的优化：

分类名本可以在brand表中，但**关联查询对数据库性能有影响（笛卡尔乘积）**，在电商中大表数据不做关联，哪怕**分步查**也不用关联，所以像name这种**冗余字段**可以保存。

前端只传过来的id（看接口文档），原生的save没有name可存，在categoryBrandRelationController里自定义save方法：

```
@RequestMapping("/save")
public R save(@RequestBody CategoryBrandRelationEntity categoryBrandRelation){
    categoryBrandRelationService.saveDetail(categoryBrandRelation);
    return R.ok();}
```

写接口和实现方法：

```
// * 根据获取品牌id 、三级分类id查询对应的名字保存到数据库
@Override // CategoryBrandRelationServiceImpl
public void saveDetail(CategoryBrandRelationEntity categoryBrandRelation) {
    Long brandId = categoryBrandRelation.getBrandId();
    Long catelogId = categoryBrandRelation.getCatelogId();
    // 根据id查 品牌名字、分类名字，统一放到一个表里，就不关联分类表查了
    BrandEntity brandEntity = brandDao.selectById(brandId);
    CategoryEntity categoryEntity = categoryDao.selectById(catelogId);
    // 把查到的设置到要保存的哪条数据里
    categoryBrandRelation.setBrandName(brandEntity.getName());
    categoryBrandRelation.setCatelogName(categoryEntity.getName());
    this.save(categoryBrandRelation);}
```

2）保持冗余字段的数据一致性

如果分类表或品牌表里的name发送变化，那么关联表里的name字段应该同步变化。在分类表或品牌表修改的时候，也要把关联表的数据进行更改

```java
// BrandController：调用一个自定义方法
    @RequestMapping("/update")
    public R update(@Validated(UpdateGroup.class) @RequestBody BrandEntity brand){
		brandService.updateDetail(brand);
        return R.ok();}
// 接口和BrandServiceImpl
@Override
public void updateDetail(BrandEntity brand) {   
    // 自己品牌表先更新
    this.updateById(brand);
    if (!StringUtils.isEmpty(brand.getName())) {
        // 调用关联表中自定义方法，同步更新关联表中的数据
        categoryBrandRelationService.updateBrand(brand.getBrandId(),brand.getName());
        // TODO 更新其他关联
    }
}
// categoryBrandRelationServiceImpl：  //也可以直接写在上面方法里
  @Override    //需要更新的id和name封装到关联表类里，再调用mp带的update方法或自定义sql语句方法
    public void updateBrand(Long brandId, String name) {
        CategoryBrandRelationEntity relationEntity = new CategoryBrandRelationEntity();
        relationEntity.setBrandName(name);
        relationEntity.setBrandId(brandId);
        this.update(relationEntity,new UpdateWrapper<CategoryBrandRelationEntity>().eq("brand_id",brandId));
    }
```

```java
//CategoryController：调用一个自定义方法
 @RequestMapping("/update")
  public R update(@RequestBody CategoryEntity category){
	categoryService.updateCascade(category);  return R.ok(); }
// 接口和CategoryServiceImpl
@Override
public void updateCascate(CategoryEntity category) {
    this.updateById(category);
     // 调用关联表中自定义方法，同步更新关联表中的数据
    categoryBrandRelationService.updateCategory(category.getCatId(),category.getName());
}
// categoryBrandRelationServiceImpl：
   @Override
    public void updateCategory(Long catId, String name) {
        this.baseMapper.updateCategory(catId,name);
    }
//categoryBrandRelationDao：
void updateCategory(@Param("catId")Long catId,@Param("name") String name);
//mapper的sql语句，可生成<update id="">
 <update id="updateCategory">
        update pms_category_brand_relation set catelog_name=#{name} where catelog_id=#{catId}
 </update>
```

## 十一、商品服务&平台属性

### 11.1  Object 划分

1、**PO** (persistant object) 持久化对象

**po 就是对应数据库中某一个表的一条记录**，多个记录可以用 PO 的集合，PO 中应该不包含任何对数据库到操作

2、**DO** ( Domain Object) 领域对象

就是从现实世界抽象出来的有形或无形的业务实体

3、**TO** (Transfer Object) 数据传输对象

不同的应用程序之间传输的对象

4、**DTO**  (Data Transfer Object) 数据传输对象

这个概念来源于 J2EE 的设计模式，原来的目的是为了 EJB的分布式应用提供粗粒度的数据实体，以减少分布式调用的次数，从而提高分数调用的性能和降低网络负载，但在这里，泛指用于展示层与服务层之间的数据传输对象

5、**VO**(value object) 值对象

通常用于业务层之间的数据传递，和 PO 一样也是仅仅包含数据而已，但应是抽象出的业务对象，可以和表对应，也可以不，这根据业务的需要，用 new 关键字创建，由 GC 回收

**接受页面传递来的数据，封装对象，封装页面需要用的数据**

6、**BO**(business object) 业务对象

主要作用是把业务逻辑封装成一个对象，这个对象包括一个或多个对象，比如一个简历，有教育经历，工作经历，社会关系等等，我们可以把教育经历对应一个 PO 、工作经验对应一个 PO、 社会关系对应一个 PO, 建立一个对应简历的的 BO 对象处理简历，每 个 BO 包含这些 PO ,这样处理业务逻辑时，我们就可以针对 BO 去处理

7、**POJO** ( plain ordinary java object) 简单无规则 java 对象

**POJO 时是 DO/DTO/BO/VO 的统称**

8、**DAO**（data access object） 数据访问对象

是一个 sun 的一个标准 j2ee 设计模式，这个模式有个接口就是 DAO ，他负持久层的操作，为业务层提供接口，此对象用于**访问数据库，通常和 PO 结合使用**，DAO 中包含了各种数据库的操作方法，通过它的方法，结合 PO 对数据库进行相关操作，夹在业务逻辑与数据库资源中间，配合VO 提供数据库的 CRUD 功能



### 11.2 新增规格参数&VO

当新增字段时，往往会在entity实体类中新建一个字段，**并标注 @TableField(exist=false) 数据库中不存在该字段** ,然而比较规范的做法是新建一个`vo`文件夹，存不是数据库的字段。

 URL: http://localhost:88/api/product/attr/save，它在保存的时候，只是保存了attr，并没有保存attrgroup，为了解决这个问题，新建一个vo/AttrVo.java，在原AttrEntity基础上增加了attrGroupId字段，vo只用来响应请求封装数据，不用标注@TableId、@TableField 注解。（可继承，但多实现少继承）

```
@Data
public class AttrVo extends AttEntity implements Serializable { private Long attrGroupId;}
```

pms_attr属性表里同时有基本属性和销售属性，靠attr_type=1/0区别，在common里建 constant/ProductConstant：

```
public class ProductConstant {
    public enum AttrEnum{
        ATTR_TYPE_BASE(1,"基本属性"),ATTR_TYPE_SALE (0,"销售属性");
        private int code;
        private String msg;
        AttrEnum(int code ,String msg){
            this.code=code;
            this.msg=msg;
        }
        public int getCode(){
            return  code;
        }
        public String getMsg(){
            return msg;
  }  }}
```

如果是销售属性就不存入关联表，是基础属性就存入:

```java
//AttrController:
 @RequestMapping("/save")
  public R save(@RequestBody AttrVo attr){   //改AttrEntity为AttrVo
		attrService.saveAttr(attr);
        return R.ok(); }
//接口和实现类AttrServiceImpl
@Autowired
AttrAttrgroupRelationDao relationDao;
 @Transactional
    @Override
    public void saveAttr(AttrVo attr) {
        AttrEntity attrEntity = new AttrEntity();
        //重要！ 这个是spring的工具类，用于拷贝同名属性,vo复制到po，属性对拷
        BeanUtils.copyProperties(attr, attrEntity);
        this.save(attrEntity);   //先保存本表数据
        //是基本属性才保存关联关系
 if (attr.getAttrType()== ProductConstant.AttrEnum.ATTR_TYPE_BASE.getCode()&&attr.getAttrGroupId()!=null){  //防止只有属性id没有组id也保存情况
            AttrAttrgroupRelationEntity relationEntity = new AttrAttrgroupRelationEntity();
            relationEntity.setAttrGroupId(attr.getAttrGroupId());
            relationEntity.setAttrId(attrEntity.getAttrId());
            relationDao.insert(relationEntity);
        }
    }
```



### 11.3 查完整的规格参数或销售属性*

![image-20201021051239049](/image-20201021051239049.png)

接口文档：https://easydoc.xyz/doc/75716633/ZUqEdvA4/FTx6LRbR

<img src="/image-20210612144457348.png" alt="image-20210612144457348" style="zoom:67%;" />

发现没有 catelogName（分类名） 和 AttrGroupName （属性分组）的数据， 原因是 AttrEntity没有这两个属性

![image-20201021051653178](/image-20201021051653178.png)



新建一个AttrRespVo类继承Attrvo

```java
@Data
public class AttrRespVo extends AttrVo {
    /**
     * catelogName 手机数码，所属分类名字
     * groupName 主体 所属分组名字
     */
    private String catelogName;
    private String groupName;
}
```

改AttrController，一个list当两个用，一个查销售属性一个查基础属性：（有快捷键可以快速改service和dao方法）

```java
@GetMapping("/{attrType}/list/{catelogId}")
public R baseAttrList(@RequestParam Map<String,Object> params,@PathVariable("catelogId") Long catelogId ,@PathVariable("attrType") String type){
        PageUtils page=attrService.queryBaseAttrPage(params,catelogId,type);
        return R.ok().put("page", page);    //看接口文档的响应为page，故键为page，map参数为分页信息
    }
```

先用mp的正常分页查出来数据，得到  **IPage** 对象，然后用PageUtils把分页信息得到，但里面的数据需要替换一下，不使用联表查询

```java
 @Override
    public PageUtils queryBaseAttrPage(Map<String, Object> params, Long catelogId, String type) {
        // 基本属性和销售属性我们放在一张表内，先根据 attr_type字段进行查询 ，1是基本属性 0是销售属性，
        //有eq查询条件，后面的<>泛型要加上//相当于是sql语句了，可写attr_type
       QueryWrapper<AttrEntity> queryWrapper = new QueryWrapper<AttrEntity>().eq("attr_type","base".equalsIgnoreCase(type)?
                ProductConstant.AttrEnum.ATTR_TYPE_BASE.getCode():ProductConstant.AttrEnum.ATTR_TYPE_SALE.getCode());
        if (catelogId != 0) {   queryWrapper.eq("catelog_Id", catelogId); }   // ""必须填写数据库里的字段名，下划线也一样
        String key = (String) params.get("key");
        if (!StringUtils.isEmpty(key)) {
            queryWrapper.and(obj->obj.eq("attr_id", key).or().like("attr_name", key)); }//作为一个整体
  IPage<AttrEntity> page = this.page(new Query<AttrEntity>().getPage(params), queryWrapper);  // 正式查询，带上查询条件，由url参数自动封装成Ipage对象
        //多显示分类名和属性名
        PageUtils pageUtils = new PageUtils(page);
        List<AttrEntity> records = page.getRecords();
        List<AttrRespVo> respVos = records.stream().map(   //集合要用流操作
            attrEntity -> {   AttrRespVo attrRespVo = new AttrRespVo();
            BeanUtils.copyProperties(attrEntity, attrRespVo);
            if ("base".equalsIgnoreCase(type)){   //是基本属性才设置属性分组名
                AttrAttrgroupRelationEntity attrId = relationDao.selectOne(
                        new QueryWrapper<AttrAttrgroupRelationEntity>().eq("attr_id", attrEntity.getAttrId()));
                if (attrId != null) {  
                    AttrGroupEntity attrGroupEntity = attrGroupDao.selectById(attrId.getAttrGroupId());
                    attrRespVo.setGroupName(attrGroupEntity.getAttrGroupName());
                }
            }
            CategoryEntity categoryEntity = categoryDao.selectById(attrEntity.getCatelogId());//得到分类
            if (categoryEntity != null) {   //实体类一定要判断非空
                attrRespVo.setCatelogName(categoryEntity.getName());
            }
            return attrRespVo;
        }).collect(Collectors.toList());
        pageUtils.setList(respVos);
        return pageUtils;
    }
```



### 11.4 修改回显&修改

#### 1、回显分类和属性分组

<img src="/image-20210612163842549.png" alt="image-20210612163842549" style="zoom:67%;" />

<img src="/image-20210612163427140.png" alt="image-20210612163427140" style="zoom: 67%;" />

```java
//Attrvo:
@Data
public class AttrRespVo extends AttrVo{
    private String catelogName;
    private String groupName;
    private Long[] catelogPath;}
//Attrcontroller:
 @RequestMapping("/info/{attrId}")
    public R info(@PathVariable("attrId") Long attrId){
		//AttrEntity attr = attrService.getById(attrId);
        AttrRespVo respVo=attrService.getAttrInfo(attrId);
        return R.ok().put("attr", respVo); }
//接口和AttrServiceimpl:
  @Override//修改时回显
    public AttrRespVo getAttrInfo(Long attrId) {
        AttrEntity attrEntity = this.getById(attrId);
        AttrRespVo respVo = new AttrRespVo();
        BeanUtils.copyProperties(attrEntity, respVo);
        //是基本属性才设置属性分组id
        if (attrEntity.getAttrType()== ProductConstant.AttrEnum.ATTR_TYPE_BASE.getCode()) {
            AttrAttrgroupRelationEntity relationEntity = relationDao.selectOne(
                    new QueryWrapper<AttrAttrgroupRelationEntity>().eq("attr_id", attrId));
            if (relationEntity != null) {    //得到实体类都要先判断一下是否非空，没有属性分组的话不会插入关联表
                respVo.setAttrGroupId(relationEntity.getAttrGroupId());
            }
        }
        //设置分类路径
        Long catelogId = attrEntity.getCatelogId();
        Long[] catelogPath = categoryService.findCatelogPath(catelogId);
        respVo.setCatelogPath(catelogPath);
       return  respVo;
  }
```

#### 2、修改规格参数

```java
 //Attrcontroller:
@RequestMapping("/update")
 public R update(@RequestBody AttrVo attr){
		attrService.updateAttr(attr);
        return R.ok();}
//接口和AttrServiceimpl:
  @Transactional    //改变两张表就是一个事件了
    @Override
    public void updateAttr(AttrVo attr) {
        AttrEntity attrEntity = new AttrEntity();
        BeanUtils.copyProperties(attr,attrEntity);
        this.updateById(attrEntity);
        //是基本属性才修改关联表
        if (attrEntity.getAttrType()== ProductConstant.AttrEnum.ATTR_TYPE_BASE.getCode()) {
            AttrAttrgroupRelationEntity relationEntity =new AttrAttrgroupRelationEntity();
            relationEntity.setAttrGroupId(attr.getAttrGroupId());
            relationEntity.setAttrId(attr.getAttrId());
            Integer count = relationDao.selectCount(new QueryWrapper<AttrAttrgroupRelationEntity>().eq("attr_id", attr.getAttrId()));
            if (count > 0) {
                relationDao.update(relationEntity,new UpdateWrapper<AttrAttrgroupRelationEntity>().eq("attr_id",attr.getAttrId()));//不写updateById
            }else {    //没有属性分组的话不会插入关联表里的
                relationDao.insert(relationEntity);
            }
        }
  }
```

修改有点问题的话，**在pms_attr表里加一个value_type字段，tinyint，  po 类中也要加 private Integer valueType;**



### 11.5 属性分组和基本属性关联

#### 1、获取属性分组关联的属性

AttrGroupController：

```java
//获取属性分组的关联的所有属性
 @GetMapping("/{attrgroupId}/attr/relation")
    public R attrRelation(@PathVariable("attrgroupId")long attrgroupId){
        List<AttrEntity> entities= attrService.getRelationAttr(attrgroupId);
        return R.ok().put("data",entities);
    }
```

AttrGroupServiceImpl ：

```java
 @Override
    public List<AttrEntity> getRelationAttr(long attrgroupId) {
        // 根据attr_group_id 查询到 关系表
    List<AttrAttrgroupRelationEntity> entities = relationDao.selectList(
            new QueryWrapper<AttrAttrgroupRelationEntity>().eq("attr_group_id", attrgroupId));
        // 从关系表中 取到 属性id
        List<Long> attrIds = entities.stream().map(attr -> attr.getAttrId()).collect(Collectors.toList());
        if(attrIds==null||attrIds.size()==0){
            return null;
        }
        Collection<AttrEntity> attrEntities = this.listByIds(attrIds);
        return (List<AttrEntity>) attrEntities;
    }
```

#### 2、删除属性与分组的关联关系*

<img src="/image-20201022162103501.png" alt="image-20201022162103501" style="zoom: 67%;" />

```java
//AttrGroupController：
//删除属性与分组的关联关系，就只删除关联表中的数据
     @PostMapping("/attr/relation/delete")  //前端传来数据所必须写的，下面的Requestbody也是
    public R deleteRelation(@RequestBody AttrAttrgroupRelationEntity[] entities){   //传来参数只有attrId和attrgroupId，可以用关联po，也可自定义类
        attrService.deleteRelation(entities);
        return R.ok();
    }
//接口和AttrServiceImpl：起封装一层的作用
@Override
    public void deleteRelation(AttrAttrgroupRelationEntity[] entities) {
       relationDao.deleteBatchRelation(Arrays.asList(entities));   //支持批量删除功能
    }
//AttrAttrgroupRelationDao:
    void deleteBatchRelation(@Param("entities") List<AttrAttrgroupRelationEntity> asList);
//AttrAttrgroupRelationDao.xml:  
<delete id="deleteBatchRelation">
        delete from pms_attr_attrgroup_relation where
        <foreach collection="entities" item="i" separator=" or ">
           ( attr_id=#{i.attrId} and attr_group_id=#{i.attrGroupId})
        </foreach>
</delete>
<!--where id in <foreach  item="item" collection="list" index="index"  open="(" separator="," close=")">#{item}</foreach>-->
```

#### 3、新增分组与属性的关联

##### 1）回显所有未关联属性

AttrGroupController：

```java
@GetMapping("/{attrgroupId}/noattr/relation")   
public R attrNoRelation(@PathVariable("attrgroupId") Long attrgroupId,
                        @RequestParam Map<String, Object> params) {    //还要获取分页信息
    PageUtils page = attrService.getNoRelationAttr(params,attrgroupId);
    return R.ok().put("page",page);
}
```

接口和AttrServiceImpl：  组id-》当前分类-》所有分组-》所有组id-》关联属性id-》，分类所有基础属性-》排除关联属性

```java
 @Override//前提：当前分组只关联自己分类里的属性
    public PageUtils getNoRelationAttr(long attrgroupId, Map<String, Object> params) {
        //1、找到当前分类
        AttrGroupEntity attrGroupEntity = attrGroupDao.selectById(attrgroupId);
        Long catelogId = attrGroupEntity.getCatelogId();  //同一张表里，由一个字段得到另一个字段，先得到表po
        //2、找到同一分类下的所有组（包括自己）
        List<AttrGroupEntity> group = attrGroupDao.selectList(new QueryWrapper<AttrGroupEntity>().eq("catelog_id", catelogId));
        // 拿到所有组id   //是集合或数组要操作用流
        List<Long> collect = group.stream().map(item -> item.getAttrGroupId()).collect(Collectors.toList());
        //2.2  根据分组id查询出关联表po
        List<AttrAttrgroupRelationEntity> groupId = relationDao.selectList(new QueryWrapper<AttrAttrgroupRelationEntity>()
                .in("attr_group_id", collect));
        // 拿到已经关联的所有的属性id
        List<Long> attrIds = groupId.stream().map(item -> item.getAttrId()).collect(Collectors.toList());
        //2.3 得到当前分类的所有基础属性
        QueryWrapper<AttrEntity> wrapper = new QueryWrapper<AttrEntity>().eq("catelog_id", catelogId)
                .eq("attr_type", ProductConstant.AttrEnum.ATTR_TYPE_BASE.getCode());
        // 从分类所有属性中排除已经关联的基本属性
        if (attrIds != null && attrIds.size() > 0) {   //保证已经关联的属性不为空
            wrapper.notIn("attr_id", attrIds);
        }
        //模糊查询，取出参数进行 对应条件查询
        String key = (String) params.get("key");
        if (!StringUtils.isEmpty(key)) {  wrapper.and(w -> w.eq("attr_id", key).or().like("attr_name", key)); }
        // 根据传来参数的分页数据 以及 wrapper查询条件进行查询
        IPage<AttrEntity> page = this.page(new Query<AttrEntity>().getPage(params), wrapper);
        PageUtils pageUtils = new PageUtils(page);
        return pageUtils;
    }
```

##### 2）新增关联

AttrGroupController：

```java
  @Autowired
 AttrAttrgroupRelationService relationService;
@PostMapping("/attr/relation")
public R addRelation(@RequestBody List<AttrAttrgroupRelationEntity> relationEntities){  //可直接封装成list
        relationService.saveBatch(relationEntities);//若是自定义类接收数据，要重写这个方法（参数不同），属性对拷在用mp的原方法
        return R.ok();
    }
```

<img src="/image-20210613145116007.png" alt="image-20210613145116007" style="zoom:67%;" />

## 十二、商品服务&发布商品

点击商品维护-》发布商品

- 基本信息		规则参数				（前两步都是spu）

- 销售属性		SKU信息(根据上一步选择的录入价格、标题)		保存完成

  <img src="https://img-blog.csdnimg.cn/img_convert/0bd4b4b5e453050feeba5bd166b9f8f6.png" alt="img" style="zoom: 25%;" />

在/用户系统/会员等级里添加会员等级；

在member模块yml：

```
pring:
  datasource:
    username: root
    password: "051674"
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/gulimall_ums?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=GMT%2B8
    #nacos作为注册中心
  cloud:
    nacos:
      server-addr: localhost:8848
  application:
    name: gulimall-member
mybatis-plus:
  mapper-locations: classpath:/mapper/**/*.xml
  global-config:
    db-config:
      id-type: auto
server:
  port: 8000
```

gateway/yml添加：

```
        - id: gulimall-member
          uri: lb://gulimall-member
          predicates:
            - Path=/api/member/**
          filters:
            - RewritePath=/api/(?<segment>/?.*),/$\{segment}
```

### 12.1 获取分类关联的品牌

![image-20201023052252280](image-20201023052252280.png)

前端：接口文档：https://easydoc.xyz/doc/75716633/ZUqEdvA4/HgVjlzWV

<img src="/image-20210613184409539.png" alt="image-20210613184409539" style="zoom: 67%;" />

后端编写：

product/CategoryBrandRelationController：

```java
  /* 由分类查关联品牌
     * controller:1、处理请求，接受和校验数据
     *              2、接受service处理完的数据，分装给页面指定vo
     * service：接受controller传来数据，处理业务
     */
    @GetMapping("/brands/list")
    public R relationBrandsList(@RequestParam(value = "catId",required = true) Long catId){   // CatId Long类型，数据库为bigint类型
        List<BrandEntity> vos=categoryBrandRelationService.getBrandsByCatId();
        List<BrandVo> collect = vos.stream().map(item -> {
            BrandVo brandVo = new BrandVo();  //不用属性对拷，属性名一致才行
            brandVo.setBrandId(item.getBrandId());
            brandVo.setBrandName(item.getName());
            return brandVo;
        }).collect(Collectors.toList());
        return R.ok().put("data",collect);
    }
```

![image-20201023052748618](/image-20201023052748618.png)

product/service/impl/CategoryBrandRelationServiceImpl：

```java
@Autowired
BrandService brandService;//有更丰富的业务逻辑，不会循环依赖，spring解决了
@Override
public List<BrandEntity> getBrandCatId(Long catId) {    
    // 根据分类id查询出 分类和品牌的关系表
    List<CategoryBrandRelationEntity> catelogId = relationDao.selectList(new QueryWrapper<CategoryBrandRelationEntity>().eq("catelog_id", catId));
    List<BrandEntity> collect = catelogId.stream().map(item -> {   //为了方法的复用，返回的是一个po而不是catIds
        //根据品牌id查询出品牌对象
        BrandEntity brandEntity = brandDao.selectById(item.getBrandId());
        return brandEntity;
    }).collect(Collectors.toList());
    return collect;
}
```

### 12.2 获取分类下的分组与属性

若内存不够或麻烦可注释掉product/spuadd/里的require为false，不一定要上传图片，member开启的话就有会员功能，不开启就不显示，也可以限制每个应用内存：

<img src="/image-20210614153748225.png" alt="image-20210614153748225" style="zoom: 67%;" />

![image-20210614154006498](/image-20210614154006498.png)

**每个属性分组都要有属性才能回显**



<img src="/image-20201023072959266.png" alt="image-20201023072959266" style="zoom: 80%;" />

基本信息输入成功后，就会跳转到规格参数，并根据分类id查询出对应数据

```
//AttrGroupWithAttrsVo复制AttrGroup大部分属性：
//组中的属性
 private List<AttrEntity> attrs;
```

AttrGroupController

```java
/**
 *  获取分类下所有分组&关联属性
 * @param catelogId
 * @return
 */
@RequestMapping("/{catelogId}/withattr")
public R getAttrGroupWithAttrs(@PathVariable("catelogId") Long catelogId)   {
    List<AttrGroupWithAttrsVo> vos = attrGroupService.getAttrGroupWithAttrsByCatelogId(catelogId);
    return R.ok().put("data",vos);
}
```

AttrGroupService 实现

```java
@Override
public List<AttrGroupWithAttrsVo> getAttrGroupWithAttrsByCatelogId(Long catelogId) {
    // 1、根据分类id查询出 查询分组关系
    List<AttrGroupEntity> attrgroupEntites = this.list(new QueryWrapper<AttrGroupEntity>().eq("catelog_id", catelogId));
    List<AttrGroupWithAttrsVo> collect = attrgroupEntites.stream().map(group -> {
        AttrGroupWithAttrsVo attrsVo = new AttrGroupWithAttrsVo();
        // 2、将分组属性拷贝到 VO中
        BeanUtils.copyProperties(group, attrsVo);
        // 3、通过分组id查询出 商品属性信息
        // 调用 getRelationAttr方法先根据 分组id去 中间关系表查询到商品属性id 然后根据商品属性id查询到商品信息
        List<AttrEntity> relationAttr = attrService.getRelationAttr(attrsVo.getAttrGroupId());
        attrsVo.setAttrs(relationAttr);
        return attrsVo;
    }).collect(Collectors.toList());
    return collect;
}
```

### 12.3 VO 生成&保存新增商品

<img src="https://fermhan.oss-cn-qingdao.aliyuncs.com/img/20210218140953.png" alt="img" style="zoom:67%;" />

<img src="https://fermhan.oss-cn-qingdao.aliyuncs.com/img/20210218141001.png" alt="img" style="zoom: 67%;" />

商品属性、销售属性、规格参数、基本信息都填好了后就会生成一串 JSON

![image-20201023080411308](/image-20201023080411308.png)

我们将 json **放到 json在线解析网站**上 并生成对应得实体类

可先保存json，后面直接postman提交保存

#### 1、 更改生成的Vo

![image-20201023081250886](/image-20201023081250886.png)

有些参与计算的属性 如 weight，price 将类型更改为 BigDecimal ，cateId 改为 Long，get/set 方法删除加 @data

#### 2、 保存编码

SpuInfoEntity中和SpuSaveVo对不上的属性：

````
	//可开启mp的自动填充
	@TableField(fill = FieldFill.INSERT_UPDATE)
	private Date createTime;
	
	@TableField(fill = FieldFill.INSERT_UPDATE)
	private Date updateTime;
````

![image-20210614114710174](/image-20210614114710174.png)

product/SpuInfoServiceImpl:    				**filter(条件)  满足条件的保留，验证是true就留**

```java
  @Transactional
    @Override
    public void saveSpuInfo(SpuSaveVo vo) {
 // 1、保存Spu基本信息  pms_spu_info
        SpuInfoEntity spuInfoEntity = new SpuInfoEntity();
        BeanUtils.copyProperties(vo, spuInfoEntity);
//    spuInfoEntity.setCreateTime(new Date());              //可开启mp的自动填充
//    spuInfoEntity.setUpdateTime(new Date());
        this.baseMapper.insert(spuInfoEntity);       //mapper里用insert，service用save
   // 2、保存Spu的描述信息  pms_spu_info_desc
        SpuInfoDescEntity spuInfoDescEntity = new SpuInfoDescEntity();
        List<String> decript = vo.getDecript();
        spuInfoDescEntity.setSpuId(spuInfoEntity.getId()); // SpuInfoEntity保存到取得 spuId 设置到 Desc中
        spuInfoDescEntity.setDecript(String.join(",", decript)); // 以逗号来分割集合中元素，返回一个String
        spuInfoDescService.save(spuInfoDescEntity);
  // 3、保存Spu的图片集 pms_spu_images
        List<String> images = vo.getImages();
        spuImagesService.saveImages(spuInfoEntity.getId(), images);  //也可以将原生方法写这
  // 4、保存spu的规格参数 pms_product_attr_value
        List<BaseAttrs> baseAttrs = vo.getBaseAttrs();
        List<ProductAttrValueEntity> collect = baseAttrs.stream().map(attr -> {
            // 设置 spu 属性值
            ProductAttrValueEntity valueEntity = new ProductAttrValueEntity();
            valueEntity.setAttrId(attr.getAttrId());
            AttrEntity attrEntity = attrService.getById(attr.getAttrId());  //循环查库
            valueEntity.setAttrName(attrEntity.getAttrName());
            valueEntity.setSpuId(spuInfoEntity.getId());
            valueEntity.setQuickShow(attr.getShowDesc());
            valueEntity.setAttrValue(attr.getAttrValues());
            return valueEntity;
        }).collect(Collectors.toList());
        attrValueService.saveBatch(collect);
  // 5、保存SPU的积分信息 gulimall_sms sms => sms_spu_bounds
        Bounds bounds = vo.getBounds();
        SpuBoundTo spuBoundTo = new SpuBoundTo();
        BeanUtils.copyProperties(bounds, spuBoundTo);
        spuBoundTo.setSpuId(spuInfoEntity.getId());
        // 远程服务调用
        R r = couponFeignService.saveSpuBounds(spuBoundTo);
        if (r.getCode() != 0) { log.error("远程保存优惠信息失败"); }
        // 5、保存当前Spu对应的所有SKU信息
        List<Skus> skus = vo.getSkus();
        if (skus != null && skus.size() > 0) {
            // 遍历 skus 集合
            skus.forEach(item -> {
                String defaultImage = "";
                // 遍历 skus 集合中的图片
                for (Images image : item.getImages()) {
                    // 默认图片等于 1 该记录则是默认图片
                    if (image.getDefaultImg() == 1) { defaultImage = image.getImgUrl(); }
                }
                //  skuName;price; skuTitle; skuSubtitle;
                // 只有上面4个属性相同
                SkuInfoEntity skuInfoEntity = new SkuInfoEntity();
                BeanUtils.copyProperties(item, skuInfoEntity);
                // 其他属性需要自己赋值
                skuInfoEntity.setBrandId(spuInfoEntity.getBrandId());
                skuInfoEntity.setCatalogId(spuInfoEntity.getCatalogId());
                skuInfoEntity.setSaleCount(0L);
                skuInfoEntity.setSpuId(spuInfoDescEntity.getSpuId());
                skuInfoEntity.setSkuDefaultImg(defaultImage);
            //5.1、SKU的基本信息 pms_sku_info
                skuInfoService.save(skuInfoEntity);
                Long skuId = skuInfoEntity.getSkuId();
                // 保存 sku 图片信息
                List<SkuImagesEntity> imagesEntities = item.getImages().stream().map(img -> {
                    SkuImagesEntity skuImagesEntity = new SkuImagesEntity();
                    skuImagesEntity.setSkuId(skuId);
                    skuImagesEntity.setImgUrl(img.getImgUrl());
                    skuImagesEntity.setDefaultImg(img.getDefaultImg());
                    return skuImagesEntity;
                }).filter(entity -> !StringUtils.isEmpty(entity.getImgUrl())//filter(条件)  满足条件的保留，
                ).collect(Collectors.toList());
                // TODO 没有图片路径的无需保存
            //5.2、SKU的图片信息 pms_sku_images
                skuImagesService.saveBatch(imagesEntities);
                List<Attr> attr = item.getAttr();
                // 保存 sku 销售属性
                List<SkuSaleAttrValueEntity> skuSaleAttrValueEntities = attr.stream().map(a -> {
                    SkuSaleAttrValueEntity skuSaleAttrValueEntity = new SkuSaleAttrValueEntity();
                    BeanUtils.copyProperties(a, skuSaleAttrValueEntity);
                    skuSaleAttrValueEntity.setSkuId(skuId);
                    return skuSaleAttrValueEntity;
                }).collect(Collectors.toList());
           //5.3、SKU的销售属性信息 pms_sku_sale_attr_value
                saleAttrValueService.saveBatch(skuSaleAttrValueEntities);
           //5.4、SKU的优惠、满减等信息 gulimall_sms ->sms_sku_ladder \sms_sku_full_reduction\sms_member_price
                SkuReductionTo skuReductionTo = new SkuReductionTo();
                BeanUtils.copyProperties(item, skuReductionTo);
                skuReductionTo.setSkuId(skuId);
  if (skuReductionTo.getFullCount() > 0 || skuReductionTo.getFullPrice().compareTo(new BigDecimal("0")) == 1) {//封装类的比较
                    R r1 = couponFeignService.saveSkuReduction(skuReductionTo);
                    if (r1.getCode() != 0) {
                        log.error("远程保存sku优惠信息失败");
                    }
                }
            });
        }
    }
```

spuImagesServiceImpl：

```java
 @Override
    public void saveImages(Long id, List<String> images) {
        if (images != null || images.size() != 0) {
            List<SpuImagesEntity> collect = images.stream().map(img -> {     //相当于是for循环
                SpuImagesEntity spuImagesEntity = new SpuImagesEntity();
                spuImagesEntity.setSpuId(id);     //直接传id解耦
                spuImagesEntity.setImgUrl(img);
                return spuImagesEntity;
            }).collect(Collectors.toList());
            this.saveBatch(collect);
        }
    }
```

pms_spu_info_desc**主键不自增（表也没有设置**）：

```
@Data
@TableName("pms_spu_info_desc")
public class SpuInfoDescEntity implements Serializable {
	private static final long serialVersionUID = 1L;
	// * 商品id
	@TableId(type = IdType.INPUT)
	private Long spuId;
```

R类添加：

```
//查看code
	public Integer getCode(){
		return (Integer) this.get("code");}
```

#### 3、远程服务调用

1）在common包下，建/to/SkuReductionTo和common/to/SpuBoundTo；MemberPrice复制过去

2） product调用coupon模块，调用者下建一个feign包和 feign/CouponFeignService 接口

  过程：**先转为json再装到请求体里，nacos中找到被调用服务发送，只要json数据是兼容的（名字一样），双方就无需使用同个to**（被调的可以不用to，方法 名可不同，R相同）

```java
@FeignClient("gulimall-coupon")
public interface CouponFeignService {
    //先转为json再装到请求体里，在nacos中找到被调用服务发送，只要json数据是兼容的（名字一样），双方就无需使用同个to
    @PostMapping("/coupon/spubounds/save")
    public R saveSpuBounds(@RequestBody SpuBoundTo spuBoundTo);
//    @PostMapping("/save")
//    public R save(@RequestBody SpuBoundsEntity spuBounds);//不在同一个模块下，类和方法不能直接用
    @PostMapping("/coupon/skufullreduction/saveInfo")
R saveSkuReduction(@RequestBody SkuReductionTo skuReductionTo);}

//对应controller；coupon/SpuBoundsController：
 @PostMapping("/save")
    //@RequiresPermissions("coupon:spubounds:save")
    public R save(@RequestBody SpuBoundsEntity spuBounds){
		spuBoundsService.save(spuBounds);
        return R.ok(); }
//coupon/SkuFullReductionController：
 @PostMapping("/saveInfo")
    R saveInfo(@RequestBody SkuReductionTo skuReductionTo){
        skuFullReductionService.saveSkuReduction(skuReductionTo);
        return R.ok();}  //保存不用返回其他数据库内容，不用put
```

3）调用者主启动类 @EnableFeignClients(basePackages = "com.example.gulimall.product.feign")）

coupon/service/impl/SkuFullReductionServiceImpl：保存了 **商品阶梯价格**、**商品满减信息**、**商品会员价格**

```java
 @Override
    public void saveSkuReduction(SkuReductionTo skuReductionTo) {
        //5.4、SKU的优惠、满减等信息 gulimall_sms ->sms_sku_ladder \sms_sku_full_reduction\sms_member_price
        //sms_sku_ladder
        SkuLadderEntity skuLadderEntity = new SkuLadderEntity();
        BeanUtils.copyProperties(skuReductionTo,skuLadderEntity );
        if (skuLadderEntity.getFullCount() > 0) {
            skuLadderService.save(skuLadderEntity);
        }
        //sms_sku_full_reduction
        SkuFullReductionEntity skuFullReductionEntity = new SkuFullReductionEntity();
        BeanUtils.copyProperties(skuReductionTo,skuFullReductionEntity);
        // BigDecimal 用 compareTo来比较
        if (skuFullReductionEntity.getFullPrice().compareTo(new BigDecimal("0")) == 1) {
            this.save(skuFullReductionEntity);
        }
        //sms_member_price
        List<MemberPrice> memberPrice = skuReductionTo.getMemberPrice();
        List<MemberPriceEntity> collect = memberPrice.stream().map(item -> {
            MemberPriceEntity priceEntity = new MemberPriceEntity();
            priceEntity.setSkuId(skuReductionTo.getSkuId());
            priceEntity.setMemberPrice(item.getPrice());
            priceEntity.setMemberLevelName(item.getName());
            priceEntity.setMemberLevelId(item.getId());
            priceEntity.setAddOther(1);
            return priceEntity;
        }).filter(item -> {
            // 会员对应价格等于0 过滤掉
            return item.getMemberPrice().compareTo(new BigDecimal("0")) == 1;
        }).collect(Collectors.toList());
        memberPriceService.saveBatch(collect);
    }
```

#### 4、过程调整

<img src="/image-20201024113618961.png" alt="image-20201024113618961" style="zoom:80%;" />

#### 12.3.8 商品保存Debug 调试

debug时，mysql默认的隔离级别为读已提交，为了能够在调试过程中，获取到数据库中的数据信息，可以调整隔离级别为读未提交：(如果想要设置全局的，需要加上global字段)

```mysql
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

#### 12.3.9 其他问题处理

1、   **`sku_images` 表中 img_url 字段为空**

`sku_images` 中有很多图片都是为空，因此我们需要在程序中处理这个数据，空数据不写入到数据库中

<img src="/image-20201024114238060.png" alt="image-20201024114238060" style="zoom:67%;" />

`skuImages` 保存部分代码、如果 `ImgUrl` 为空则进行过滤

```java
}).filter(entity ->{
    //返回 true 需要 false 过滤
    return !StringUtils.isEmpty(entity.getImgUrl());
}).collect(Collectors.toList());
```

2、**sku 满减以及打折信息数据**

有部分数据 为0

![image-20201024114652266](/image-20201024114652266.png)

在代码中过滤对应为0的数据

```java
// 满几件 大于0 可以添加  满多少钱 至少要大于0
if (skuReductionTo.getFullCount() > 0 || skuReductionTo.getFullPrice().compareTo(new BigDecimal("0")) == 1) {
    R r1 = couponFeignService.saveSkuReduction(skuReductionTo);
    if (r1.getCode() != 0) {
        log.error("远程保存sku优惠信息失败");
    }
}
```

远程服务中也进行对应修改

```java
/**
保存 商品阶梯价格
件数 大于0才能进行修改
**/
if (skuLadderEntity.getFullCount() > 0) {
    skuLadderService.save(skuLadderEntity);
}
/**
保存商品满减信息
**/
  // BigDecimal 用 compareTo来比较
if (skuFullReductionEntity.getFullPrice().compareTo(new BigDecimal("0")) == 1) {

	this.save(skuFullReductionEntity);
}
/**
保存商品会员价格
也进行了过滤数据
**/
 }).filter(item -> {
            // 会员对应价格等于0 过滤掉
            return item.getMemberPrice().compareTo(new BigDecimal("0")) == 1;
        }).collect(Collectors.toList());
```



## 十三、 商品服务&商品管理

#### 13.1 商品管理 SPU 检索

对发布的商品进行对应的条件查询，主要查询的是 `pms_spu_info` 这张表 。

![image-20201024061157844](/image-20201024061157844.png)

![image-20210614193625856](/image-20210614193625856.png)

SpuInfoController：

```java
  @RequestMapping("/list")
    //@RequiresPermissions("product:spuinfo:list")
    public R list(@RequestParam Map<String, Object> params){
        PageUtils page = spuInfoService.queryPageByCondition(params);
        return R.ok().put("page", page);
    }
```

SpuInfoServiceImpl 实现

```java
/**
 * 根据条件进行查询 spu相关信息
 * @param params 分页参数  status上架状态，catelogId 分类id brandId 品牌id
 */
 @Override
    public PageUtils queryPageByCondition(Map<String, Object> params) {
        QueryWrapper<SpuInfoEntity> wrapper = new QueryWrapper<>();
        // 取出参数 key 进行查询
        String key = (String) params.get("key");
        if (!StringUtils.isEmpty(key)) {
            wrapper.and((w) ->{
                w.eq("id","key").or().like("spu_name",key);
            });
        }
        // 验证不为空 取出参数进行查询
        String status = (String) params.get("status");
        if (!StringUtils.isEmpty(status)) {
            wrapper.eq("publish_status",status);
        }
        String brandId = (String) params.get("brandId");
        if (!StringUtils.isEmpty(brandId)  && ! "0".equalsIgnoreCase(brandId)) {  //前端的默认数据为0，会默认查0的，改为是0就全查
            wrapper.eq("brand_id",brandId);
        }
        String catelogId = (String) params.get("catelogId");
        if (!StringUtils.isEmpty(catelogId) && ! "0".equalsIgnoreCase(catelogId)) {
            wrapper.eq("catalog_id",catelogId);
        }
        IPage<SpuInfoEntity> page = this.page(new Query<SpuInfoEntity>().getPage(params), wrapper);
        return new PageUtils(page);
    }
```

问题：在SPU中，写出的日期数据都不符合规则，可以设置写出数据的规则：

```
spring:
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8
```

#### 13.1 商品管理 SkU 检索

 主要查询是 `pms_sku_info` 这张表，我出现的问题是发布商品时候，实体类的 价格 与 VO属性不一致，出现了数据没有拷贝，修改后，数据查询正常

![image-20201024061836312](/image-20201024061836312.png)

![image-20210614204235724](/image-20210614204235724.png)

SkuInfoController:

```
@RequestMapping("/list")
//@RequiresPermissions("product:skuinfo:list")
public R list(@RequestParam Map<String, Object> params){
    PageUtils page = skuInfoService.queryPageByCondition(params);
    return R.ok().put("page", page);
}
```

SkuInfoServiceImpl:

```java
    QueryWrapper<SkuInfoEntity> wrapper = new QueryWrapper<>();
        // 取出参数 进行查询
        String key = (String) params.get("key");
  if (!StringUtils.isEmpty(key)) { wrapper.and(w ->w.eq("sku_id",key).or().like("sku_name",key)); }
        // 验证 id 是否为0 否则进行匹配
        String catelogId = (String) params.get("catelogId");
  if (!StringUtils.isEmpty(catelogId) && !"0".equalsIgnoreCase(catelogId)) { wrapper.eq("catalog_id",catelogId); }
        String brandId = (String) params.get("brandId");
   if (!StringUtils.isEmpty(brandId) && !"0".equalsIgnoreCase(brandId)) { wrapper.eq("brand_id",brandId); }
        String min = (String) params.get("min");
        if (!StringUtils.isEmpty(min)) { wrapper.ge("price",min); }    //前端的默认数据为0，会默认查大于等于0的
        String max = (String) params.get("max");
        if (!StringUtils.isEmpty(max) ) {
            // 怕前端传递的数据是 abc 等等 所以要抛出异常
            try {
                BigDecimal bigDecimal = new BigDecimal(max);
                if ( bigDecimal.compareTo(new BigDecimal("0")) == 1) {     //前端的默认数据为0，会默认查小于等于0的
                    wrapper.le("price",max);
                }
            } catch (Exception e) { e.printStackTrace(); }
        }
        IPage<SkuInfoEntity> page = this.page(new Query<SkuInfoEntity>().getPage(params), wrapper);
        return new PageUtils(page);
    }
```



## 十四、仓储服务&仓库管理

### 14.1 仓库维护

库存信息表：

wms_ware_info 包括仓库所在地区等仓库信息（与商品无关）
wms_ware_sku：具体商品的库存量和所在仓库
wms_purchase_detail：采购需求
wms_purchase：采购单
wms_ware_order_task：库存工作单
wms_ware_order_task_detail：库存工作单详情

#### 14.1.1整合 ware 服务

1、首先服务需要先注册到 Nacos 中，需要自己配置文件 配置对应的 nacos注册地址，

```
logging:
  level: 
    com.example: debug 		#开启日志
```

2、启动类需要开启 服务注册发现，开启远程服务调用

```java
@EnableFeignClients // 开启openfeign 远程服务调用
@EnableTransactionManagement //开启事务   //类上有可以不加
@MapperScan("com.atguigu.gulimall.ware.dao") //mapper包扫描  //类上有可以不加
@EnableDiscoveryClient //服务注册发现
```

#### 14.1.2 模糊查询仓库列表

前端： 库存系统/仓库维护，提供模糊查询仓库信息，具体对于操作的是`wms_ware_info` 表,	参数为key

```java
//ware/WareInfoController：
@RequestMapping("/list")
public R list(@RequestParam Map<String, Object> params){
    PageUtils page = wareInfoService.queryPage(params);
    return R.ok().put("page", page);}
@Override // ware/WareInfoServiceImpl
public PageUtils queryPage(Map<String, Object> params) { // 传入分页信息
    QueryWrapper<WareInfoEntity> wrapper = new QueryWrapper<>();
    // 查询关键字
    String key = (String) params.get("key");
    if (!StringUtils.isEmpty(key)) {
        // 仓库编号、仓库名字、仓库地址、区域编号
        wrapper.eq("id", key).or().like("name", key).or().like("address", key).or().like("areacode", key);}
    // 执行
    IPage<WareInfoEntity> page = this.page( new Query<WareInfoEntity>().getPage(params),wrapper );
    return new PageUtils(page);}
```

### 14.2 商品库存&创建采购需求

#### 14.2.1 商品库存

库存系统/商品库存，前端参数： **wareId、skuId** ，进行查询对应的表是 wms_ware_sku ，参数不是key了

模糊查询:

```java
//ware/WareSkuController：
@RequestMapping("/list")
public R list(@RequestParam Map<String, Object> params){
    PageUtils page = wareSkuService.queryPage(params);
    return R.ok().put("page", page);
}
//WareSkuServiceImpl:
 @Override
    public PageUtils queryPage(Map<String, Object> params) {
        QueryWrapper<WareSkuEntity> queryWrapper = new QueryWrapper<>();
        // 取出请求的参数 组装条件进行查询
        String skuId = (String) params.get("skuId");
        if (!StringUtils.isEmpty(skuId)) {
            queryWrapper.eq("sku_id",skuId);
        }
        String wareId = (String) params.get("wareId");
        if (!StringUtils.isEmpty(wareId)) { queryWrapper.eq("ware_id",wareId); }
        IPage<WareSkuEntity> page = this.page(new Query<WareSkuEntity>().getPage(params), queryWrapper);
        return new PageUtils(page);}
```

#### 14.2.2 查询采购需求

参数有key和status、wareId，选择 仓库、状态、 以及其他关键字 查询出对应的数据， 查询的是 `wms_purchase_detail`表

```java
//PurchaseDetailController
@RequestMapping("/list")
public R list(@RequestParam Map<String, Object> params){
    PageUtils page = purchaseDetailService.queryPage(params);
    return R.ok().put("page", page);
}
//PurchaseDetailServiceImpl:
  @Override
    public PageUtils queryPage(Map<String, Object> params) {
        QueryWrapper<PurchaseDetailEntity> wrapper = new QueryWrapper<>();
        // 取出key
        String key = (String) params.get("key");
        // key 主要查询条件 
        if (!StringUtils.isEmpty(key)) { wrapper.and(w ->w.eq("purchase_id",key).or().eq("sku_id",key)); }
        String status = (String) params.get("status");
        if (!StringUtils.isEmpty(status)) { wrapper.and(w ->w.eq("status",status)); }
        String wareId = (String) params.get("wareId");
        if (!StringUtils.isEmpty(wareId)) { wrapper.and(w -> w.eq("ware_id",wareId)); }
        IPage<PurchaseDetailEntity> page = this.page(new Query<PurchaseDetailEntity>().getPage(params), wrapper);
        return new PageUtils(page);
    }
```

### 14.3 合并需求&领取采购单&完成采购

#### 14.3.1  查未领取的采购单

<img src="/image-20201024131822802.png" style="zoom: 67%;" />

采购需求-》合并-》采购单-》分配人员-》人员领取

```
//PurchaseController
 @RequestMapping("/unreceive/list")
    public R unreceivelist(@RequestParam Map<String, Object> params){
        PageUtils page = purchaseService.queryPageUnreceivePurchase(params);
        return R.ok().put("page", page);}
//PurchaseServiceImpl:
  @Override
    public PageUtils queryPageUnreceivePurchase(Map<String, Object> params) {
        IPage<PurchaseEntity> page = this.page(new Query<PurchaseEntity>().getPage(params),
                new QueryWrapper<PurchaseEntity>().eq("status",0).or().eq("status",1));
        return new PageUtils(page);}
```

#### 14.3.2 合并采购需求

选中点击批量操作，然后弹出已有采购单，可以选择一个点确定，格式为（用户:电话号），如果不选择整单直接点击确定，将自动新建一个采购单；

![image-20201024132207468](/image-20201024132207468.png)

合并采购需求成采购单：

如果没有带过来采购单id，先新建采购单
然后修改【采购需求】里对应的**【采购单id、采购需求状态】**，即purchase_detail表
采购需求是purchase_detail表，指明采购单每项要采购的sku；采购单是purchase表，里有创建时间和分类人员等信息。采购单由多个采购需求组成
采购单页面分配采购成功后应该刷新页面，或者说不能重复分配采购需求给不同的采购单（或者说是更新操作）

请求参数：

```java
{ purchaseId: 1, //采购单id
  items:[1,2,3,4] //合并项即采购需求id的集合}
```

那就建立对应的 Vo 用来接收请求参数

```
@Data
public class MergeVo {
    private Long purchaseId;
    private List<Long> items;
}
```

PurchaseController

```java
// 合并采购
///ware/purchase/merge
@PostMapping("/merge")
public R merge(@RequestBody MergeVo mergeVo) {
    purchaseService.mrgePurchase(mergeVo);
    return R.ok();
}
```

PurchaseServiceImpl

```java
  @Transactional
    @Override
    public void mergePurchase(MergeVo mergeVo) {
        Long purchaseId = mergeVo.getPurchaseId();  // 拿到采购单id
        // 采购单 id为空 新建
        if (purchaseId == null ) {
            PurchaseEntity purchaseEntity = new PurchaseEntity();
            // 状态设置为新建
            purchaseEntity.setStatus(WareConstant.PurchaseStatusEnum.CREATED.getCode());
            purchaseEntity.setCreateTime(new Date());
            this.save(purchaseEntity);
            // 拿到最新的采购单id
            purchaseId = purchaseEntity.getId();
        }
        //TODO 确认采购是 0 或 1 才可以合并
        // 拿到合并项 **采购需求的id**
        List<Long> items = mergeVo.getItems();   //采购需求ids
        Long finalPurchaseId = purchaseId;          //表常量，不可变
        List<PurchaseDetailEntity> collect = items.stream().map(i -> {
            PurchaseDetailEntity detailEntity = new PurchaseDetailEntity();   // 采购需求
            // 通过采购单id 查询到 采购信息对象
            PurchaseEntity byId = this.getById(finalPurchaseId);  //用常量
                detailEntity.setStatus(WareConstant.PurchaseDetailStatusEnum.ASSIGNED.getCode()); // 设置为已分配
            detailEntity.setId(i);
            detailEntity.setPurchaseId(finalPurchaseId);    // 设置采购单id
            return detailEntity;
        }).collect(Collectors.toList());
        // 根据id批量更新
        detailService.updateBatchById(collect);
//         再次合并的话 更新修改时间，// 可以通过mp的@TableField(fill=FieldFill.INSERT_UPDATE)来完成，
        PurchaseEntity purchaseEntity = new PurchaseEntity();
        purchaseEntity.setId(purchaseId);
        purchaseEntity.setUpdateTime(new Date());
        this.updateById(purchaseEntity);
    }
```

采购需求和采购单对应 status 状态有比较多的选择，也是在common里定义了两个常量

```java
public class WareConstant {
    /** 采购单状态枚举 */
    public enum PurchaseStatusEnum{
        CREATED(0,"新建"),ASSIGNED (1,"已分配 "),RECEIVE(2,"已领取")
        ,FINISH(3,"已完成"),HASERROR(4,"有异常");
        private int code;
        private String msg;
        PurchaseStatusEnum(int code ,String msg){
            this.code=code;
            this.msg=msg;
        }
        public int getCode(){
            return  code;
        }
        public String getMsg(){
            return msg;
        }
    }
    public enum  PurchaseDetailStatusEnum{
        CREATED(0,"新建"),ASSIGNED(1,"已分配"),
        BUYING(2,"正在采购"),FINISH(3,"已完成"), HASERROR(4,"采购失败");
        private int code;
        private String msg;
        PurchaseDetailStatusEnum(int code,String msg) {
            this.code = code;this.msg = msg;
        }
        public int getCode() { return code; }
        public void setCode(int code) { this.code = code; }
        public String getMsg() { return msg; }
        public void setMsg(String msg) { this.msg = msg; }
    }
}
```

在yml中设置日期格式；



#### 14.3.2 领取采购单

API：https://easydoc.xyz/doc/75716633/ZUqEdvA4/vXMBBgw1


采购人员通过 APP 点击采购，通过采购 单Id 查询出采购相关信息，只有采购单是新建或以领取状态时，才更新采购单的状态，并通过采购 单Id 将采购需求的状态设置成 **正在采购**

后台系统里没有领取采购单这个功能，通过postman模拟手动领取采购单，还有每个采购人只能领取相应的采购单这个功能没法做；

POST localhost:88/api/ware/purchase/received，参数：[1,2,3,4]			//**采购单id**![image-20201024140120849](/image-20201024140120849.png)

PurchaseController

```java
@PostMapping("/received")
public R received(@RequestBody List<Long> ids) {
    purchaseService.received(ids)
    return R.ok();}
```

PurchaseServiceImpl

```java
  @Override
    public void received(List<Long> ids) {  //采购单ids
        // 没有采购单直接返回，否则会破坏采购单
        if (ids == null || ids.size() == 0) { return; }
        // 1.确认当前采购单是已分配状态 // 优化成查询list
        List<PurchaseEntity> purchaseEntityList = (List<PurchaseEntity>) this.listByIds(ids);
        purchaseEntityList = purchaseEntityList.stream()
        .filter(item -> item.getStatus() == WareConstant.PurchaseStatusEnum.ASSIGNED.getCode()
         || item.getStatus() == WareConstant.PurchaseStatusEnum.CREATED.getCode())  //新建或是已分配的
      .map(item -> { item.setStatus(WareConstant.PurchaseStatusEnum.RECEIVE.getCode());   //被领取之后重新设置采购单状态
                            item.setUpdateTime(new Date());
                            return item;
                        }).collect(Collectors.toList());
        this.updateBatchById(purchaseEntityList);
        // 别用eq，得用in
        List<PurchaseDetailEntity> purchaseDetailEntityList = purchaseDetailService.list(   //分页再用list包一层
                new QueryWrapper<PurchaseDetailEntity>().in("purchase_id", ids));   //也可自定义方法写到采购需求里实现
        // 更改采购需求的状态
        purchaseDetailEntityList = purchaseDetailEntityList.stream().map(e-> {
            e.setStatus(WareConstant.PurchaseDetailStatusEnum.BUYING.getCode());return e;}
        ).collect(Collectors.toList());
        purchaseDetailService.updateBatchById(purchaseDetailEntityList);
    }
```

```
 //PurchaseDetailServiceImpl
 @Override
    public List<PurchaseDetailEntity> listDetailByPurchaseId(Long id) {
        List<PurchaseDetailEntity> entities = this.list(new QueryWrapper<PurchaseDetailEntity>().eq("purchase_id", id));
        return entities;
    }
```

#### 14.3.3 完成采购

采购人员，发送哪些采购什么完成状态；wms_ware_sku表，采购表，采购需求表

- 采购项都完成的时候采购单为完成
- 采购项完成时增加库存
- 增加库存时要判断原来是否有库存以区分insert和update
- 加上分页插件

- POST localhost:88/api/ware/purchase/done

请求参数：（如果比较多，那么底考虑设计一个 VO ）

```
{  id: 6,//采购单id
    items: [  //完成/失败的需求详情
        {itemId:12,status:3,reason:"完成"},{itemId:13,status:4,reason:"失败"}]  //采购需求id
}
```

PurchaseController：

```java
@PostMapping("/done")
public R finish(@RequestBody PurchaseDoneVo doneVo) {
    purchaseService.done(doneVo);
    return R.ok();
}
```

Vo

```java
@Data
public class PurchaseDoneVo {
    /** 采购单id*/
    @NotNull
    private Long id;
    /** 采购项(需求) */
    private List<PurchaseItemDoneVo> items;
    public PurchaseDoneVo() {}
}
```

```java
@Data//完成/失败的需求详情,
public class PurchaseItemDoneVo {
     //* 采购单id
    private Long itemId;
     //* 状态
    private Integer status;
    private String reason;
}
```

PurchaseServiceImpl

```java
@Transactional
@Override
public void done(PurchaseDoneVo doneVo) {
    // 采购单id
    Long id = doneVo.getId();
    // 2、改变采购项目的状态
    Boolean flag = true;
    List<PurchaseItemDoneVo> items = doneVo.getItems();
    List<PurchaseDetailEntity> updates = new ArrayList<>();
    for (PurchaseItemDoneVo item : items) {     //也可以写成stream.map
        PurchaseDetailEntity detailEntity = new PurchaseDetailEntity();
        // 如果采购失败
        if (item.getStatus() == WareConstant.PurchaseDetailStatusEnum.HASERROR.getCode()) {
            flag = false;
            detailEntity.setStatus(item.getStatus());
        } else {
             // 3、将成功的采购需求进行入库
                PurchaseDetailEntity entity = detailService.getById(item.getItemId());
                wareSkuService.addStock(entity.getSkuId(),entity.getWareId(),entity.getSkuNum()); //不要直接传，最少知道原则
                detailEntity.setStatus(WareConstant.PurchaseDetailStatusEnum.FINISH.getCode());  //改采购需求的状态
        }
        detailEntity.setId(item.getItemId());
        updates.add(detailEntity);
    }
    // 批量更新
    purchaseDetailService.updateBatchById(updates);
    // 1、改变采购单状态
    PurchaseEntity purchaseEntity = new PurchaseEntity();
    purchaseEntity.setId(id);
    // 设置状态根据变量判断
    purchaseEntity.setStatus(flag?WareConstant.PurchaseStatusEnum.FINISH.getCode():WareConstant.PurchaseStatusEnum.HASERROR.getCode());
    purchaseEntity.setUpdateTime(new Date());
    this.updateById(purchaseEntity);
}
```

addStock方法

```java
   @Override
    public void addStock(Long skuId, Long wareId, Integer skuNum) {
 List<WareSkuEntity> wareSkuEntities = wareSkuDao.selectList(new QueryWrapper<WareSkuEntity>()
                                    .eq("sku_id", skuId).eq("ware_id", wareId));
        //没有这个用户 那就新增
        if(wareSkuEntities == null || wareSkuEntities.size() == 0) {
            WareSkuEntity wareSkuEntity = new WareSkuEntity();
            // 根据属性值设置
            wareSkuEntity.setSkuId(skuId);
            wareSkuEntity.setStock(skuNum);
            wareSkuEntity.setWareId(wareId);
            wareSkuEntity.setStockLocked(0);
            // TODO 远程查询sku的名字 如果失败整个事务不需要回滚
            try {
                // 远程调用 根据 skuid进行查询
                R info = productFeignService.info(skuId);
                Map<String,Object> map = (Map<String, Object>) info.get("skuInfo");
                if (info.getCode() == 0) {
                    wareSkuEntity.setSkuName((String) map.get("skuName"));
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
            wareSkuDao.insert(wareSkuEntity);
        }else {
            wareSkuDao.addStock(skuId,wareId,skuNum);     // 有该记录那就更新，加上原来的库存
        }
    }
```

WareSkuDao:

```
@Mapper
public interface WareSkuDao extends BaseMapper<WareSkuEntity> {
    void addStock(@Param("skuId") Long skuId, @Param("wareId") Long wareId, @Param("skuId1") Integer skuId1);//sql和@Param可以自动生成
}
```

addStock 具体实现:	加上原库存

```java
<insert id="addStock">
UPDATE `wms_ware_sku` SET stock=stock+#{skuNum} WHERE sku_id=#{skuId} AND ware_id=#{wareId}  //在dao方法里sql和@Param可以自动生成
</insert>
```

配置分页：config/

```
@Configuration
@EnableTransactionManagement  //开启使用事务
@MapperScan("com.example.gulimall.ware.dao")
public class MyBatisConfig {
    @Bean   //引入分页插件
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        return paginationInterceptor;
    }
}
```

配置远程调用：

```
@FeignClient("gulimall-product")
public interface ProductFeignService {
    @RequestMapping("/product/skuinfo/info/{skuId}")   //也可以加个api/直接发给网关路由到product
    public R info(@PathVariable("skuId") Long skuId);
}
```

### **14.4**	Spu规格维护

#### 1、Spu规格维护

之前创建sku的时候指定了一些基本属性（规格参数），后期怎么回显修改，单表操作，根据 spu_id 查询出 `pms_product_attr_value` 表的信息

前端项目遇到的问题： 需要自己去 router目录下找到 index.js 增加改行配置，或/product/attrupdate

在 Spu管理上 点击规格后查询出相关规格信息

AttrController:

```java
@GetMapping("/base/listforspu/{spuId}")
public R baseAttrListforspu(@PathVariable("spuId") Long spuId) {
    List<ProductAttrValueEntity> productAttrValueEntity = productAttrValueService.baseAttrListforspu(spuId);
    return R.ok().put("data",productAttrValueEntity);}
```

ProductAttrValueServiceImpl: 

```java
@Override
public List<ProductAttrValueEntity> baseAttrListforspu(Long spuId) {
    // 1、根据spuid进行查询
  List<ProductAttrValueEntity> attrValueEntities = this.baseMapper.selectList(new QueryWrapper<ProductAttrValueEntity>().eq("spu_id", spuId));
    return attrValueEntities;
}
```

#### 2、 Spu更新操作

**具体需求：**

根据spu_id 查询出规格信息后 ，修改对应信息 提交后会发送一个post请求，并同时带上请求参数

```json
[{
	"attrId": 7,
	"attrName": "入网型号",
	"attrValue": "LIO-AL00",
	"quickShow": 1
}, {
	"attrId": 14,
	"attrName": "机身材质工艺",
	"attrValue": "玻璃",
	"quickShow": 0
}, {
	"attrId": 16,
	"attrName": "CPU型号",
	"attrValue": "HUAWEI Kirin 980",
	"quickShow": 1
}]
```

刚好这四个参数和实体类的一致，就不需要创建 Vo来接收

AttrController

```java
@RequestMapping("/update/{spuId}")
//@RequiresPermissions("product:attr:update")
public R updateSpuAttr(@PathVariable("spuId") Long spuId,
                       @RequestBody List<ProductAttrValueEntity> productAttrValueEntityList){

    productAttrValueService.updateSpuAttr(spuId,productAttrValueEntityList);
    return R.ok();
}
```

ProductAttrValueServiceImpl: 

```java
/**
 * 先删除 后更新
 * @param spuId
 * @param productAttrValueEntityList
 */
 @Transactional
@Override
public void updateSpuAttr(Long spuId, List<ProductAttrValueEntity> productAttrValueEntityList) {
    // 1、根据spuid删除记录
    this.baseMapper.delete(new QueryWrapper<ProductAttrValueEntity>().eq("spu_id",spuId));

    // 2、遍历传递过来的记录 设置 spuId
    List<ProductAttrValueEntity> collect = productAttrValueEntityList.stream().map(item -> {
        item.setSpuId(spuId);
        return item;
    }).collect(Collectors.toList());
    // 3、批量保存
    this.saveBatch(collect);
}
```





## 分布式基础篇总结

**1、分布式基础概念**

- 微服务：独立自治、注册中心（Nacos）、配置中心（Nacos Cofig）、远程调用、Feign、网关

**2、基础开发**

- springboot2.0基于spring5做的最大变化：引入了reate反应式编程，带来了webfux，非常容易创建高性能的web应用
- 人人开源提供的逆向工程（最方便的）：基于Mybatis-Plus逆向工程做的改版，但是更多了编写了Controller和vue的基础组件
- 引入了阿里云的对象存储：体会了调用第三方服务应该怎么做，对照人家的sdk文档、接口文档来开发

**3、环境**

- Linux+Docker的方式，用Docker部署MySQL、Redis；用Vagrant创建虚拟机、Linux、Docker、MySQL、Redis、逆向工程&人人开源

**4、开发规范**

- 数据校验JSR303、全局异常处理、全局统一返回、全家跨越处理
- 枚举状态、业务状态、VO与TO与PO划分、逻辑删除
- Mybatis-Plus提供了逻辑删除功能 ，Lombok:@Data、@Slf4j