# 项目演示地址
[system](http://www.ppp000.top)

# 教程简介
主要记录基于 ng + nodejs + mysql 开发的系统部署阿里云服务器的过程

# 准备
1、云服务器  
2、域名  
3、各种安装包  
4、自行了解部分词汇含义：ecs、备案、dns解析、安全组、公网ip，私网ip，iis

# 主要步骤
关键词：angular + nodejs + mysql + ecs + win2012R2_64位 + iis + pm2  
主要步骤中最耗费时间的是备案，笔者准备备案资料2-3天，备案审核整整一个星期，这个时间仅供参考   
这里用的是阿里云全家桶服务  
若是学生身份看这里：200大洋前后，一年的ecs服务器使用期，细节自行了解阿里云云翼计划。  
不是学生身份看这里：注册后阿里云会赠送一个月的ecs服务器，再购买两个月（约120大洋）即可（必须购足三个月，否则无备案服务号，也将无法备案）

### 域名
阿里云控制台=>域名与安全=>域名=>域名购买  
.com || .cn || .net 年费100+，笔者购10块购买了.top域名  
支付成功后，在控制台导入后列表中会出现该域名，然后实名认证   
域名解析（将域名和ecs的公网ip绑定起来）  
安全组设置（简而言之，配置可访问ecs的ip段、端口段）：端口80/80,88/88，ip配置：0.0.0.0/0  

说明：1、部分域名后缀是无法备案的 [详情见这里](http://www.miitbeian.gov.cn/publish/query/indexFirst.action)  
2、域名购买成功后，立即申请备案初审是无法通过的。原因：域名未入库，工信部无法核实，大概需要3个工作日  
3、在使用阿里云产品的过程中遇到任何问题，可以直接找在线客服或者电话人工客服，响应速度算是业界良心了  

### ECS
笔者选择的是最低配置：1g内存 + 1g CPU + 1Mbps带宽    
考虑到Linux的学习成本，镜像选择了win2012R2_64位  


### 备案
当你有了实名且已入库的域名 + 正常状态的ecs + 备案服务号即可申请备案了  
申请备案要很多资料，不同省份不同规则 [详情见这里](https://help.aliyun.com/knowledge_detail/36895.html?spm=5176.8087400.600752.1.58d815c9T0iS4d)   
这是备案成功后的反馈，你们也可以根据它大概知道自己需要准备哪些资料：  
![默认图片](https://raw.githubusercontent.com/ppp000/deploy/master/img-storage/1527082424(1).jpg)  
备案的一个原则是严格按照官方的要求去做，因为一个信息的不合规的后果便是重新申请备案，有一个时间成本在这里  

### 环境搭建
阿里云创建的默认实例是40GB的固态磁盘，通过公网ip可远程连接到云服务器  
#### iis
远程连接过程可设置本地磁盘映射到ecs中，以达到传输文件的目的 [详情见这里](https://jingyan.baidu.com/album/148a192185f0ae4d71c3b138.html?picindex=1)  
连接成功后，开始菜单旁边的服务器管理器，添加角色,具体角色功能如图：  


#### nodejs
[node安装包下载地址](https://npm.taobao.org/mirrors/node/v10.1.0/)


# 部署过程中常见坑
iis、base、虚拟内存
