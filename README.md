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
主要步骤中最耗费时间的是备案，笔者准备备案资料2-3天，备案审核整整一个星期，时间仅供参考  
这里用的是阿里云全家桶服务  
* 若是学生身份看这里：200大洋前后，一年的ecs服务器使用期，细节自行了解阿里云云翼计划。  
* 不是学生身份看这里：注册后阿里云会赠送一个月的ecs服务器，再购买两个月（约120大洋）即可（必须购足三个月，否则无备案服务号，也将无法备案）
---

### 域名
入口：阿里云控制台=>域名与安全=>域名=>域名购买  
.com || .cn || .net 年费100+，笔者购10块购买了.top域名  
支付成功后，在控制台导入后列表中会出现该域名，然后实名认证   
域名解析（将域名和ecs的公网ip绑定起来）  
安全组设置（简而言之，配置可访问ecs的ip段、端口段）：端口80/80,88/88，ip配置：0.0.0.0/0  

说明：  
* 部分域名后缀是无法备案的 [详情见这里](http://www.miitbeian.gov.cn/publish/query/indexFirst.action)  
* 域名购买成功后，立即申请备案初审是无法通过的。原因：域名未入库，工信部无法核实，大概需要3个工作日  
* 在使用阿里云产品的过程中遇到任何问题，可以直接找在线客服或者电话人工客服，响应速度算是业界良心了  
---

### ECS
笔者选择的是最低配置：1g内存 + 1g CPU + 1Mbps带宽  
考虑到Linux的学习成本，镜像选择了win2012R2_64位  

---

### 备案
当你有了实名且已入库的域名 + 正常状态的ecs + 备案服务号即可申请备案了  
申请备案要很多资料，不同省份不同规则 [详情见这里](https://help.aliyun.com/knowledge_detail/36895.html?spm=5176.8087400.600752.1.58d815c9T0iS4d)  
这是备案成功后的反馈，你们也可以根据它大概知道自己需要准备哪些资料：  
![默认图片](https://raw.githubusercontent.com/ppp000/deploy/master/img-storage/1527082424(1).jpg)  
备案的一个原则是严格按照官方的要求去做，因为一个信息的不合规的后果便是重新申请备案，有一个时间成本在这里  

---

### 环境搭建
阿里云创建的默认实例是40GB的固态磁盘，通过公网ip可远程连接到云服务器
  

#### `iis`
远程连接过程可设置本地磁盘映射到ecs中，以达到传输文件的目的 [详情见这里](https://jingyan.baidu.com/album/148a192185f0ae4d71c3b138.html?picindex=1)  
连接成功后，开始菜单旁边的服务器管理器，添加角色,具体角色功能如图（可以理解为安装iis）：  
![默认图片](https://raw.githubusercontent.com/ppp000/deploy/master/img-storage/1527121199(1).jpg)  
静态内容一定要勾选上！笔者当时一时疏忽没勾选上出现的问题：服务请求一切正常，本地apache访问也一切正常，发布到生产环境后静态资源出现问题！折腾了几天才发现这个没勾上  

基于这个阿里云的最低配置的服务器在安装角色的过程中会出现内存不足的情况 [内存不足解决方案](https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&tn=baidu&wd=%E8%99%9A%E6%8B%9F%E5%86%85%E5%AD%98%20%E8%AE%BE%E7%BD%AE&oq=md%25E7%25BC%2596%25E8%25BE%2591%25E5%2599%25A8&rsv_pq=d5df6d5d0000e893&rsv_t=e0d8%2FdixoNmMsQ6zVIapAzegp1iKzCR0YmZw10L6%2BuvU4cfu8QeRRr2Dt6E&rqlang=cn&rsv_enter=1&inputT=213796&rsv_sug3=28&rsv_sug1=19&rsv_sug7=100&bs=md%E7%BC%96%E8%BE%91%E5%99%A8)  
* 若成功安装，打开服务器的ie：http://localhost 会出现iis的初始化界面。  
* 否则，重新安装

#### `dist`
成功安装iis后，打开iis管理器，添加网站，配置物理路径、网站名（可空）、ip、端口等  

#### `nodejs + pm2`
[node下载](https://npm.taobao.org/mirrors/node/v10.1.0/)  
下载好对应安装包后上传到服务器，node安装步骤和windows一致，没什么去很大区别

成功安装nodejs、npm后：  
* npm install pm2 -g  
* 跳转到项目的服务端文件夹
* pm2 start 具体的服务端js文件

nodejs + pm2配合使用理论上可以达到ecs在不关机的情况下，node服务会一直连接

#### `mysql`
[mysql下载](https://dev.mysql.com/downloads/installer/)  
32位安装包下载还是可以安装64位的mysql服务的  
安装过程同理，和windows没什么很大区别

连接mysql有可能会报：E RROR 1130: Host 'XXXXXX' is not allowed to connect to this MySQL server  
hack：  
* 打开mysql控制台
* ```mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION```;  

# 其它坑
对于前端开发人员来说，iis可能是很陌生的，但这个配置又很重要！  

有时候dist打包好会出现js文件404的情况，两个处理方式：  
* 正确配置webpack，重新打包  
* 在build好的index.html：`<base href="./">`
