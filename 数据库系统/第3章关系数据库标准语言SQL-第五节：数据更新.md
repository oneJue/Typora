  * [pdf下载：密码7281](https://url18.ctfile.com/f/22722418-803434693-77fa8b)
  * [专栏目录首页：【专栏必读】（考研复试）数据库系统概论第五版（王珊）专栏学习笔记目录导航及课后习题答案详解](https://zhangxing-tech.blog.csdn.net/article/details/122771126)

### 文章目录

  * 一：插入数据（INSERT）
  *     * （1）插入元组
    * （2）插入子查询结果
  * 二：修改数据（UPDATE）
  *     * （1）修改某一个元组的值
    * （2）修改多个元组的值
    * （3）带子查询的修改语句
  * 三：删除数据（DELETE）
  *     * （1）删除某一个元组的值
    * （2）删除多个元组的值
    * （3）带子查询的删除语句

SQL数据更新主要有三种形式

  * 插入数据（INSERT）
  * 修改数据（UPDATE）
  * 删除数据（DELETE）

# 一：插入数据（INSERT）

## （1）插入元组

**语法： 格式如下，用于将新元组插入指定表中。需要注意**

  * `INTO`子句中没有出现的属性列，新元组在这些列上将会取`NULL`
  * 若`INTO`子句中没有指明任何属性列名，则新插入的元祖必须在每个属性列上均有值

![在这里插入图片描述](https://img-
blog.csdnimg.cn/93a441256646481a9039e63212692a3b.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

**演示：**

【例1】将一个新学生元组 **（学号：200215128；姓名：陈冬；性别：男；所在系：IS；年龄：18岁）** 插入到Student表中

    
    
    INSERT
    INTO student(Sno,Sname,Ssex,Sdept,Sage)
    VALUES('201215128','陈冬','男','IS',18);
    

  * 注意 **顺序可以和表不一致**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/686138abb487419ca4ff556a4a503d7d.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

【例2】插入学生张成民

    
    
    INSERT
    INTO student
    VALUES('201215126','张成民','男',18,'CS');
    

  * 注意由于没有指定顺序，所以 **按照必须按照表中属性列的顺序插入，否则会导致插入错误**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/52f1ee2db0cb42c6b8eff6844dfb27f5.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

【例3】插入一条选课记录（201215128,1）

    
    
    INSERT
    INTO sc(Sno,Cno)
    values('201215128','1');
    

  * 在这种情况下，`Grade`属性列会设置为`NULL`
  * 需要注意如果没有给出属性列，则 **必须执行`Grade`为`NULL`**

![在这里插入图片描述](https://img-
blog.csdnimg.cn/75ac235d145845af963acfd5f38baa82.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_13,color_FFFFFF,t_70,g_se,x_16)  
【例4】插入多条记录

    
    
    INSERT 
    INTO student 
    VALUES 
    (201515000,'小赵','男',30,'IS')，
    (201515001,'小钱','女',28,'MA')，
    (201515002,'小孙','男',33,'MJ')，
    (201515003,'小李','女',25,'CS')，
    (201515004,'小周','男',41,'LI');
    

## （2）插入子查询结果

**语法： 格式如下，子查询同样可以嵌套在`INSERT`语句中用于生成待插入的批量数据**

![在这里插入图片描述](https://img-blog.csdnimg.cn/d58b60a350b448f08f6878b455270f84.png)

**演示：**

【例5】对每一个系，求学生的平均年龄，并把结果存入数据库

    
    
    CREATE TABLE dept_age(
    	Sdept char(15),
    	Age SMALLINT
    );
    
    INSERT 
    INTO dept_age(Sdept,Age)
    SELECT Sdept,AVG(Sage)
    FROM student
    GROUP BY Sdept;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/c367eca22fb74c5ebc7201c438f1605a.png)

# 二：修改数据（UPDATE）

**语法： 格式如下，其功能是修改指定表中满足`WHERE`子句条件的元组**

  * 如果省略WHERE子句，则表示要修改表中所有元组

![在这里插入图片描述](https://img-blog.csdnimg.cn/ae9586d56ff742f4b98bbcdef7722ac6.png)

## （1）修改某一个元组的值

**演示：**

【例6】将学生201215121的年龄改为22岁

    
    
    UPDATE student
    set Sage=22
    WHERE Sno='201215121';
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/784a98b35d0b405a923c3e8771bf39f5.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

## （2）修改多个元组的值

**演示：**

【例7】将所有学生的年龄增加1岁

    
    
    UPDATE student
    set Sage=Sage+1;
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/32d9b9e9b33a43cf9a9f25be2911a9a7.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

## （3）带子查询的修改语句

**演示：**

【例8】将CS系所有学生的成绩置0

    
    
    UPDATE sc
    SET Grade=0
    WHERE Sno IN
    (
    	SELECT Sno FROM student WHERE Sdept='CS'
    );
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/e5f56a92e7034c2690b6ce66eb7dd523.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_11,color_FFFFFF,t_70,g_se,x_16)

# 三：删除数据（DELETE）

**语法： 格式如下，其功能是从指定表中删除满足`WHERE`子句条件的所有元组，注意**

  * `DELETE`删除的是表的数据，而不是表的定义
  * 如果省略`WHERE`子句，那么就表示删除全部元组

![在这里插入图片描述](https://img-blog.csdnimg.cn/e45c2afc96444072be578fdaa0d0cb17.png)

## （1）删除某一个元组的值

**演示：**

【例9】删除学号为201215128的学生记录

    
    
    DELETE 
    FROM student
    WHERE Sno='201215128';
    

![在这里插入图片描述](https://img-
blog.csdnimg.cn/36cea86cb87a4411ad15be135122d304.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

## （2）删除多个元组的值

**演示：**

【例10】删除所有的学生选课记录

    
    
    DELETE
    FROM SC；
    

## （3）带子查询的删除语句

**演示：**

【例11】删除计算机科学系所有学生的选课记录

    
    
    DELETE FROM sc
    WHERE Sno IN
    	(SELECT Sno FROM student WHERE Sdept='CS');
    

