  * [pdf下载：密码7281](https://url18.ctfile.com/f/22722418-803434693-77fa8b)
  * [专栏目录首页：【专栏必读】（考研复试）数据库系统概论第五版（王珊）专栏学习笔记目录导航及课后习题答案详解](https://zhangxing-tech.blog.csdn.net/article/details/122771126)

### 文章目录

  * 零：有关说明
  *     * （1）安装数据库与建表
    * （2）一些语法说明
  * 一：模式的定义和删除（SCHEMA）
  *     * （1）定义模式
    * （2）删除模式
  * 二：基本表的定义、删除和修改（TABLE）
  *     * （1）定义基本表
    * （2）修改基本表
    * （3）删除基本表
  * 三：索引的建立与删除（INDEX）
  *     * （1）建立索引
    * （2）修改索引
    * （3）删除索引

# 零：有关说明

## （1）安装数据库与建表

  * 关于数据库如何安装，表如何建立这里不再介绍，请移步：[（数据库系统概论|王珊）第三章关系数据库标准语言SQL-第零节：MYSQL环境安装和表的建立以及一些注意事项](https://zhangxing-tech.blog.csdn.net/article/details/122552342)
  * 所用表为（上面文章中也有完整代码）：  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/013a2479007d4c6dadd2ee7f9873a1db.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

## （2）一些语法说明

由于我们使用的MYSQL所以有些语法和可能和SQL标准有所差异，甚至无法实现，总结如下

**①：关于语法中的括号**

  * **< >**：尖括号用于分隔字符串
  *  **[]** ：方括号表示规则中的可选元素，可以选择也可以省略
  *  **{}** ：花括号表示聚集规则中的元素，必须明确指定

**②：其它**

  * 在SQLserver中我们可以用 `CREATE DATABASE`来创建数据库，而用`CREATE SCHEMA`来创建架构。但是在MYSQL中两者是同等作用的，可以认为没有区别。所以下面介绍时没有实际的演示例子
  * 在MYSQL中字符串用的不是单引号，而是``（键盘上数字1左边的那个）
  * 在MYSQL中创建模式时是没有后面的`AUTHROIZATION <用户名>`的
  *  **MYSQL中大小写均可行**

* * *

回归正题，SQL的数据定义主要包括以下内容

  * 模式定义
  * 表定义
  * 视图定义
  * 索引定义

![在这里插入图片描述](https://img-
blog.csdnimg.cn/37ec2e55519140af99da914dd08ed67f.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

  * 注意 **修改视图和修改模式时只能删除重建**

* * *

# 一：模式的定义和删除（SCHEMA）

**注意 ：在SQLserver中我们可以用 CREATE DATABASE来创建数据库，而用CREATE
SCHEMA来创建架构。但是在MYSQL中两者是同等作用的，可以认为没有区别。所以下面介绍时没有实际的演示例子**

## （1）定义模式

**语法 ：`CREATE SCHEMA <模式名> AUTHORIZATION <用户名>`（注意上面语法说明）**

  * **如果没有指定 <模式名>， 那么<模式名>隐含为<用户名>**
  * 定义模式本质是定义了一个 **命名空间**
  * 创建模式的同时也可以创建其它东西：`CREATE SCHEMA <模式名> AUTHORIZATION <用户名> [<表定义子句> | <视图定义子句> | <授权定义子句>]`

## （2）删除模式

**语法 ：`DROP SCHEMA <模式名><CASCADE|RESTRICT>`（注意上面语法说明）**

  * `CASCADE`：表示 **级联** ，也即删除模式时会删除该模式中所有数据库对象
  * `RESTRICT`：表示 **限制** ，也即在删除时如果该模式下定义了其它对象，则拒绝

# 二：基本表的定义、删除和修改（TABLE）

## （1）定义基本表

**语法** ：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2cf408a165bc49b1b42ff3c92cd5fd8e.png)

  * 建表的同时通常还可以定义与该表有关的完整性约束条件（参照前几节内容）
  *  **如果完整性约束条件涉及该表的多个属性列，则必须定义在表级上，否则既可以在列级上也可以在表级上**

**演示** ：  
【例1】：建立学生表`Student`，其中`Sno`是主码，且`Sname`不能重复

    
    
    CREATE TABLE Student
    (
    	Sno CHAR(9) PRIMARY KEY, //主码
    	Sname CHAR(20) UNIQUE,	//唯一值
    	Ssex	CHAR(2),
    	Sage SMALLINT,
    	Sdept CHAR(20)
    );
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/b32cfb0dca444def88d9e753bda7a013.png)  
【例2】：建立课程表`Course`，其中`Cno`是主码，`Cname`不能取空值，`Cpno`代表先修课（意思是学习该课前必须先学习某个课程），它是外码，需要参照本表，参照列为`Cno`

    
    
    CREATE TABLE Course
    (
    	Cno CHAR(4) PRIMARY KEY,//主码
    	Cname CHAR(20) NOT NULL,//非空
    	Cpno CHAR(4),
    	Credit SMALLINT,
    	FOREIGN KEY (Cpno) REFERENCES Course(Cno)//表级完整性，Cpno是外码，被参照表示Course，被参照列是Cno
    );
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/f0b44a11abbe400992248ae90fcb6376.png)  
【例3】：建立学生选课表`SC`，其中`Sno`和`Cno`是外码，分别参照`Student`表的`Sno`列和`Course`表的`Cno`列

  * `Sno`和`Cno`是`SC`的主码，必须使用表级完整性定义

    
    
    CREATE TABLE SC
    (
    	Sno CHAR(9),
    	Cno CHAR(4),
    	Grade SMALLINT,
    
    	PRIMARY KEY(Sno,Cno),//必须使用表级完整性定义
    	FOREIGN KEY(Sno) REFERENCES Student(Sno),
    	FOREIGN KEY(Cno) REFERENCES Course(Cno)
    )
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/28ad92c161b4436b988d5103a493acc4.png)

## （2）修改基本表

**语法** ：  
![在这里插入图片描述](https://img-
blog.csdnimg.cn/ee787b17b1e44ecdb4361f8f76c1b841.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

  * **`ADD`：用于增加新列，新的列级完整性约束条件和新的表级完整性约束条件**
  *  **`DROP COLUMN`：用于删除表中的列**
  *  **`DROP CONSTRAINT`：用于删除指定的完整性约束条件**
  *  **`ALTER COLUMN`：用于修改原有的列定义**

**演示** ：

【例4】：向`Student`表中增加“入学时间”列，数据类型为“日期型”

    
    
    ALTER TABLE Student ADD Sentrance DATE;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/5f16c97cb475453db31b2e6dba69cbe3.png)  
【例5】：将`Student`表“年龄”这一列的数据类型由改为 `INT`

    
    
    ALTER TABLE Student ALTER COLUMN Sage CHAR;
    

【例6】：`Course`表中的`Cname`必须取唯一值

    
    
    ALTER TABLE Course ADD UNIQUE(Cname);
    

【例7】：删除`Student`表中的“入学时间”列

    
    
    ALTER TABLE Student DROP COLUMN Sentrance CASCADE;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/5e62f9eeb4fe4fc5a6ae2efd61ff5818.png)  
【例8】：删除`Course`表中`Cname`的唯一约束

  * 此部分看完这个就明白了：[（数据库系统概论|王珊）第五章数据库完整性-第四、六、七节：约束命名子句、断言和触发器](https://zhangxing-tech.blog.csdn.net/article/details/122654008)

    
    
    ALTER TABLE Course DROP INDEX Cname;
    

MYSQL在删除约束时有一些不一样，请点击查看：[链接](https://www.begtut.com/sql/sql-ref-drop-
constraint.html)

## （3）删除基本表

**语法** ：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/fae55e92650e4bfa91e8ccc3a99de49c.png)

  * **选择`RESTRICT`**：欲删除的基本表不能被其他表的约束所引用（比如CHECK、FOREIGN KEY等）、不能有视图、不能有触发器（trigger），不能有存储过程或函数等
  *  **选择`CASCADE`**：没有限制条件，所有相关依赖对象连同基本表一起删除

**演示** ：

【例9】：删除`Student`表，使用`RESTRICT`

  * 由于`SC`表参照的是`Student`，所以删除不成功

    
    
    DROP TABLE student RESTRICT;
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/42c859bfd6644183bc1cd6b717e06130.png)

# 三：索引的建立与删除（INDEX）

当表的数据量较大时，查询操作就会十分耗时。 **建立索引是加快查询速度的有效手段**

  * 数据库索引类似于图书后面的索引，能快速定位需要查询的内容

![在这里插入图片描述](https://img-
blog.csdnimg.cn/268748a6f88243678de9e8b66817b609.png?x-oss-
process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5b-r5LmQ5rGf5rmW,size_20,color_FFFFFF,t_70,g_se,x_16)

**用户可以根据应用环境的需要在基本表上建立一个或多个索引，类型有**

  * 顺序文件上的索引
  * B+树索引
  * 散列索引
  * 位图索引

**索引虽然能加快查询速度，但也有缺点**

  * 需要占用一定的存储空间
  * 会提高查询速度但是会降低更新速度

## （1）建立索引

**语法** ：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/dbd9b112a0054167bb3c84d78de6ce81.png)

  * **`<表名>`**：要建立索引的基本表的名字
  *  **索引可以建立在该表的一列或多列上，各列之间使用逗号分隔**
  *  **每个 <列名>后面还可以用<次序>指定索引值的排列次序，可选ASC-升序（默认）或DESC-降序**
  *  **UNIQUE** ：表明此索引的每一个索引值只对应唯一的数据记录
  *  **CLUSTER** ：表示需要建立聚簇索引（第七章会讲到）

**演示** ：

【例10】：请按以下要求建立索引

  * `Student`表按学号升序建立唯一索引
  * `Course`表按课程号升序建立唯一索引
  * `SC`表按学号升序和课程号降序建立唯一索引

    
    
    CREATE UNIQUE INDEX Stusno ON Student(Sno);
    CREATE UNIQUE INDEX Coucno ON Course(Cno);
    CREATE UNIQUE INDEX SCno ON SC(Sno ASC,Cno DESC);
    

## （2）修改索引

**语法** ：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/e5148e5c937d414a9848b0209ec29e71.png)  
**演示** ：

【例11】：将`SC`表的`SCno`索引改名为`SCSno`

    
    
    ALTER INDEX SCno RENAME TO SCSno
    

## （3）删除索引

**语法** ：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f760184ed48498886cf36ddfd40aaf1.png)

**演示** ：  
【例12】：删除`SC`表的SCSno`索引`

    
    
    DROP INDEX SCSno
    
    //如果失败，也可这样写
    DROP INDEX SCSno on SC;
    DROP INDEX SC.SCSno;
    
    

