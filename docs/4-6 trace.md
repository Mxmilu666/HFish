**溯源功能支持版本：3.1.4**

### 什么是溯源

溯源，一般是在攻防中，用于挖掘攻击者身份，通过资产面，回溯等判断攻击者身份。

在近几年攻防演练中，更加特指了解对方的身份信息。

HFish作为蜜罐软件，提供被攻击时候的正常防卫需求。当攻击者扫描、攻击或者恶意连接HFish的蜜罐时，HFish提供包括但不限于：
通过现有攻击工具漏洞，在合理范围内对攻击者进行信息提取、溯源等必要的攻击防御信息收集手段。



### 纯内网部署，能否溯源

**安全业务层面：**

内网没有太大的溯源意义，通过IP地址分配，dhcp查询，以运维手段可以很快了解到来源IP的所属用户。所以内网并没有非常大的意义

**溯源技术层面**：

HFish共支持三种溯源手段。

Mysql反制（支持内网，能够对攻击者机器做任意文件读取）

厂商vpn蜜罐反制（支持内网，能够获得windows机器登陆的微信号信息与桌面截图）

web型蜜罐溯源（不支持内网）这种溯源方式，在溯源时，会自动获取攻击者的出口IP进行标记。



### HFish溯源实现原理与测试方法

HFish共支持三种溯源手段。

##### 1.Mysql反制（支持内网，能够对攻击者机器做任意文件读取）

通过MySql客户端漏洞，当攻击者使用8.0以下的**Mysql客户端直连**HFish的Mysql蜜罐的时候。可以触发任意文件读取的能力，读取Mysql客户端所在机器上的任意一个文件。

注意：

1. 一定是Mysql客户端直连。不兼容诸如navicat这类工具。

2. 在节点管理中，可以设置mysql任意文件读取的具体读取路径

   ![image-20220807141822892](http://img.threatbook.cn/hfish/image-20220807141822892.png)

##### 2.厂商vpn蜜罐反制（支持内网，能够获得windows机器登陆的微信号信息与桌面截图）

通过蜜罐页面提供可下载二进制，攻击者下载执行后，会获取攻击者的微信与屏幕截图

该部分记录的是连接IP，所以您在内网中测试，测试结果不会回传到云端共享，可放心测试。

##### 测试步骤：

a. 直接在HFish中添加深信服vpn蜜罐。

b.下载蜜罐页面中的exe并运行

<img src="/var/folders/6b/pkkyf9hs46v78zfksx2fqrq80000gn/T/com.tencent.WeWorkMac/wecom-temp-c27ab2f3a541d0a7be62a1e9391c8db6.png" alt="wecom-temp-c27ab2f3a541d0a7be62a1e9391c8db6" style="zoom: 33%;" />

c. 进入攻击来源，搜索IP或者直接点击溯源信息按钮。查看详情

<img src="http://img.threatbook.cn/hfish/image-20220807123404778.png" alt="image-20220807123404778" style="zoom:50%;" />

<img src="/Users/maqian/Library/Application Support/typora-user-images/image-20220807124218394.png" alt="image-20220807124218394" style="zoom:50%;" />

#### 

##### 3.web型蜜罐溯源。（不支持内网溯源）

HFish所有的web页面蜜罐，包括自定义蜜罐，都可以在该情况下进行溯源。

对扫描器、webshell管理器、java反序列化反制。该部分反制数据会显示攻击者的出口IP。

可以拿到包括**机器架构，微信账号、QQ账号、QQ音乐账号、百度云账号，浏览器浏览记录、history、whoami**等信息

⚠️注意：处于对产品技术保密的原因。我们不会公开可以对扫描器、webshell管理器能够进行溯源反制，也不会提供相关的应用教程。

如果您确实有测试需求，请用您的企业邮箱，说明您的公司，身份，测试需求原因，邮件honeypot@threatbook.cn发送申请。我们只支持企业安全部门的测试申请与协助。



##### 4 .Java 远程调用蜜罐溯源（不支持内网溯源）

Java远程调用蜜罐是HFish内置的一种蜜罐服务。添加到节点上即可食用

![image-20220807144001639](http://img.threatbook.cn/hfish/image-20220807144001639.png)

当攻击者使用java反序列化工具对你的Java远程调用蜜罐进行攻击时，由于该蜜罐内置了Java反序列化工具的漏洞利用方式。

因此可以拿到包括**机器架构，微信账号、QQ账号、QQ音乐账号、百度云账号，浏览器浏览记录、history、whoami**等信息

⚠️注意：处于对产品技术保密的原因。我们不会提供相关的应用教程。

如果您确实有测试需求，请用您的企业邮箱，说明您的公司，身份，测试需求原因，邮件honeypot@threatbook.cn发送申请。我们只支持企业安全部门的测试申请与协助。



### 溯源信息查看

进入攻击来源，搜索IP或者直接点击溯源信息按钮。查看详情

<img src="http://img.threatbook.cn/hfish/image-20220807123404778.png" alt="image-20220807123404778" style="zoom:50%;" />

<img src="/Users/maqian/Library/Application Support/typora-user-images/image-20220807124218394.png" alt="image-20220807124218394" style="zoom:50%;" />

#### 



**写在最后**：

朋友们，理智看待溯源功能

不会是有一个人访问你的蜜罐页面，我们就可以控制它的权限。

HFish在对方有恶意与攻击行为的时候，才会通过当前掌握到的攻击工具/其他工具漏洞触发溯源反制。

最后，我们应该是极少数的，公开详细解读自己溯源功能原理的厂商，各位且行且珍惜....