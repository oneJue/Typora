  * [pdf下载：密码7281](https://url18.ctfile.com/f/22722418-803434693-77fa8b)

  * [专栏目录首页：【专栏必读】（考研复试）数据库系统概论第五版（王珊）专栏学习笔记目录导航及课后习题答案详解](https://zhangxing-tech.blog.csdn.net/article/details/122771126)

### 文章目录

  * 一：关于视图
  *     * （1）什么视图
    * （2）什么时候会用到视图
    * （3）视图的作用
  * 二：视图的定义和删除
  *     * （1）建立视图
    *       * A：简单创建
      * B：基于多个基表的视图
      * C：基于视图的视图
      * D：带表达式的视图
      * E：分组视图
    * （2）删除视图
  * 三：视图的查询
  * 四：视图的更新
  *     * （1）UPDATE
    * （2）INSERT
    * （3）DELETE

# 一：关于视图

## （1）什么视图

**视图：
视图是一个虚表，其本质就是一条`SELECT`语句，而查询结果被赋予了一个名字，也即视图名字。或者说视图本身不包含任何数据，它只包含映射到基表的一个查询语句，当基表数据发生变化时，视图数据也随之变化。其目的就是在于方便，简化数据操作**

## （2）什么时候会用到视图

简单点说， **当我们感觉查询语句太过复杂且不好操控时** 就可以创建视图，直接

    
    
    SELECT * FROM [view]
    

## （3）视图的作用

  * 视图能够简化用户的操作
  * 视图使用户能以多种角度看待同一数据
  * 视图对重构数据库提供了一定程度的逻辑独立性
  * 视图能够对机密数据提供安全保护
  * 适当的利用视图可以更清晰的表达查询

# 二：视图的定义和删除

## （1）建立视图

**语法： 使用`CREATE VIEW`语句创建视图，格式如下**![在这里插入图片描述](https://img-
blog.csdnimg.cn/35453393eb694f159939b678b6dd963f.png)

  * 子查询可以是 **任意的`SELECT`语句**（是可以含有`ORDER BY`子句和`DISTINCT`短语取决于具体系统）
  * 组成视图的属性列名 **要么全部省略要么全部指定，不能有第三种情况**
  * 如果省略视图列名，则其列名默认由 **`SELECT`子句目标列诸字段组成**

### A：简单创建

**演示 ：**

【例1】建立信息系IS学生的视图

    
    
    CREATE VIEW IS_student
    AS 
    SELECT Sno,Sname,Sage,Sdept
    FROM student
    WHERE Sdept='IS'
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/b3c0ad2963b04eae9f511234e57f38b6.png)

### B：基于多个基表的视图

**演示 ：**

【例2】建立计算机科学系选修了1号课程的学生视图

    
    
    CREATE VIEW CS1
    AS
    SELECT student.Sno Sno,Sname,Grade
    FROM student,sc
    WHERE Sdept='CS' AND student.Sno=sc.Sno AND sc.Cno='1';
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/2b4fd84d45dc4830b8244ef38e55606f.png)

### C：基于视图的视图

**演示 ：**

【例3】 建立计算机科学系选修了1号课程且成绩大于95分的学生的视图

    
    
    CREATE VIEW CS2
    AS
    SELECT *
    FROM CS1
    WHERE Grade > 95;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/987022f96ec54b508b640c99056ae3a9.png)

### D：带表达式的视图

**演示 ：**

【例4】定义一个反映学生出生年份的视图

    
    
    CREATE VIEW birthday(Sno,Sname,Syear)
    AS
    SELECT Sno,Sname,2022-Sage
    FROM student;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/d396993daa3b4b8997421c246352d1d3.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

### E：分组视图

  * 带有聚集函数和`GROUP BY`

**演示 ：**

【例5】将学生的学号及他的平均成绩定义为一个视图

    
    
    CREATE VIEW stu_grade_avg(Sno,Savg)
    AS
    SELECT Sno,AVG(Grade)
    FROM sc
    GROUP BY Sno;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/b18180ddcfd14839a12086785a728e52.png)

## （2）删除视图

**语法： 格式如下，需要注意**

  * 基本表删除之后，由该基本表导出的所有视图均无法使用， **但是视图的定义没有从字典中清除**

![在这里插入图片描述](https://img-blog.csdnimg.cn/4faab94addaf4178965b8ea0622e37b7.png)

# 三：视图的查询

**语法： 从用户角度出发，查询视图和查询基本表相同；从DBMS角度出发，采用视图消解法，具体来讲**

  * 首先进行 **有效性检查**
  * 接着转换成 **等价的** 对基本表的查询
  * 最后执行 **修正** 后的查询

这里具体就不再做过多演示了，一般来说转换是能成功进行的，更多细节大家可以了解课本（这部分内容不多）

# 四：视图的更新

**语法：
视图是虚表，所以对视图的更新最终会转化为对基本表的更新。为了防止用户通过视图对数据进行更新时，有意或无意地对不属于视图范围内的基本表数据进行操作，可以在定义视图时加上`WITH
CHECK OPTION`子句。这样在更新时，如果不满足条件，DBMS会拒绝操作**

## （1）UPDATE

**演示 ：**

【例6】将信息系学生视图`IS_Student`中学号201215125的学生姓名改为“刘辰”

    
    
    UPDATE is_student
    SET Sname='刘辰'
    WHERE Sno='201215125';
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/e38deb10ba1c4b4eaff5c19d0401fb04.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

【例7】如果在定义视图`is_student`在定义时加入了`WITH CHECK OPTION`子句，接着再执行【例6】需求

那么在更新时如果将`Sdept`字段改为了’MA’或其他值，DBMS就会拒绝执行，并有下面的错误反馈

    
    
    UPDATE is_student
    SET Sdept='MA'
    WHERE Sno='201215125';
    

> ![在这里插入图片描述](https://img-
> blog.csdnimg.cn/217941776d7c4d4eb1aa34e8427dd7d4.png)

如果在更新时只修改名字，那么就没有问题

    
    
    UPDATE is_student
    SET Sname='德玛'
    WHERE Sno='201215125';
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/f99e7f391cb841b7b1a584c8d3848c42.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

## （2）INSERT

**演示 ：**

【例8】向信息系学生视图`IS_Student`中插入一个新的学生记录：201215129，赵新，20岁

    
    
    INSERT INTO is_student
    VALUES('201215129','赵新',20);
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/3df7025dcf304f8d85098a071791c41e.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

  * 这里视图没有数据，且20插入到了错误的地方（如果没有`WITH CHECK OPTION`就会导致这些错误出现）
  * 如果假如了`WITH CHECK OPTION`，那么DBMS会拒绝执行

## （3）DELETE

  * 删除数据时，有无`WITH CHECK OPTION`都是一样的

**演示 ：**

【例9】删除学号为201215125的学生

    
    
    DELETE 
    FROM is_student
    WHERE Sno='201215125';
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/fa3ab49a20e340e093020481c3f3cd35.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

