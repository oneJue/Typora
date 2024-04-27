 

### 文章目录

- [一：文件相关概念](#_2)
- - [（1）什么是文件](#1_3)
  - [（2）文件名](#2_11)
  - [（3）文件类型](#3_16)
- [二：文件指针](#_43)
- [三：文件的打开和关闭](#_55)
- [四：文件的顺序读写](#_87)
- - [（1）写](#1_111)
  - [（2）读](#2_201)
- [五：文件的随机读取](#_286)
- - [（1）fseek](#1fseek_287)
  - [（2）ftell](#2ftell_312)
  - [（3）rewind](#3rewind_334)
- [六：文件结束条件判定](#_358)
- [七：其它注意点](#_372)

# 一：文件相关概念

## （1）什么是文件

我们在磁盘上所见到的都是文件，在程序设计中，所谈到文件主要有两种：

- 一种是**程序文件**：包括源程序文件（后缀为.c）,目标文件（windows环境后缀为.obj）,可执行程序（windows环境后缀为.exe）；
- 另一种是**数据文件**：文件的内容不一定是程序，也可以是程序运行时读写的数据，比如程序运行需要从中读取数据的文件，或者输出内容的文件

本节主要讨论的是数据文件

## （2）文件名

一个文件有“**文件路径+文件主干+文件后缀**”三部分组成，为了方便起见，文件表示也被称为文件名  
![](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2e57a3512fc944c188091ed1d1018404.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

## （3）文件类型

数据文件可以被分为**文本文件**和**二进制文件**

- 举个例子，程序里有个变量是10000，如果要把它放到磁盘里，你想要让用户读懂那么你肯定要用字符表示，也就是1对应的ASCII码，2对应的ASCII码，这样去转换，那么这种就是文本。如果不经过任何转换，就是二进制文件（使用记事本方式打开exe文件时的乱码）

如下程序代码表示将数字10000用存储为二进制文件

```c
int main()
{
            
            
	int a = 10000;
	FILE* pf = fopen("test.txt", "wb");
	fwrite(&a, 4, 1, pf)；//二进制写
	fclose(pf);
	pf = NULL;
	return 0;
}
```

虽然显示的是txt文件，但是其本质是二进制文件，所以用记事本打开是乱码  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fef46b5776e7c43b6bf38e724eeae6f81.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)  
要去看到到底是什么，可以用VS，使用二进制编辑器打开  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F848c8dafb8c546169b657b027d943be4.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9fc24685bd2f44589fbc92798e2d9153.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

# 二：文件指针

**每一个使用的文件，在内存中都会开辟一个文件信息区，这个文件信息区存放的是文件的名字，文件的状态等等，所有的这些信息存放在一个结构体变量中，将其取名为`FILE`**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5dc0539bac6c44b5937424873ade8c29.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)  
既然它是结构体变量，那么我们就可以创建一个指针去操作它，这个指针也就被称为文件指针

```c
FILE* pf;//文件指针
```

**pf是一个指向FILE类型的数据变量，可以使pf指向某个文件的文件信息区（是一个结构体变量）。通过该文件信息区中的信息就可以访问该文件**

# 三：文件的打开和关闭

文件读写时先打开文件，再关闭文件  
在打开文件的时候，会返回一个`File*`的指针，C语言规定使用`fopen`打开文件，`fclose`关闭文件

```c
FILE* fopen(const char* filename,const char* mode);
int fclose(FILE* stream)
```

`mode`选项有如下几种

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe0df58fe1bf44e52a8d9ae10120cca3c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

```c
int main()
{
            
            
FILE* pf = fopen("test.txt", "r");
if (pf == NULL) {
            
            
	printf("%s\n", strerror(errno));
	return 0;
}
fclose(pf);
pf == NULL;
return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0d830907bade496280627cdfe589ef09.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

# 四：文件的顺序读写

读写函数如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc07ec7991bb94842b9567878159dfeb6.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

注意**标准输入流和标准输入流**

```c
int main()
{
            
            
	char arr[1024] = {
            
             0 };
	fgets(arr, 1024, stdin);//键盘就是标准输入流
	fputs(arr, stdout);//屏幕是标准输出流
}
```

注意这几组函数

- `scanf`和`printf`是针对标准输入/输出流的格式化输入/输出语句
- `fscanf`和`fprintf`是针对所有输入/输出流的格式化输入/输出语句
- `sscanf`是从字符串中读取格式化的数据
- `sprintf`是把格式化数据输出到字符串

## （1）写

**1：字符输出**

```c
int main()
{
            
            
	FILE* pfwrite = fopen("test.txt", "w");
	if (pfwrite == NULL) {
            
            
		printf("%s\n", strerror(errno);
		return	0;
	}
	fputc('L', pfwrite);
	fputc('o', pfwrite);
	fputc('v', pfwrite);
	fputc('E', pfwrite);
	fclose(pfwrite);
	pfwrite = NULL;
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6e7a8145d75f4e089efe34dddde5f4b1.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

**2：文本行输出**，其中换行需要手动加入

```c
int main()
{
            
            
	FILE* pfwrite = fopen("test.txt", "w");
	if (pfwrite == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	fputs("I\n", pfwrite);
	fputs("LOVE\n", pfwrite);
	fputs("YOU", pfwrite);
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F364e25d7a0494254bb74c904ddedf094.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

**3：格式化输出**

```c
struct S
{
            
            
	int n;
	float score;
	char arr[10];
};

int main()
{
            
            
	struct S s = {
            
             100,3.14f,"love" };
	FILE* pfwrite = fopen("test.txt", "w");
	if (pfwrite == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	fprintf(pfwrite, "%d-%f-%s", s.n, s.score, s.arr);//格式化输出
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F053a0778760f46e79a33e4e4bfd0bf52.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

**4：二进制输出**

```c
struct S
{
            
            
	char arr[10];
	int age;
};

int main()
{
            
            
	struct S s = {
            
             "Bob",19};
	FILE* pfwrite = fopen("test.txt", "wb");//二进制
	if (pfwrite == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	fwrite(&s, sizeof(struct S), 1, pfwrite);
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4ea8704cc9304915a6b1d89009b30f89.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

## （2）读

**1：字符读入**

```c
int main()
{
            
            
	FILE* pfread = fopen("test.txt", "r");
	if (pfread == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	printf("%c", fgetc(pfread));
	printf("%c", fgetc(pfread));
	printf("%c", fgetc(pfread));
	printf("%c", fgetc(pfread));
	printf("%c", fgetc(pfread));
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffdea0d9245ab41cdbc0b287c4c8e503c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

**2：文本行输入**，可以获得换行符

```c
int main()
{
            
            
	FILE* pfread = fopen("test.txt", "r");
	if (pfread == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	char a[20];
	fgets(a, 6, pfread);//读一行6个字符
	printf("%s\n", a);
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F88c253da98fc45b28a19c1034fd29c94.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

**3：格式化输入**

```c
struct S
{
            
            
	int n;
	float score;
	char arr[10];
};

int main()
{
            
            
	struct S s = {
            
             0 };
	FILE* pfread = fopen("test.txt", "r");
	if (pfread == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	fscanf(pfread, "%d%f%s", &(s.n), &(s.score), &(s.arr));
	printf("%d-%f-%s", s.n, s.score, s.arr);
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdec2f1b851724128ac259f497e6f46e9.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

**4：二进制输入**

```c

int main()
{
            
            
	struct S s = {
            
             0 };
	FILE* pfread = fopen("test.txt", "rb");//二进制
	if (pfread == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	fread(&s, sizeof(struct S), 1, pfread);
	printf("%d-%f-%s", s.n, s.score, s.arr);
	return 0;
}
```

# 五：文件的随机读取

## （1）fseek

文件指针开始会指向文件内容的开头，顺序读取时想要读取某个特定字符时，必须先读其前面的字符，而使用随机读取，可以直接偏移文件指针，直接读取  
**需要注意，在读取完成后文件指针会自动向后偏移**

 -    `SEEL_CUR`：以文件指针的当前位置偏移
 -    `SEEK_END`：以文件的末尾位置偏移
 -    `SEEK_SET`：以文件的起始位置开始

```c
int main()
{
            
            

	FILE* pfread = fopen("test.txt", "r");
	if (pfread == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	fseek(pfread, 2, SEEK_CUR);//以当前位置偏移整数
	printf("%c\n", fgetc(pfread));
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa827d4ebc8ae46199fa8bedde013c4ab.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

## （2）ftell

返回当前文件指针相对于起始位置的偏移量

```c
int main()
{
            
            

	FILE* pfread = fopen("test.txt", "r");
	if (pfread == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	fgetc(pfread);//读取一个跳过一个
	int pos = ftell(pfread);
	printf("%d\n", pos);
	
	fclose(fread);
	return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fadf7ac7ce16d4b2f81ea0345b5a98522.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

## （3）rewind

让文件指针回到起始位置

```c
int main()
{
            
            

	FILE* pfread = fopen("test.txt", "r");
	if (pfread == NULL) {
            
            
		printf("%s\n", strerror(errno));
		return	0;
	}
	fgetc(pfread);//读取一个跳过一个
	int pos1 = ftell(pfread);
	rewind(pfread);//回到其实位置
	int pos2 = ftell(pfread);
	printf("%d\n", pos2);//结果仍然是0
	
	fclose(fread);
	return 0;
}
```

# 六：文件结束条件判定

对于`fgetc`，如果其返回值为`EOF`；对于`fgets`，如果其返回值为`NULL`，就代表文件读到末尾  
**需要特别注意的一点是千万不要用feof判断文件结束**

下面是读到文件末尾的标准写法

```c
while ((c = fgetc(fp)) != EOF)
{
            
            
	//操作
}
```

# 七：其它注意点

1：flcose函数在文件成功关闭时返回0，关闭失败时返回EOF  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdc759132a6a74f7fbb0b68dd42dfd3c8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121042441)

2：当文件到达末尾时，feof返回的是非0，没有到达则返回0

3：使用getchar\(\)输入字符时，是先将所有字符送入缓冲区，直到键入回车换行符才从缓冲区逐个读出并赋值给变量

4：fgets从fp所指的文件中读取字符串并在字符串末尾添加’\\0’，然后存入s，最多读n-1个字符

5：fgets在出错和到达文件末尾返回都是NULL，所以应该用ferror来确定它返NULL的原因是什么

- 函数ferror用来检测是否出现文件错误，如果出现，函数返回非0值，没有错误，返回0值

6：fputs出现写入错误，返回EOF，成功返回一个非负数

7：与gets\(\)不同的是，fgets\(\)从指定的流读字符串，读到换行符时将换行符也作为字符串的一部分读到字符串。同理，与puts\(\)不同的是，fputs\(\)不会在写入文件的字符串末尾自动加上换行符

8：函数fseek调用成功，返回0，否则返回非0

9：fopen发生错误时，返回0