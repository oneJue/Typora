  * [pdf下载：密码7281](https://url18.ctfile.com/f/22722418-803434693-77fa8b)
  * [专栏目录首页：【专栏必读】（考研复试）数据库系统概论第五版（王珊）专栏学习笔记目录导航及课后习题答案详解](https://blog.csdn.net/qq_39183034/article/details/122771126)

### 文章目录

  * 一：注意事项
  * 二：MYSQL环境
  *     * （1）下载
    * （2）安装
    * （3）MYSQL可视化工具Navicat 安装
  * 三：连接数据库
  * 五：建立练习所需基本表

# 一：注意事项

  * 本章是整个数据库中最重要的一章，学了那么多理论，如果连最基本的SQL查询语句都写的磕磕绊绊，那数据库等于没学
  * 初次接触数据库的小伙伴可能会感觉前两章的概念太过晦涩难懂，但是没有关系，学完这一章之后，你会发现前面的概念似乎逐渐清晰了
  *  **切记！一定要安装MYSQL，然后随本章内容一起练习。你看懂了其实根本没有懂， 不要骗自己，欠下的账终归是要还的！**
  * 本节所有准备工作在我的机器上已经全部测试正确了，如果你能按照以下步骤操作，基本没有问题。当然我也不能保证不出错，如果出现错误， **请大家自己查阅百度或者C站，一定要自己动手，不要一遇到挫折就撂挑子不干了**

# 二：MYSQL环境

## （1）下载

  * MYSQL有很多版本，为了适合大多数人，这里选择Windows版本，下载地址为[点击跳转](https://dev.mysql.com/downloads/)
  * 网速较慢，我已经下载好了，[百度网盘链接](https://pan.baidu.com/s/1lAGy8zVsb32XDi8ZMIoIEw)
  * 提取码：1eeg

## （2）安装

点击下载后的文件，运行  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/60cf4a1c946548408907176d8909892c.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
选择 I accept 然后点击next进入下一步  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/3daa9d6f4cc1474fa16b135cc6333192.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
这里选择Developer Default，然后点击next进入下一步  
在选择安装路径时默认C盘 不建议 请更换其他磁盘路径  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/bb9a266de5f846628a1417b6c4e2ad01.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
这一步是检查安装条件，直接点击next进入下一步就可以了

![在这里插入图片描述](https://img-
blog.csdnimg.cn/c9d3882e78234a5fadb328e286a44562.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

这里直接点击execute执行就可以了，执行完后点击next进入下一步。

![在这里插入图片描述](https://img-
blog.csdnimg.cn/2cc4b915b5d643ccab8e7a2da4911b18.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/87bece3433314aba9fd27b26a56e9248.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-
blog.csdnimg.cn/140ae903a0d84ed2b7a97d8863597937.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
到这里设置mysql数据库root密码 然后再点击next：

![在这里插入图片描述](https://img-
blog.csdnimg.cn/8b2e535d21784efbb33cffc14e1feeed.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-
blog.csdnimg.cn/b18b7ef649374d1caed6ea476d7978d9.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/9ec72867b3c044219219b0d097a581ef.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/2cd4a8a744884bdab4defdb390c751c2.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/6ea31630082549ca985bd427b1ceed75.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/9edbda840d6740b1b40d67b253af47e3.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/eb164072263346e0bad8e7d006eadab7.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
一路点击next，并check你的root密码，MySQL就成功在你的电脑上安装完成了  
安装完成后进入MySQL的安装目录，进入MySQL Sever，其目录下的文件如下：

![在这里插入图片描述](https://img-
blog.csdnimg.cn/1a08bb507aac417b8214531ee65d2f03.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
安装完成 使用mysql可视化工具查询和操作

## （3）MYSQL可视化工具Navicat 安装

  * [百度网盘链接][https://pan.baidu.com/s/1Em07R2MLHnpKheFOCANW-Q]
  * 提取码：w8n7

下载后是这样的

![在这里插入图片描述](https://img-blog.csdnimg.cn/a59ae604e9ec41f39523d0aff1e73dd0.png)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/004fd1df197e4a23a0ec5595209ab889.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/3aee45280b644ff2937900aa0674a6a5.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/26409b6bc4404de1a79555bfb1dbee2b.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/c05232a1e5e7403f8b980f307d963ab6.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/cf77cc304fb941e88699e742b7ec286b.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/6f37666ecfd74759b140a78e47327fd3.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/d359f1e31a954ce98cbbf5d808640f62.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
接着安装补丁  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/52be73aaf379453ba8064fec124c4fe2.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
然后选择刚刚安装的Navicat安装路径下找到navicat.exe文件，点击选择即可激活 成功  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/57b02133aea04da9a13bec2275c88fab.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/70afeda250b74981972dbb2d25b2addf.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_11,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/92d78d75cc1946c496a30efbb402b958.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

# 三：连接数据库

  * 注意如果连接不成功，可在CSDN中查询错误提示信息，一定有解决方案

* * *

打开 navicat ，点击 连接 ，选择 数据库  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/4e8e66ed0eec46cea234374965fc253c.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

弹出以下界面 （以MySQL为例），熟悉各部分的作用  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/8c6aa17effb649f0bbaca88224a95e95.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
测试是否可以连接，有以下提示，点击确定开始使用数据库  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/84a00252ca4147d5b74441fe0a42cc10.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
双击 或 右键 打开连接，图标变亮表示已经打开连接  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/516ae4c2033d423889bb4e9ede2402d8.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_10,color_FFFFFF,t_70,g_se,x_16)

# 五：建立练习所需基本表

和课本一样，本章所涉及的关系如下

  * **关系Student** ：学生基本信息
  *  **关系Course** ：课程基本信息
  *  **关系SC** ：成绩基本信息

![在这里插入图片描述](https://img-
blog.csdnimg.cn/24828c89134e4b80bcdfd63ee59dc580.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
请按如下步骤执行即可，先不要管为什么，你只需要把表建立好即可

* * *

点击查询，再点击新建查询  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/486c9d64eee94d0f8cb37b89248db8c3.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
输入以下SQL语句

    
    
    /*例3.5建立一个学生表*/
    /*1、删除practice_db数据库（如果存在）*/
    drop database if exists practice_db;
    /*2、创建数据库practice_db数据库*/
    create database practice_db charset utf8;
    use practice_db; -- 选择jt_db数据库
    /*3. 创建学生表Student(例3.5)*/
    Create table Student(
    Sno char(9) Primary key,/*列级完整性约束条件,Sno是主码*/
    Sname char(20) unique,/*Sname取唯一值*/
    Ssex char(2),
    Sage smallint,
    Sdept char(20)
    );
    /*4.插入学生信息*/
    insert into Student values('201215121','李勇','男',20,'CS');
    insert into Student values('201215122','刘晨','女',19,'CS');
    insert into Student values('201215123','王敏','女',18,'MA');
    insert into Student values('201215125','张立','男',19,'IS');
    /*6.创建课程表Course(例3.6)*/
    create table Course(
    Cno char(4) primary key,
    Cname char(40) Not NULL,
    Cpno char(4),/*Cpno的含义是先行课*/
    Ccredit smallint,
    foreign key(Cpno) references Course(Cno)  /*Cpno是外码,被参照表示Course,被参照列是Cno*/
    );
    /*6.插入课程信息*/
    /*由于Course表以自身为外键约束,所以要先禁用外键约束插入数据,插入完成后再开启外键约束*/
    SET FOREIGN_KEY_CHECKS=0; /*禁用外键约束*/
    insert into Course values('1','数据库','5',4);
    insert into Course values('2','数学','null',2);
    insert into Course values('3','信息系统','1',4);
    insert into Course values('4','操作系统','6',3);
    insert into Course values('5','数据结构','7',4);
    insert into Course values('6','数据处理','null',2);
    insert into Course values('7','PASCAL语言','6',4);
    SET FOREIGN_KEY_CHECKS=1; /*插入完成后开启外键约束*/
    /*7.建立学生选课表SC(例3.7)*/
    create table SC(
    Sno char(9),
    Cno char(4),
    Grade smallint,
    primary key (Sno,Cno), /*主码由两个属性构成,必须作为表级完整性进行定义*/
    foreign key(Sno) references Student(Sno), /*表级完整性约束条件,Sno是外码,被参照表是Student*/
    foreign key(Cno) references Course(Cno)/*表级完整性约束条件,Cno是外码,被参照表是Course*/
    );
    /*8.插入学生选课信息*/
    insert into SC values('201215121','1',92);
    insert into SC values('201215121','2',85);
    insert into SC values('201215121','3',88);
    insert into SC values('201215122','2',90);
    insert into SC values('201215122','3',80);
    
    

右键点击刷新，所创建的数据库`practice_db`便出现了  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/f61e44a2cf504f99809c10b968239b69.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
双击`practice_db`，并选择“表”展开，我们所创建的三张表便出现了

  * 注意，如果后续再写一些SQL语句运行完之后发现对应位置没有更改，继续右键刷新即可

![在这里插入图片描述](https://img-
blog.csdnimg.cn/4f1154404d804b8e8c7bd874fe00e0a7.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

* * *

至此，所有准备已经完成

