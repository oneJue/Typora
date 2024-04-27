 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/125668174)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408操作系统+Linux系统编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- [一：万维网概述](#_9)
- - [（1）万维网（WWW）](#1WWW_11)
  - [（2）URI和URL](#2URIURL_21)
  - - [A：URI和URL是什么](#AURIURL_23)
    - [B：URL的格式](#BURL_33)
- [二：超文本传输协议（HTTP）](#HTTP_60)
- - [（1）HTTP协议操作过程](#1HTTP_65)
  - [（2）HTTP协议特点](#2HTTP_95)
  - [（3）HTTP协议报文结构](#3HTTP_109)
  - - [A：request请求报文](#Arequest_117)
    - - [①：请求报文基本构成](#_119)
      - [②：请求方法](#_148)
    - [B：respond](#Brespond_178)
    - - [①：响应报文基本构成](#_179)
      - [②：HTTP常见状态码](#HTTP_220)
    - [C：HTTP常见Header（字段）](#CHTTPHeader_272)

# 一：万维网概述

## （1）万维网（WWW）

**万维网（WWW）：是一个资料空间，在这个空间中：一样有用的事物称为一样“资源”，并由一个全域“统一资源定位符”\(URL\)标识。这些资源通过超文本传输协议\(HTTP\)传送给使用者，而后者通过单击链接来获取资源。万维网的内核部分是由三个标准构成的**

- **统一资源定位符\(URL\)**：负责**标识万维网上的各种文档，并使每个文档在整个万维网的范围内具有唯一的标识符**URL
- **超文本传输协议\(HTTP\)**：一个应用层协议，它使用TCP连接进行可靠的传输，**HTTP是万维网客户程序和服务器程序之间交互所必须严格遵守的协议**
- **超文本标记语言（HTML）**：一种文档结构的标记语言，它**使用一些约定的标记对页面上的各种信息\(包括文字、声音、图像、视频等\)、格式进行描述**

## （2）URI和URL

### A：URI和URL是什么

- **URI\(Uniform Resource Identifier\)：统一资源标识符**——表示的是web上每一种可用的资源，如 HTML文档、图像、视频片段、程序等都由一个URI进行标识的。
- **URL\(Uniform Resource Locator\)：统一资源定位符**——URL是Internet上描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上。

**URL是URI的一个子集，URL是URI概念的一种实现方式。**

URI和URL都定义了资源是什么，但是URL还定义了如何访问资源，URL是一种具体的URL**。他不仅唯一标识资源，而且还提供了定位该资源的信息。因此URL是一种语义上的抽象概念，可以是绝对的也可以是相对的，但是URL必须提供绝对的定位信息**

### B：URL的格式

URL的格式如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210628224904419.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **协议方案名**：http或https协议
- **登录信息\(认证\)**：指定用户名和密码作为服务器端获取资源时的必要信息，此项为可选项，浏览器显示时会隐藏
- **服务器地址**：访问服务器时必须指明服务器地址，上图给出的只是方便人们记忆的网址，实际会由DNS（域名解析器）进行解析
- **服务器端口号**：指定服务器连接的网络端口号，也是一个可选项，其中有些端口号非常有名，属于强绑定了，如果用户省略则会使用默认的端口号
- **带层次的文件路径**：指定服务器的文件路径来定位指定的资源。和UNIX系统目录结构类似，**但这不是根目录，而是一个部署好的web根目录。**
- _（注意"\?“好前面就是基本的URL格式，如果需要传入参数，在”\?"号后面加入，以K-V形式传入）_
- **查询字符串**：百度搜索时就按照这种方式传参

另外，可以发现像`/ ? @ :`这样的字符在URL中是具有特殊的意义的，因此在传参时如果需要传入这样的字符，**就必须对其进行转义**。**转义的过程称为urlencode，其逆过程称为urldecode**

如下，如果输入普通字符，可以发现关键字未被转义  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210628230459124.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
如果输入特殊字符，发现进行了转义  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210628230555445.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **转义规则**：将需要转义的字符先转为16进制，然后从右向左取4位（不足4位直接处理），每2位做一位，在前面加上\%，编成`“%XY”`的格式

因此在编写服务器时，面对这些特殊字符，一定要做编码处理，下面是一个在线的编码/解码工具

- [urlencode工具](http://tool.chinaz.com/tools/urlencode.aspx)  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210628231200122.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

# 二：超文本传输协议（HTTP）

**超文本传输协议（HTTP）：HTTP定义了浏览器\(万维网客户进程\)怎样向万维网服务器请求万维网文档，以及服务器怎样把文档传送给浏览器。从层次的角度看，HTTP是面向事务的\(Transaction-oriented\) 应用层协议，它规定了在浏览器和服务器之间的请求和响应的格式与规则，是万维网上能够可靠地交换文件\(包括文本、声音、图像等各种多媒体文件\)的重要基础**

## （1）HTTP协议操作过程

**万维网大致工作过程如下图所示**

- 每个万维网站点都有一个服务器进程，它**不断地监听TCP的端口80\(默认\)**，当监听到连接请求后便与浏览器建立连接
- TCP连接建立后，浏览器就向服务器发送**请求获取某个Web页面的HTTP请求**
- 服务器收到HTTP请求后，将构建所请求Web页的必需信息，并**通过HTTP响应**返回给浏览器
- 浏览器再将信息进行解释，**然后将Web页显示给用户**。最后, TCP连接释放

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F992b804996934bd1a54d41c6cbe47434.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

**用户通过浏览器与服务器进行交互，遵循HTTP协议，有以下两类报文**

- **请求报文**：从Web客户端向Web服务器发送服务请求

- **响应报文**：响应报文从Web服务器对Web客户端请求的回答

**当用户单击鼠标后会发生如下事情**

- 浏览器分析URL
- 浏览器向DNS请求解析IP地址
- DNS解析出IP地址
- 浏览器与服务器建立TCP连接
- 浏览器发出取文件命令
- 服务器响应
- 释放TCP连接
- 浏览器显示

## （2）HTTP协议特点

**HTTP协议有如下两个最大的特点**

- **无连接**：HTTP建立与TCP之上，所以**是不关心TCP通信细节的**，TCP是否是面向连接的与HTTP是没有任何关系的。而且，当TCP连接建立成功之后，HTTP是没有必要在应用层再建立一个连接的，所以TCP建立好之后，HTTP直接向对方发HTTP请求\(request\)就可以了
- **无状态**：HTTP是一种**不保存状态协议**，其自身不对请求和相应之间的通信状态进行保存，也就是说在HTTP这个级别，协议对于发送过的请求或相应**都不会做持久化处理**

对于非持久连接，**每个网页元素对象的传输都需要单独建立一个TCP连接**；所谓持久连接，是指**万维网服务器在发送响应后仍然保持这条连接**，使同一个客户和服务器可以继续在这条连接上传送后续的HTTP请求与响应报文

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2d6c58ee7aa045fd88fb420816e3852c.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

## （3）HTTP协议报文结构

### 文章目录

- [一：万维网概述](#_9)
- - [（1）万维网（WWW）](#1WWW_11)
  - [（2）URI和URL](#2URIURL_21)
  - - [A：URI和URL是什么](#AURIURL_23)
    - [B：URL的格式](#BURL_33)
- [二：超文本传输协议（HTTP）](#HTTP_60)
- - [（1）HTTP协议操作过程](#1HTTP_65)
  - [（2）HTTP协议特点](#2HTTP_95)
  - [（3）HTTP协议报文结构](#3HTTP_109)
  - - [A：request请求报文](#Arequest_117)
    - - [①：请求报文基本构成](#_119)
      - [②：请求方法](#_148)
    - [B：respond](#Brespond_178)
    - - [①：响应报文基本构成](#_179)
      - [②：HTTP常见状态码](#HTTP_220)
    - [C：HTTP常见Header（字段）](#CHTTPHeader_272)

  
注意下面需要分析请求报头和响应报头的构成，需要使用到抓包工具，推荐使用fillder  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210630211924628.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

### A：request请求报文

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210628234110192.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

#### ①：请求报文基本构成

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210629002110722.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
在浏览器中输入百度的网址，选中相应数据，右侧显示中上半部分为请求报头，下半部分显示为相应报头，点击raw（原始数据）查看

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210630212114581.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
信息具体如下

```bash
GET https://www.baidu.com/ HTTP/1.1 #请求行
Host: www.baidu.com #请求的主机
Connection: keep-alive #长连接 HTTP可以选择不断开TCP，从而连接持久化
sec-ch-ua: " Not;A Brand";v="99", "Microsoft Edge";v="91", "Chromium";v="91"
sec-ch-ua-mobile: ?0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)#浏览器，操作系统信息
AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36 Edg/91.0.864.59
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Cookie: BIDUPSID=9F74B621908F780204441F86DCCE03A5; PSTM=1598331815; BAIDUID=9F74B621908F7802E2D41BCA0AE628E1:FG=1; BAIDUID_BFESS=9F74B621908F7802E2D41BCA0AE628E1:FG=1; BD_UPN=12314753; COOKIE_SESSION=0_1_1_1_0_0_0_0_1_0_0_0_0_0_0_1_0_1624878202_1624878201%7C1%230_1_1624878201%7C1; BDUSS=kFjVE4wOVphT2l-Ym82YnE5RHdJUlZTRW10MU5qM1lqS1p4eVUxaVNHR2pPUUZoRVFBQUFBJCQAAAAAAAAAAAEAAADjDoXG1-7Lp77NysfO0jYyMQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKOs2WCjrNlgR; BDUSS_BFESS=kFjVE4wOVphT2l-Ym82YnE5RHdJUlZTRW10MU5qM1lqS1p4eVUxaVNHR2pPUUZoRVFBQUFBJCQAAAAAAAAAAAEAAADjDoXG1-7Lp77NysfO0jYyMQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKOs2WCjrNlgR; BD_HOME=1; H_PS_PSSID=34099_34225_31253_34226_33848_34133_33607_34134_26350; sugstore=0; BA_HECTOR=ah85al8g252h0l05ui1gdoqqk0r #缓存信息
# 此行为空行


```

#### ②：请求方法

HTTP请求方法如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702204937234.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**1：GET方法**  
GET方法用来请求已经被URI识别的资源，指定的资源经过服务器解析后返回响应内容。比如请求的资源是文本，那么就会保持原样返回  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702205636685.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
使用GET方法请求的例子  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702205707695.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**2：POST方法**  
虽然用GET方法也可以传输实体的主体，但是一般不用GET方法进行传输，而是采用POST。虽然说POST的功能与GET很相似，但是POST的主要目的并不是获取响应的主体内容  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702205959957.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
使用POST方法请求的例子  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702210031423.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**GET和POST的区别**  
你现在打开我的这篇文文章阅读，浏览器就会发送GET请求给服务器，服务器则会返回相应的数据  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702210252421.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
而如果你在我的文章下面评论，点击提交后，浏览器就会执行一次POST请求，把你的评论放进请求报文里，然后拼接好POST请求头，再通过TCP协议发送服务器  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702210407333.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**3：PUT方法**  
PUT方法用来传输文件，比如QQ邮箱发送邮件添加附件，上传百度云盘文件都可以近似理解为PUT方法。为什么说近似呢，**因为实际上PUT方法是不安全的**，一旦对服务器进行写操作都是不安全的，而且在HTTP1.1中PUT方法自身又不带验证机制，因此任何人都可以上传文件，存在极大的安全隐患。所以一般的web网站是不会使用这样的方法的。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702210935793.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**4：HEAD方法**  
HEAD方法只请求报文首部，而不获得报文正文  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702211231380.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702211358898.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**5：DELETE方法**  
DELETE方法用于删除文件，和PUT相反。当然DELETE肯定也是不安全的，所以一般不会开放  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702211509847.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

### B：respond

#### ①：响应报文基本构成

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210630211747284.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
抓包信息如下

```bash
HTTP/1.1 200 OK #响应行
Bdpagetype: 2
Bdqid: 0x8231b40200113aab
Cache-Control: private
Connection: keep-alive # 长连接
Content-Encoding: gzip
Content-Type: text/html;charset=utf-8 # 返回的文件类型
Date: Wed, 30 Jun 2021 13:08:55 GMT
Expires: Wed, 30 Jun 2021 13:08:55 GMT
Server: BWS/1.1
Set-Cookie: BDSVRTM=271; path=/ # 服务器给客户端设置cookie
Set-Cookie: BD_HOME=1; path=/
Set-Cookie: H_PS_PSSID=34099_34225_31253_34226_33848_34133_33607_34134_26350; path=/; domain=.baidu.com
Strict-Transport-Security: max-age=172800
Traceid: 162505853526142891629381477419473517227
X-Ua-Compatible: IE=Edge,chrome=1
Transfer-Encoding: chunked
# 此行为空行，下面是正文
dab
        k k u  V ?@ EͽC4ޏ #Gd  ()! -*   @    NIv I$[  S \N *  D +J  , ǘ3 > /d= Y q t ;   4  } >        ' ?  w

*** FIDDLER: RawDisplay truncated at 128 characters. Right-click to disable truncation. ***
```

可以看到响应报头正文部分均为乱码，其实这是HTTPS请求，已经被加密了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210630214111891.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
这里我们使用Linux中telnet工具尝试抓包  
在命令行中输入`telent www.baidu.com 80`，请求百度的80端口  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210630215425976.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
接着我们手动进行请求，请求方法是GET，请求URL如果输入`/`表示请求首页，版本为1.1  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210630215659582.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
请求正文略过，直接输入换行，此时会发现百度服务器会将百度的首页，也就是一个HTML文件返回过来,这就是响应正文  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210630215847644.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
其中content-length就是标识响应正文的长度的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210630220018423.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

#### ②：HTTP常见状态码

当客户端向服务器端发送请求时，会返回状态，借助它，用户可以知道服务器到底是正常了请求，还是出了问题，如果处理问题，到底是出了怎样的问题  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702212558774.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

HTTP状态码常见的共有五大类，不同数字具有不同的含义  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702212217799.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**2XX-成功**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702212647614.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **204-No Content**：**服务器已经成功处理了请求，但在返回的响应报文中不含实体的主体部分**（另外，也不会允许返回任何实体的主体）。举例，如果你从浏览器发出的请求处理后，若返回204，那么浏览器显示的页面则不会发生更新。**使用场景主要集中在只需要从客户端向服务器发信息，而对客户端不需要发送新信息内容的情况下使用**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702213011478.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **206-Partial Content**：**表示客户端发起了范围请求，同时服务器成功执行了此次请求**。响应报文中的`“Content-Range”`指定范围的实体内容  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021070221325253.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

**3XX-重定向**  
该状态码表示浏览器需要执行**某些特殊的操作**才能正确处理请求  
以302为例，你在没有淘宝网时，点击上面的购物车选项  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702214601496.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
本应该跳转到购物车页面，但是由于没有登录，所以服务器强制让你重定向到登录页面，可以发现次登录页面状态码是302  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702214725935.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **301-Moved Permanently**：**意思是永久重定向，表示请求的资源已经分配了新的URL**，意思可以理解为人家网址已经变了，你的快捷方式（收藏）还指向以前的URL  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702215102833.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **302-Found**：意思是**临时重定向，表示请求的资源已经分配了新的URL**，希望用户可以新的URL访问，但是这一次先放过你，虽然访问错了，但是服务器帮你跳转过来。  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702215326540.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **303-See Other**：**该状态码表示由于请求的圆存在着另一个URL**，应使用GET（这一点和302有所区别）获取资源  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021070222062148.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **304-Not Modified**：**不具有跳转的含义，表示资源未修改，重定向已存在的缓冲文件，也称缓存定定向，用于缓存控制**。  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702220947145.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

**4xx-客户端错误**

- **400-Bad Request**：**表示客户端的请求报文有错误**，是一个很笼统的错误。  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702221303123.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **403-Forbidden**：**意思是服务器拒绝访问**，并不是客户端的请求出错  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702221443892.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **404-Not Found**：**表示请求的资源在服务器上找不到，无法提供客户端**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702221545764.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
  **5xx-服务器错误**

- **500-Internal Server Error**：**和400一样，是一个笼统性的错误，只是表明服务器出现了错误**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702221752711.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- **501-Not Implemented**：**表示客户端请求的功能还不⽀持，类似“即将开业，敬请期待”的意思**

- **502 Bad Gateway**：**通常是服务器作为网关或代理时返回的错误码，表示服务器自身工作正常，访问后端服务器发生了错误**。

- **503-Service Unavailable**：**表示服务器当前很忙，暂时无法响应服务器，类似“网络服务正忙，请稍后再试”的意思。**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021070222210585.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

### C：HTTP常见Header（字段）

可以看到不管是request还是response都有请求行这一部分

**Host字段：客户端告知服务器, 所请求的资源是在哪个主机的哪个端口上**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702222550793.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**Content-Length：服务器在返回数据时，会有 Content-Length 字段，表明本次回应的数据长度。**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702222643670.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**Connection字段：常用于客户端要求服务器使用 TCP 持久连接，以便其他请求复用。**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702222754660.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- HTTP/1.1 版本的默认连接都是持久连接，但为了兼容⽼版本的 HTTP，需要指定 Connection 首部字段的值为Keep-Alive 。

**Content-Type 字段：用于服务器回应时，告诉客户端，本次数据是什么格式**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702222906780.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)

- `Content-Type: text/html; charset=utf-8`表示发送的是网页及其编码格式
- 客户端请求的时候，可以使用 Accept 字段声明自己可以接受哪些数据格式。`Accept: */*`表示可以接受任意格式

**Content-Encoding 字段：段说明数据的压缩⽅法。表示服务器返回的数据使⽤了什么压缩格式**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210702223050859.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F125693301)  
**User-Agent: 声明用户的操作系统和浏览器版本信息，这东西在爬虫中涉及较多**  
**referer: 当前页面是从哪个页面跳转过来的**  
**location: 搭配3xx状态码使用, 告诉客户端接下来要去哪里访问**  
**Cookie: 用于在客户端存储少量信息. 通常用于实现会话\(session\)的功能**