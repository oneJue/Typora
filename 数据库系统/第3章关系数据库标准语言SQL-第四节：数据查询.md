  * [pdf下载：密码7281](https://url18.ctfile.com/f/22722418-803434693-77fa8b)
  * [专栏目录首页：【专栏必读】（考研复试）数据库系统概论第五版（王珊）专栏学习笔记目录导航及课后习题答案详解](https://zhangxing-tech.blog.csdn.net/article/details/122771126)

### 文章目录

  * ★★★★★SELECT语句格式★★★★★
  * 一：单表查询（查询时只涉及一张表）
  *     * （1）选择表中的若干列
    *       * A：查询指定列
      * B：查询全部列
      * C：查询经过计算的值
      *         * ①：算数表达式
        * ②：字符串常量及函数
        * ③：使用列别名改变查询结果标题
    * （2）选择表中的若干行（元组）
    *       * A：消除取值重复的行（DISTINCT）
      * B：查询满足条件的元组
      *         * ①：比较大小
        * ②：确定范围
        * ③：确定集合
        * ④：字符匹配
        * ⑤：转义字符
        * ⑥：涉及空值的查询
        * ⑦：多重条件查询
    * （3）ORDER BY子句
    * （4）聚集函数
    * （5）GROUP BY子句
  * 二：连接查询（查询时涉及多张表）
  *     * （1）等值连接和非等值连接
    * （2）自身连接
    * （3）连接JOIN
    *       * A：INNER JOIN(JOIN)
      * B：LEFT JOIN(LEFT OUTER JOIN)
      * C：RIGHT JOIN(RIGHT OUTER JOIN)
      * D：FULL JOIN(FULL OUTER JOIN)
    * （4）复合条件连接
  * 三：嵌套查询
  *     * （1）带有IN谓词的子查询
    * （2）带有比较运算符的子查询
    * √：不相关子查询和相关子查询
    * （3）带有ANY（SOME）或ALL谓词的子查询
    * （4）带有EXISTS谓词的子查询
  * 四：集合查询

  * 关于数据库如何安装，表如何建立这里不再介绍，请移步：[（数据库系统概论|王珊）第三章关系数据库标准语言SQL-第零节：MYSQL环境安装和表的建立以及一些注意事项](https://zhangxing-tech.blog.csdn.net/article/details/122552342)
  * 所用表为（上面文章中也有完整代码）：  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/013a2479007d4c6dadd2ee7f9873a1db.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

# ★★★★★SELECT语句格式★★★★★

数据库最核心的操作便是 **数据查询** ，SQL提供了 **SELECT** 语句实现该功能，其使用非常灵活而且有极其丰富的功能。格式如下

**SELECT语句含义
：根据WHERE子句的条件表达式从FROM子句指定的表、视图中找出满足条件的元组，再按照SELECT子句中的目标列表达式选出元组中的属性值形成结果表。如果有：**

  * **GROUP BY ：结果按<列名1>的值进行分组，该属性列值相等的元组为一个组；通常会在每组中作用聚集函数；如果该子句还携带HAVING短语，则只有满足指定条件的组才予以输出**
  *  **ORDER BY ：结果表还要按<列名2>的值的升序或降序排序**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/fbe8bc8bf0624ef7b2f367769c4a0b4e.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

# 一：单表查询（查询时只涉及一张表）

## （1）选择表中的若干列

### A：查询指定列

**演示 ：**

【例1】查询`Student`表中的学生及年龄

    
    
    SELECT Sname,Sage from Student;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/97f36ebd6b774a0e941675eda3e61c4c.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

### B：查询全部列

**语法 ：**

  * `*`作为通配符表示全部

**演示 ：**

【例2】查询`Student`表中全部列

    
    
    SELECT * from Student;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/8a047d620dc242a1890087f063352b5b.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

### C：查询经过计算的值

**语法 ：SELECT子句的`<目标列表达式>`不仅可以是属性列，还可以是表达式，具体有**

  * 算数表达式
  * 字符串常量
  * 函数
  * 列别名

#### ①：算数表达式

**演示 ：**

【例3】根据年龄计算学生的出生日期

    
    
    SELECT Sname,2022-Sage from Student;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/63a46a4982c44db393ea3e17c3395171.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

#### ②：字符串常量及函数

**演示 ：**

【例4】使用小写字母展示所在系别

    
    
    SELECT Sname,LOWER(Sdept) from student;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/9702c20d9b8d4c54839b4675a9df257f.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_15,color_FFFFFF,t_70,g_se,x_16)

#### ③：使用列别名改变查询结果标题

**演示 ：**

【例5】查询`Student`表中的`Sname`和`Sage`，列标题起别名为“姓名”和年龄

    
    
    SELECT Sname `姓名`,Sage `年龄` from student;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/184d94f83ef54362841775046780b456.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

## （2）选择表中的若干行（元组）

### A：消除取值重复的行（DISTINCT）

**语法 ：前面说过投影操作可能会导致相同的行出现所以其结果必须消除重复行。可以使用`DISTINCT`消除**

**演示 ：**

【例6】：查询`SC`表的`Sno`列，然后消除重复学号

    
    
    SELECT DISTINCT Sno from SC;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/2148b19e0a7a431fa73cdbe94247ebcb.png)

### B：查询满足条件的元组

**语法 ：通过WHERE子句实现，常用的查询条件如下**  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/a3c32c0cd68a43fe8ec27a5be039c2fa.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

#### ①：比较大小

**演示 ：**

【例7】在`SC`表中查询成绩大于85的同学的学号

    
    
    SELECT Sno,Grade from SC
    WHERE Grade > 85;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/3eead6c6654d4626b0d22f090e4acacf.png)

#### ②：确定范围

**演示 ：**

【例8】查血年龄在闭区间[19,20]的学生

    
    
    SELECT Sname,Sage from student
    WHERE Sage BETWEEN 19 AND 20;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/45503a4bedbd4ddbb97e7aca270b759a.png)  
【例9】查血年龄不在闭区间[19,20]的学生

    
    
    SELECT Sname,Sage from student
    WHERE Sage NOT BETWEEN 19 AND 20;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/32ad6d4b21bd4f06b033f599a380a49a.png)

#### ③：确定集合

**演示 ：**

【例10】查询数字1是否在集合(1,2,3)中

  * 由于满足，所以会返回1

    
    
    SELECT 1 IN (1,2,3);
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/cf715022bff645e699c7bb12a049fd69.png)

【例11】查询数学系（MA）和计算机科学系（CS）学生的姓名

    
    
    SELECT Sname,Sdept from student
    WHERE Sdept IN('MA','CS');
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/1f622c9454024cb48c85e481c714a5a4.png)

【例12】查询非数学系（MA）和非计算机科学系（CS）学生的姓名

    
    
    SELECT Sname,Sdept from student
    WHERE Sdept NOT IN('MA','CS');
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/57ec64a6fb684477b023c45340f23938.png)

#### ④：字符匹配

注意：

  * 可以使用`=`代替`LIKE`，使用`!=`代替`NOT LIKE`
  * `%`可以代替多个字符
  * `_`只能代替一个字符

**演示 ：**

【例13】查询所有男生

    
    
    SELECT Sname from student
    WHERE Ssex LIKE '男';
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/0cd5f14c53c042cb9d3193435ca57ff9.png)  
【例14】查询所有姓刘的学生（%代替多个）

    
    
    SELECT Sname FROM student 
    WHERE Sname like '刘%';
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/e04ecb63224a47fa9034c2fe9c9931e7.png)

#### ⑤：转义字符

注意：

  * `ESCAPE '＼'` 表示将“ ＼” 翻译为转义字符

**演示 ：**

【例15】假设`Course`表中有一门课叫做`DB_Design`

  * 如果不作处理，这里的`_`代替某个字符，产生歧义

    
    
    SELECT Cno,Ccredit
    FROM Course
    WHERE Cname LIKE 'DB\_Design' ESCAPE '\';
    

#### ⑥：涉及空值的查询

**演示 ：**

【例16】查询缺少成绩的学生的学号和相应的课程号

  * 某些学生选修课程后没有参加考试，所以有选课记录，但没有考试成绩

    
    
    SELECT Sno Cno FROM SC
    WHERE Grade IS NULL;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/981d9804728442579a6fbd3e46f6b1fa.png)

#### ⑦：多重条件查询

注意：

  * `AND`和 `OR`来联结多个查询条件
  * `AND`的优先级高于`OR`
  * 可以用括号改变优先级

**演示 ：**

【例17】 查询计算机系年龄在20岁以下的学生姓名

    
    
    SELECT * FROM student
    WHERE Sdept='CS' AND Sage < 20;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/f332b7529e314f6a93d2e3015ce2683c.png)

## （3）ORDER BY子句

**语法 ：ORDER BY子句对查询结果按照一个或多个属性列进行排序**

  * ASC-升序（默认）
  * DESC-降序

**演示 ：**

【例18】查询选修了3号课程的学生的学号及其成绩，查询结果按分数降序排列

    
    
    SELECT Sno,Grade
    from SC
    WHERE Cno='3'
    ORDER BY Grade DESC;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/6f205ee3ee4f4876b23f542503ca4dda.png)

【例19】查询全体学生情况，查询结果按所在系的系号升序排列，同一系中的学生按年龄降序排列

    
    
    SELECT *
    FROM student
    ORDER BY Sdept,Sage DESC;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/25e2906b9f704a8ba3903c63a68b1879.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_19,color_FFFFFF,t_70,g_se,x_16)

## （4）聚集函数

**语法 ：主要有以下几种**  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/03b2cab78c434dafb90f6accf809a5a9.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

**演示 ：**

【例20】查询学生的总人数

    
    
    SELECT COUNT(Sno)
    FROM student;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/8feb23d51e9c4648ab0b438eff301357.png)  
【例21】查询选修了课程的学生人数

    
    
    SELECT COUNT(DISTINCT Sno) Num
    FROM SC;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/02c753f55db546a3989122866953a31d.png)  
【例22】 计算2号课程的学生平均成绩

    
    
    SELECT AVG(Grade)
    FROM SC 
    WHERE Cno = '2';
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/c9193d752bce4b9e9b891e857aa6287d.png)  
【例23】查询选修2号课程的学生最高分数

    
    
    SELECT Sno,MAX(Grade)
    FROM SC 
    WHERE Cno='2';
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/f9041848be6147a497c7b4815910b46c.png)

## （5）GROUP BY子句

**语法 ：GROUP BY子句将查询结果按某一列或多列的值分组，值相等的分为一组**

  * 分组目的是为了 **细化聚集函数的作用对象： 若未分组，聚集函数将会作用于整个查询结果；若分组，聚集函数将会作用于每一个组，也即每一个组都有一个函数值**
  * 需要注意： **WHERE子句作用于整个表或视图，从中选择出满足条件的元组；HAVING短语作用于组，从中选择满足条件的组**

相信读完之后大家可能还是有点迷糊，举个例子。比如我要查询“各个课程对应的选课人数”，如果没有`GROUP BY`子句

    
    
    SELECT Cno,Count(Sno)
    FROM sc;
    

由于它会作用于整个查询结果，所以直接统计出了记录的条数  
![在这里插入图片描述](https://img-blog.csdnimg.cn/78bcf98f315e430bb7a648ec927373da.png)  
如果加入`GROUP BY`子句，按照课程号分组， **那么`GROUP BY`会按照`Cno`进行分组，相同的为一组，然后在每组内统计`Sno`**

    
    
    SELECT Cno,Count(Sno)
    FROM sc
    GROUP BY Cno;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/1357320be958415fa367e476fa1c6a31.png)

而如果我只想显示那些选课人数大于1以上的课程号呢， **那么就可以使用`HAVING`短语，在组内进行筛选**

    
    
    SELECT Cno,Count(Sno)
    FROM sc
    GROUP BY Cno
    HAVING Count(Sno) > 1;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/ca032ff259434c12878badbc2b7bd8af.png)  
**演示 ：**

【例24】查询平均成绩大于等于80分的学生学号和平均成绩

    
    
    SELECT Sno,AVG(Grade)
    FROM SC 
    GROUP BY Sno
    HAVING AVG(Grade) >= 80;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/dd98750c9a1140caa99779d1e276fc55.png)

# 二：连接查询（查询时涉及多张表）

## （1）等值连接和非等值连接

**语法 ：在WHERE子句中写入连接条件（又叫做连接每谓词），其格式为**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/04445338e11c4257b9e38b00ea959b88.png)  
**其中比较运算符有：`=`、`>`、`<`、`>=`、`<=`、`!=`**

  * **当运算符为`=`时称之为等值连接**
  *  **当运算符不为`=`时称之为非等值连接**

**演示 ：**

【例25】查询每个学生及其选修课程的情况

    
    
    SELECT student.*,sc.*
    FROM student,sc
    WHERE student.Sno=sc.Sno;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/9098dae7e8664c35a00b3efc7bef1a6e.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
【例26】使用自然连接（ **特殊的等值连接** ）完成【例25】

    
    
    SELECT student.Sno,Sname,Ssex,Sage,Sdept,Cno,Grade
    FROM student,sc
    WHERE student.sno=sc.sno;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/6010830aed8d49c8b1f4255b54f223bc.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
【例27】查询选修2号课程且成绩在80分以上的所有学生的学号和姓名

    
    
    SELECT Student.Sno,Sname
    FROM student,sc
    WHERE student.Sno=sc.Sno AND //连接条件
    	Cno='2' AND Grade > 80; //其他限定条件
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/0a277f31557246178600e1e4decb66be.png)

## （2）自身连接

**语法 ：所谓自身连接就是指一个表与自己连接**

**演示 ：**

【例28】查询每一门课的先修课的先修课

  * 在`Course`表中有的只是每门课的直接先修课，要想得到先修课的先修课，那么就必须先找到一门课的先修课，然后再按此先修课的课程号查找它的先修课

因此， **为`Course`表取两个别名，分别为`ONE`和`TWO`**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/86891c0d38cd493bba9bfb6d3da0d7e4.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

    
    
    SELECT ONE.Cno,TWO.Cpno
    FROM Course ONE,Course TWO
    WHERE ONE.Cpno=TWO.Cno;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/4838953460f94e42b4b74ad1d66f2bdf.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_14,color_FFFFFF,t_70,g_se,x_16)

当然，还可以继续找 **先修课的先修课的先修课**

    
    
    SELECT ONE.Cno,THREE.Cpno
    FROM Course ONE,Course TWO,course THREE
    WHERE ONE.Cpno=TWO.Cno AND TWO.Cpno=THREE.Cno;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/0f77917831a44722be332a3a9122ecae.png)

## （3）连接JOIN

**语法 ：SQL JOIN用于把来自两个或多个表的行结合起来，其格式如下**

    
    
    SELECT column_name(s)
    FROM TABLE1//左表
    <某某 JOIN>TABLE2//右表
    ON TABLE1.column_name=TABLE2.column_name
    

**有如下几类**

  * `INNER JOIN`(`JOIN`)
  * `LEFT JOIN`(`LEFT OUTER JOIN`)
  * `RIGHT JOIN`(`RIGHT OUTER JOIN`)
  * `FULL JOIN`(`FULL OUTER JOIN`)

### A：INNER JOIN(JOIN)

**`INNER JOIN`(`JOIN`)：关键字在表中存在至少一个匹配时返回行**  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/8235d2a68c914bf09cd38e5a0e335c48.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_19,color_FFFFFF,t_70,g_se,x_16)  
**演示 ：**

【例29】以`sc`和`course`的`Cno`作为比对标准，将相同连接在一起

    
    
    SELECT Sno,sc.Cno,Grade,course.Cno,Cname,Cpno,Ccredit
    FROM sc INNER JOIN course ON(sc.Cno=course.Cno);
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/c4702afa17964fe89116980641e9253d.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

### B：LEFT JOIN(LEFT OUTER JOIN)

**`LEFT JOIN`(`LEFT OUTER JOIN`)：以左表为标准，若右表中无匹配，则填NULL**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/c45245514e184c8d83c6ceb1b84f0a4d.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_15,color_FFFFFF,t_70,g_se,x_16)

    
    
    SELECT Sno,sc.Cno,Grade,course.Cno,Cname,Cpno,Ccredit
    FROM sc LEFT JOIN course ON(sc.Cno=course.Cno);
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/c4702afa17964fe89116980641e9253d.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

### C：RIGHT JOIN(RIGHT OUTER JOIN)

**`RIGHT JOIN`(`RIGHT OUTER JOIN`)：以右表为标准，若左表中无匹配，则填NULL**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/7357f9afd26a453288fc308993a0cc88.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_18,color_FFFFFF,t_70,g_se,x_16)

    
    
    SELECT Sno,sc.Cno,Grade,course.Cno,Cname,Cpno,Ccredit
    FROM sc RIGHT JOIN course ON(sc.Cno=course.Cno);
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/17c4bdf789b242eab9878687a482f149.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

### D：FULL JOIN(FULL OUTER JOIN)

**`FULL JOIN`(`FULL OUTER JOIN`)：本质就是结合了LEFT JOIN和RIGHT JOIN**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/66d741f7877348c49fa7986278c3af6a.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

    
    
    SELECT Sno,sc.Cno,Grade,course.Cno,Cname,Cpno,Ccredit
    FROM sc FULL JOIN course ON(sc.Cno=course.Cno);
    

## （4）复合条件连接

**语法 ：没有什么新的东西，就是涉及多张表，多个条件的查询**

**演示 ：**

【例30】查询每个学生的学号、姓名、选修的课程名及成绩

    
    
    SELECT student.Sno,Sname,Cname,Grade
    FROM student,course,sc
    WHERE student.Sno=sc.Sno AND sc.Cno =course.Cno;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/f02845b4019f4d74a96741556cdc9297.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_16,color_FFFFFF,t_70,g_se,x_16)

# 三：嵌套查询

在SQL中，一个`SELECT-FROM-WHERE`语句称为一个 **查询块** ，
**将一个查询块嵌套在另一个查询块的WHERE子句或HAVING短语的条件中的查询称之为 嵌套查询。比如**

在下面的这个例子中，
**内层循环查出来的是符合`Cno=2`的`Sno`集合，外层循环则在该集合内查询是否有满足的`Sno`，有的话显示`Sname`即可**

    
    
    SELECT Sname //外层查询
    FROM Student
    WHERE Sno IN
    	(
    		SELECT Sno //内层查询
    		FROM SC
    		WHERE Cno='2'
    	)
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/986cc55e311f42a497c2a7ab7d0ebdf3.png)

**需要注意以下几点**

  * **子查询的SELECT语句不能使用`ORDER BY`子句**
  *  **嵌套查询往往可以转换为对应的连接运算**

## （1）带有IN谓词的子查询

**语法 ：嵌套查询中，子查询的结果往往是一个集合，所以IN在嵌套查询中使用次数最为频繁**

**演示 ：**

【例31】查询与“刘晨”在同一个系学习的学生

  * 考虑时可以由内向外， **先查询出刘晨所在的系，然后在该集合中查询满足该集合的学生姓名**

    
    
    SELECT student.Sno,Sname,Sdept FROM student WHERE Sdept IN
    (SELECT Sdept FROM student WHERE Sname='刘晨');
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/7c9126e4322e4892b5952796903bc769.png)

当然嵌套查询有时也可以转化为 **连接** 完成

    
    
    SELECT S1.Sno,S1.Sname,S1.Sdept
    FROM Student S1,Student S2
    WHERE S1.Sdept=S2.Sdept AND S2.Sname='刘晨';
    

【例32】查询选修了课程名为“信息系统”的学生学号和姓名

  * 首先在`Course`表中找出“信息系统”的`Cno`，形成`Cno`的集合
  * 然后在`SC`表中找出哪些记录的`Cno`在`Cno`集合内，形成`Sno`集合
  * 最后在`Student`表中找出哪些记录的`Sno`在`Sno`集合内，形成最终的结果

    
    
    SELECT Sno,Sname FROM Student WHERE Sno IN 
    (SELECT Sno FROM SC WHERE Cno IN
    	(SELECT Cno FROM Course WHERE Cname='信息系统')
    );
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/fff0fca87f6f4601986f1753edb837b0.png)  
当然本例也可以使用 **连接** 完成

    
    
    SELECT student.Sno,Sname
    FROM student,course,sc
    WHERE Student.Sno=sc.Sno AND sc.Cno=course.Cno AND Cname='信息系统';
    

## （2）带有比较运算符的子查询

**语法
：带有比较运算符的子查询是指父查询与子查询之间用比较运算符进行连接。当用户能确切知道内层查询返回的是单个值时，可以使用`>`、`<`、`=`、`>=`、`<=`、`!=`等比较运算符**

**演示 ：**

【例33】查询与“刘晨”在同一个系学习的学生

  * 和【例31】一样，只不过【例31】使用`IN`完成的

    
    
    SELECT Sno,Sname,Sdept FROM student WHERE Sdept
    =
    (SELECT Sdept FROM student WHERE Sname='刘晨');
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/e4492d7dcb3d45fb8bd869d722449e47.png)

【例34】找出每个学生超过他选修课程平均成绩的课程号

  * 首先内层循环要计算该学生的平均成绩
  * 外层循环使用`>=`
  * 两个循环要用`Sno`关联在一起（相关子查询）

    
    
    SELECT Sno,Cno FROM sc x WHERE Grade
    >=
    (SELECT AVG(Grade) from sc WHERE x.Sno=Sno);
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/14ce95e514964ea69faba61b1a21d454.png)

## √：不相关子查询和相关子查询

**不相关子查询 ：子查询的查询条件不依赖于父查询**

  * 像【例31】、【例32】这样的都是不相关子查询

**查询时由里向外、逐层处理。每个子查询在上一级查询处理之前求解，子查询的结果用于建立父查询的查找条件**

**相关子查询 ：子查询的查询条件依赖于父查询**

  * 像【例34】这样的都是相关子查询

**查询时首先会取外层查询中表的第一个元组，根据它与内层查询相关的属性值处理内层查询，若WHERE子句返回为真，则将此元组放入结果表，然后再取外层表的下一个元组，接着重复上述过程**

**比如对于【例34】，其处理过程可能是下面这样的**

①：从外层查询中取出`sc`的一个元祖`x`，将`x.Sno`的值(201215121)传递给内层查询

    
    
    SELECT AVG(Grade)
    FROM sc y
    WHERE y.Sno='201215121';
    

②：执行内层查询，得到值88（近似），用该值代替内层查询，得到外层查询

    
    
    SELECT Sno,Cno
    FROM SC X
    WHERE Grade >= 88
    

③：执行这个查询，得到

    
    
    (201215121,1)
    (201215121,3)
    

然后再取下一个元组重复上述过程即可

## （3）带有ANY（SOME）或ALL谓词的子查询

**语法 ：内层查询返回单个值时使用比较运算符。如果返回多个值要用`ANY`（有的是SOME）或`ALL`，然后同时使用比较运算符**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/c5550c56e34f4b0bac6f2ce4899ad1c5.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)  
**另外`ANY`或`ALL`与聚集函数、`IN`等谓词有等价关系。也就是说在一些情况下，他们的写法效果作用是一样的**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/cfde6efde6f44212a68f0857e6b70069.png)

  * 例如`<ALL`意思是小于所有值，那么它的等价含义就是小于最小值，也即`<MIN`

**演示 ：**

【例35】查询其他系比计算机科学系 **任意** 一个学生年龄小的学生姓名和年龄

    
    
    SELECT Sname,Sage FROM student WHERE Sage < ANY
    	(SELECT Sage FROM student WHERE Sdept='CS')
    AND Sdept!='CS';
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/8a2c8a7288704cbb9dd55d77979ddc9d.png)  
由于`<ANY`等价于`<MAX`，所以其等价写法就是

    
    
    SELECT Sname,Sage FROM student WHERE Sage <
    	(SELECT MAX(Sage) FROM student WHERE Sdept='CS')
    AND Sdept!='CS';
    

## （4）带有EXISTS谓词的子查询

**语法 ：EXISTS代表“存在量词 ∃ {\exists}
∃”。带有EXISTS谓词的子查询不返回任何数据，只返回`true`或`false`。另外，由EXISTS引出的子查询，其目标列表达式通常都是`*`，因为给出列名没有实际意义**

  * 若内层查询结果非空，则外层WHERE子句返回`true`
  * 若内层查询结果为空，则外层WHERE子句返回`false`

**与EXISTS相对的便是NOT EXISTS**

  * 若内层查询结果为空，则外层WHERE子句返回`true`
  * 若内层查询结果非空，则外层WHERE子句返回`false`

**需要注意的是，一些带有EXISTS和NOT EXISTS谓词的子查询不能被其他形式的子查询等价替换；但是
所有带IN谓词，比较运算符，ANY和ALL谓词的子查询都可以用带EXISTS谓词的子查询替换**

**演示 ：**

【例36】查询所有选修了1号课程的学生姓名

  * 处理时，首先会取外层查询中`Student`表的第一个元组，根据它与内层查询相关的属性值（`Sno`）处理内层查询，若`WHERE`子句返回为`true`则取外层查询中该元组的`Sname`放入结果表

    
    
    SELECT Sname FROM student WHERE 
    EXISTS
    (SELECT * from sc where Sno=student.Sno AND Cno='1');
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/e769e622fb2045429f81f8e8e699cb94.png)

【例37】查询与“刘晨”在同一个系学习的学生

  * 和【例31】一样，这里用EXISTS代替IN

    
    
    SELECT Sno,Sname Sdept FROM student s1 WHERE
    EXISTS
    	(select * FROM student WHERE Sdept=s1.Sdept AND Sname='刘晨');
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/69356106ed3546168328734d81bd6e54.png)

【例38】查询选修了全部课程的学生姓名

  * 它等价于： **查询这样一个学生，没有一门课是它不选的**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/3447702db2a04524a65f4647cf4ac640.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

# 四：集合查询

**语法 ：SELECT语句的查询结果是元组的集合，所以多个SELECT语句的结果可进行集合操作。主要有**：

  * 并操作：`UNION`
  * 交操作：`INTERSECT`
  * 差操作：`EXCEPT`

**需要注意的是查询结果的列数必须相同且对应项的数据类型也必须相同**

**演示 ：**

【例39】查询计算机科学系的学生及年龄不大于19岁的学生

    
    
    SELECT Sname,Sage,Sdept FROM student WHERE Sdept='CS' 
    UNION
    SELECT Sname,Sage,Sdept FROM student WHERE Sage<=19;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/4d79d4890395426eb9548e7a0ed23055.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

【例40】查询计算机科学系的学生与年龄不大于19岁的学生的交集

    
    
    SELECT * FROM student WHERE Sdept='CS'
    INTERSECT
    SELECT * FROM student WHERE Sage <=19;
    

