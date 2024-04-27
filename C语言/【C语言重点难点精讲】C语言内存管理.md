 

### 文章目录

- [一：相关动态内存函数](#_4)
- - [（1）malloc和free](#1mallocfree_5)
  - [（2）calloc](#2calloc_40)
  - [（3）realloc](#3realloc_54)
- [二：进程地址空间](#_91)
- [三：常见内存错误](#_129)

C语言内存管理其实是一个很糟糕的话题，很烦这个，但是没有办法，考试就爱考这些东西

# 一：相关动态内存函数

## （1）malloc和free

![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F719a671e5feb41e7a248e6638828cc37.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

```c
int main()
{
            
            
	//malloc返回值为void*，所以需要进行强转
	int* p = (int*)malloc(sizeof(int) * 10);
	//malloc有可能申请失败
	if (p == NULL) {
            
            
		printf("%s\n", strerror(errno));
	}
	else {
            
            
		for (int i = 0; i < 10; i++) {
            
            
			p[i] = i;
		}
		for (int j = 0; j < 10; j++) {
            
            
			printf("%d ", *(p + j));
		}
	}
	return 0;

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7f2b1e55f2fb46e7949477e795e70e30.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

**free用于释放mallco开辟的空间，malloc和free要成对使用，开辟的空间如果不使用了，一定要将其free掉，如果不free掉，堆区空间会越挤越大的\(除非程序直接结束\)**。同时当那个指针所指空间free掉之后，由于指针其实还是已经保存了那片空间的地址，这个行为是相当危险的，因为找到指针就有可能会改变它，所以为了彻底断绝他们的联系，在free掉之后，要将指针置为NULL

```c
free(p);
p = NULL;
```

## （2）calloc

calloc函数和malloc函数的作用相同，都是用来动态开辟的。**calloc与malloc所不同的是地方是两者参数格式不一样，并且calloc在开辟的同时会将此空间初始化为0， 而malloc则为随机值**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F96ebb1c7e15a4a83b3bc0aa7af7eb5bf.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

- malloc情况

calloc语法格式为

```c
int* p=(int*) calloc(10,sizeof(int));
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6763277e52fd42c184ab453caf511e7c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

## （3）realloc

在申请好空间后，如果发现申请的空间不合适，过大或者过小，就可以使用realloc来进行调整![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F6a7756d7ff084b28b3a6e4852d0e2301.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)  
比如下面的例子中申请了20个字节后发现空间不够，然后重新调整

```c
int main()
{
            
            
int* p = (int*)malloc(20);//20字节数据
if (p == NULL) {
            
            
	printf("%s\n", strerror(errno));
}
else {
            
            
	for (int i = 0; i < 5; i++) {
            
            
		p[i] = i;
	}
}

int*p1 = realloc(p, 40);//将空间调整为40字节，并返回调整后的地址
for (int j = 0; j < 10; j++) {
            
            
	printf("%d ", *(p + j));
}
free(p1);
p = NULL;
return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe64a6f2a07c541fcb37c809d490c176d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

需要注意：**realloc申请空间有两种方式**

在堆区重新调整空间必然会遇到两种情况，第一种原有的空间后面有足够大的空间，那么申请时相当于就在原有空间后面补上缺的部分，**这样其返回的仍然是原来空间的地址**；第二种原有的空间后面不够大，realloc会重新找一片能完整存放的区域，**然后把之前的内容照搬赋值过来，并释放先前空间**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb6a7944af739498d8508e19876256df2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

# 二：进程地址空间

C/C++程序的地址空间排布如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8e2bbe78ea6342b1bca6ae253cecf40e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)  
用一个例子说明如下

```c
#include <stdio.h>
#include <stdlib.h>

int gobal_val=100;//全局变量已经初始化
int gobal_unval;//全局变量未初始化
int main(int argc,char* argv[],char* env[])
{
            
            
	printf("main函数处于代码段，地址为：%p,十进制为：%d\n",main,main);
	printf("\n");
	printf("全局变量gobal_val，地址为：%p,十进制为:%d\n",&gobal_val,&gobal_val);
	printf("\n");
	printf("全局变量未初始化gobal_unval，地址为：%p,十进制为:%d\n",&gobal_unval,&gobal_unval);
	printf("\n");
	
	char* mem=(char*)malloc(10);
	
	printf("mem开辟的堆空间，mem是堆的起始地址，是%p,十进制为：%d\n",mem,mem);
	printf("\n");	
	printf("mem是指针变量，指针变量在栈上开采，其地址为%p,十进制为：%d\n",&mem,&mem);
	printf("\n");	
	printf("命令行参数起始地址：%p,十进制为：%d\n",argv[0],argv[0]);
	printf("\n");
	printf("命令行参数结束地址：%p,十进制为：%d\n",argv[argc-1],argv[argc-1]);
	printf("\n");
	printf("第一个环境变量的地址：%p,十进制为：%d\n",env[0],env[0]);
	printf("\n");
}

```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fdaee8ede791c4c2395e936b3d719a7c2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

# 三：常见内存错误

**1：对空指针进行解引用**

**2：越界访问**

动态内存指的是动态指的是在需求改变时我们可以随时更改内存，但是它毕竟每次申请出来的也是一个确定的空间，所以你的指针不能跑到空间外面去  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F34f683bbe45148ecaf595c8e0311ddd4.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

**3：对非动态内存进行free**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8ff90125709e4fa5b78cada45ffe1b12.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

**4：使用free释放一块动态开辟内存的一部分**

free释放的是开辟的空间的首位置，使用时指针不能变化，不然就非法释放别的内存了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F52837e18a1f44c98811b363e684403d0.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

**5：对同一块动态内存多次释放**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0e9796e1cdd34434a4e2492c22bba51e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121002053)

**6：内促泄漏**  
如果开辟的内存忘记释放（当然是工程量大的情况），极易造成内存泄漏，而排查内存泄漏是一项很麻烦的事