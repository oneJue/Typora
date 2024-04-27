  * [pdf下载：密码7281](https://url18.ctfile.com/f/22722418-803434693-77fa8b)
  * [专栏目录首页：【专栏必读】（考研复试）数据库系统概论第五版（王珊）专栏学习笔记目录导航及课后习题答案详解](https://zhangxing-tech.blog.csdn.net/article/details/122771126)

### 文章目录

  * 一：SQL的产生与发展
  * 二：SQL特点
  *     * （1）综合统一
    * （2）高度非过程化
    * （3）面向集合的操作方式
    * （4）以同一种语法结构提供多种使用方式
    * （5） 语言简洁，易学易用
  * 三：SQL的基本概念
  * 四：基本数据类型
  *     * （1）数值类型
    * （2）日期和时间类型
    * （3）字符串类型

**结构化查询语言( Structured Query Language, SQL)**
是关系数据库的标准语言，也是一个通用的、功能极强的关系数据库语言。其功能不仅仅是查询，而是包括数据库模式创建、数据库数据的插入与修改、数据库安全性完整性定义与控制等一系列功能

# 一：SQL的产生与发展

此部分没什么考点，但可以做一定了解

  * [这一篇文章大家有时间可以看看，真可谓天妒英才（点击跳转）](https://www.sohu.com/a/448543330_468731)

![请添加图片描述](https://img-
blog.csdnimg.cn/edae591253224db4848111d9b2bf6a67.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
不过需要注意以下几点

  * 目前，没有任何一个数据库系统能够支持SQL标准的所有概念和特性
  * 许多软件厂商对SQL基本命令集还进行了不同程度的扩充和修改
  *  **我们介绍的是SQL的基本概念和基本功能，并不是针对某个厂商，具体实现起来可能有所差异，所以还需要大家查阅相关手册**
  * 为了演示，我们使用的是MYSQL，具体安装细节，请见[（数据库系统概论|王珊）第三章关系数据库标准语言SQL-第零节：MYSQL环境安装和表的建立以及一些注意事项](https://zhangxing-tech.blog.csdn.net/article/details/122552342)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/a8ee835d98b74c58a08bfc29b17a0312.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_13,color_FFFFFF,t_70,g_se,x_16)

# 二：SQL特点

SQL集 **数据查询(dataquery)** 、 **数据操纵(datamanipulation)** 、 **数据定义(data
definition)** 和 **数据控制(data control)** 功能于一体， 其主要特点包括以下几部分

## （1）综合统一

SQL集数据定义语言、数据操纵语言、数据控制语言的功能于一体，语言风格统一，可以独立完成数据库生命周期中的全部活动，包括以下一系列操作要求

  * 定义和修改、删除关系模式，定义和删除视图，插入数据，建立数据库
  * 对数据库中的数据进行查询和更新
  * 数据库重构和维护
  * 数据库安全性、完整性控制，以及事务控制
  * 嵌入式SQL和动态SQL定义

## （2）高度非过程化

用SQL进行数据操作时， **只要提出“做什么”，而无须指明“怎么做”** ，因此无须了解存取路径。存取路径的选择以及SQL的操作过程由系统自动完成

## （3）面向集合的操作方式

而 **SQL采用集合操作方式** ，不仅操作对象、查找结果可以是元组的集合，而且一次插入、删除、更新操作的对象也可以是元组的集合

## （4）以同一种语法结构提供多种使用方式

**SQL可作为独立语言**
：SQL既是独立的语言，又是嵌入式语言。作为独立的语言，它能够独立地用于联机交互的使用方式，用户可以在终端键盘上直接键入SQL命令对数据库进行操作

**SQL可作为嵌入式语言** ：SQL语句可以嵌入到高级语言（例如C++、Java等）程序中，供程序员设计程序时使用

而且在这两种不同的使用方式下， **其语法结构仍然基本是一致的**

## （5） 语言简洁，易学易用

SQL功能极强，但由于设计巧妙，语言十分简洁，完成核心功能只用了9个动词（下表）。SQL接近英语口语，因此易于学习和使用  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/8110ab91aedb4b57887ca6466a650e47.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

# 三：SQL的基本概念

支持SQL的关系数据库管理系统（例如MYSQL）当然支持关系数据库三级模式结构

  * **外模式** ：包括若干视图（view）和部分基本表（base table）
  *  **内模式** ：包括若干存储文件（stored file）

![在这里插入图片描述](https://img-
blog.csdnimg.cn/c226ac1f65104a5d890c1f255dc325fa.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

注意基本表和视图

  * **基本表** ：基本表就是本身独立存在的表，在关系数据库管理系统中 **一个关系就对应了一个基本表** ，一个或多个基本表对应一个存储文件。一个表可以带若干索引，索引可以存放在存储文件中
  *  **视图** ：从一个或几个基本表中导出的表，它本身不独立存储在数据库中，也即数据库中只存放视图的定义而不存放视图对应的数据，视图是一个 **虚表**

# 四：基本数据类型

学习任何一门高级语言，必定会首先学习它的数据类型，例如`int`、`char`等。SQL也是如此，其常用数据类型如下

  * 注意：不需要刻意记忆，常用的也就那么几个，用着用着就熟悉了，这里展示的目的只是做查询手册用
  *  **常用数据类型已用黑体标出**

## （1）数值类型

![在这里插入图片描述](https://img-
blog.csdnimg.cn/a56856fc5a40453e963ace696d0e5f3c.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

## （2）日期和时间类型

![在这里插入图片描述](https://img-
blog.csdnimg.cn/8d7472c4c8bf4211834c9423621294c8.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

## （3）字符串类型

![在这里插入图片描述

