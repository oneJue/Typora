 

- [pdf下载：密码7281](https://url18.ctfile.com/f/22722418-803434693-77fa8b)
- [专栏目录首页：【专栏必读】（考研复试）数据库系统概论第五版（王珊）专栏学习笔记目录导航及课后习题答案详解](https://zhangxing-tech.blog.csdn.net/article/details/122771126)

### 文章目录

- [名词解释](#_8)
- [简答题](#_13)
- [应用题](#_55)

# 名词解释

- **视图**：视图是一个虚表，其本质就是一条`SELECT`语句，而查询结果被赋予了一个名字，也即视图名字。或者说视图本身不包含任何数据，它只包含映射到基表的一个查询语句，当基表数据发生变化时，视图数据也随之变化。其目的就是在于方便，简化数据操作

# 简答题

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F86ea6bdd07c74fc8aefeba5de69424a4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

【答案】  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3780002a6726464194fa82eef8f197f0.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F01e88d558a4448d8b9e600982a36c870.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

【答案】

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F92e297e2a065463593eb3fa680f7b523.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5c4740d74e3b4d1ca0c7debb7d336b5a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

【答案】

基本表是本身独立存在的表，在sQL中一个关系就对应一个表。视图是从一个或几个基本表导出的表。视图本身不独立存储在数据库中，是一个虚表。即数据库中只存放视图的定义而不存放视图对应的数据，这些数据仍存放在导出视图的基本表中。视图在概念上与基本表等同，用户可以如同基本表那样使用视图，可以在视图上再定义视图

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F57033fd6932242dca520a8003f3e3482.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

【答案】

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2bf346cdae654e8cbead1d784dffc0da.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3355a36f5d814c7f840735968324640f.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

【答案】  
基本表的行列子集视图一般是可更新的。若视图的属性来自集合函数、表达式，则该视图肯定是不可以更新的

# 应用题

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ff3e1852ee972411e953798796c66da15.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

【答案】

```sql
SELECT * FROM S WHERE A='10';

SELECT A,B FROM S;

SELECT A,B,S.C,S.D,E,F FROM S,T WHERE S.C=T.C AND S.D=T.D;

SELECT * FROM S,T WHERE S.C=T.C;

SELECT * FROM S,T WHERE A<E;

SELECT S.C,S.D,T.* FROM S,T;

```

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7c7fafd4b5424e4f81b3fc9ab68bcaee.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)  
> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb7a45f1dee634f8a9b4eb4fbb9189001.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

- 题目链接：[（数据库系统概论|王珊）第二章关系数据库：习题](https://blog.csdn.net/qq_39183034/article/details/122824387?spm=1001.2014.3001.5501)

【答案】

```sql
SELECT SNO FROM SPJ WHERE JNO='J1';

SELECT SNO FROM SPJ WHERE JNO='J1' AND PNO='P1';

SELECT SPJ.SNO FROM SPJ,P WHERE SPJ.PNO=P.PNO AND JNO='J1' AND COLOR='红';

SELECT JNO FROM SPJ WHERE JNO NOT IN
(SELECT JNO FROM SPJ,S,P WHERE S.CITY='天津' AND COLOR='红' AND SPJ.SNO=S.SNO AND SPJ.PNO=P.PNO)

①：先查询S1供应的零件号
SELECT PNO FROM SPJ WHERE SNO='S1' ，其结果为（P1,P2）
②：查询哪一个工程即使用P1又使用P2
SELECT JON FROM SPJ WHERE PNO='P1' AND JNO IN (SELECT JNO FROM SPJ WHERE PNO='P2');

```

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F52d1c5a368e8474c8b21bd9950a3bcaa.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

```sql
SELECT SNAME,CTIY FROM S;

SELECT PNAME,COLOR,WEIGHT FROM P;

SELECT JNO FROM SPJ WHERE SNO='S1';

SELECT PNAME,QTY FROM P,SPJ WHERE SPJ.PNO=P.PNO AND JNO='J2';

SELECT PNO FROM SPJ,S WHERE SPJ.SNO=S.SNO AND S.CITY='上海';

SELECT JNAME FROM SPJ,S,J WHERE SPJ.SNO=S.SNO AND SPJ.JNO=J.JNO AND S.CITY='上海';

SELECT JNO FROM SPJ WHERE JNO NOT IN(
	SELECT JNO FROM SPJ,S WHERE SPJ.SNO=S.SNO AND S.CITY='天津'
);

UPDATE P SET COLOR='蓝' WHERE COLOR='红';

UPDATE SPJ SET SNO='S3' WHERE SNO='S5' AND JNO='J4' AND PNO='P6';

DELETE FROM S WHERE SNO='S2' DELETE FROM SPJ WHERE SNO='S2';

INSERT INTO SPJ VALUES('S2','J6','P4',200);

```

> ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb9f1a10f228342ffb7511befda86dae8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_d3F5LXplbmhlaQ%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F122863317)

【答案】

```sql
CREATE VIEW VSP AS SELECT SNO,SPJ.PNO,QTY FROM SPJ,J WHERE PPJ.JNO=J.JNO AND J.JNAME='三建'

SELECT PNO,QTY FROM VSP

SELECT * FROM VSP WHERE SNO='S1';
```