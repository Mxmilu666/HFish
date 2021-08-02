### 配置工作总览

!> 在完成控制端的部署后，我们还需要几个配置工作让整个系统运行起来。

```wiki
1. 添加【蜜罐服务】，让系统获得相应蜜罐的能力。
2. 创建【服务模板】，模板是数个蜜罐服务的集合，在大规模部署的环境中，模板可以帮我们更高效管理我们的集群。
3. 【增加节点】，单机版自带一个节点，集群版不带节点。系统至少需要一个节点才能正常运行。
4. 为蜜罐节点选择【服务模板】，选择了什么服务模板，蜜罐节点就具有了模板中的蜜罐能力。
```



### 添加蜜罐服务

!> 控制端运行起来后，我们需要做的第一件事情就是下载蜜罐服务。您有2种方法可以添加蜜罐服务，任选一种即可。

> 【服务管理】中下载蜜罐服务

```wiki
1. 登陆控制端后，打开【服务管理】页面，首次登陆页面上蜜罐服务都是灰色。
2. 选择自己需要的蜜罐服务进行下载。
```

<img src="http://img.threatbook.cn/hfish/20210616164014.png" alt="image-20210616164012531" style="zoom:50%;" />



> 手动上传蜜罐服务包

**当您处于离线环境不方便在线安装情况下，您可以使用手动上传安装的方式。**

```wiki
1. 下载最新官方服务包 http://img.threatbook.cn/hfish/svc/services-<% version %>.tar.gz
2. 在新增服务页面上选择该服务包上传。
```

<img src="http://img.threatbook.cn/hfish/20210616165216.png" alt="image-20210616165214921" style="zoom:50%;" />



> *上传自定义web蜜罐

​	如果您有自定义web蜜罐的需求，我们为您准备了开发样例，您可以参考我们的[文档](https://hfish.io/#/function?id=web%e8%9c%9c%e7%bd%90%e8%87%aa%e5%ae%9a%e4%b9%89%e5%bc%80%e5%8f%91)完成蜜罐的开发工作后，进行上传。
​	当然您也可以在社区中寻找其它用户开发好的蜜罐上传后使用。



### 自定义web蜜罐页面

#### 通过样例了解功能实现方式

> web蜜罐样例

```wiki
# 下载自定义web蜜罐样例
http://img.threatbook.cn/hfish/svc/web-demo.zip
```



> 解压后获得两个文件

```wiki
# index.html
# portrait.js
```



> index.html文件中的代码功能

```wiki
<form>中的代码明确了页面上账密表单的提交方式。
具体利用方式参考下文[制作全新的登陆页面]

<script>中的代码明确了调用jsonp的方式。
```



> portrait.js 文件中的代码功能

```wiki
这个文件是jsonp溯源功能的利用代码，攻击者在已登录其他社交平台的情况下，蜜罐可以获得部分社交平台的账号信息。

本代码因为利用了浏览器的漏洞，有一定的时效性，随着攻击者更新自己的浏览器，利用代码可能失效。并有可能让攻击者在访问该页面时，触发杀毒软件的报警。

在利用代码失效后，您可以选择删除index.html中的利用代码。

同时请关注我们的官网（ https://hfish.io ）和x社区( https://x.threatbook.cn )，等待我们和社区用户更新漏洞利用代码，并替换本文件和index内的利用代码，恢复溯源能力。
```



### 制作全新的登陆页面

我们可以自己制作一个全新的登陆页面，通过替换表单元素实现“定制开发”

```shell
- 修改主页文件名为index.html

- 按照下面图片的要求，修改表单元素。
```

![蜜罐web页面表格元素](http://img.threatbook.cn/hfish/20210728213641.png)



### 打包并上传到蜜罐的管理后台

> 打包所有静态文件资源

把所有的静态文件文件打包名为“service-xxx.zip”文件。包括但不限于index.html 、portrait.js 和其它格式的静态文件、文件夹。

注意：文件命名为规范格式前缀必须为“service-” ； “xxx”可以自定义，但不能为“web”和“root”；必须压缩为.zip格式文件

![image-20210508222121613](http://img.threatbook.cn/hfish/20210728213740.png)



> 打开server后台

![image-20210508213915879](http://img.threatbook.cn/hfish/20210728213815.png)



> 配置新增服务页面

![image-20210508221316072](http://img.threatbook.cn/hfish/20210728213852.png)



> 自定义web蜜罐添加成功

![image-20210508222350694](http://img.threatbook.cn/hfish/20210728213909.png)