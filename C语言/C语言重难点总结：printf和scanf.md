 

# 一：printf

## （1）格式字符总结

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401164512302.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115378074)

```c
int main()
{
            
            
	int a = 10;//有符号十进制数
	unsigned int b = -1;//无符号十进制数
	double c = 3.1415926;//小数
	char d = 't';//字符
	const char* str = "just a test!";//字符串
	int* p = &a;//指针

	printf("以十进制形式输出符号整数a=%d\n", a);
	printf("以八进制形式输出符号整数a=%o\n", a);
	printf("以十六进制形式输出符号整数a=%X\n", a);
	printf("以十进制形式输出无符号整数b=%u\n", b);

	printf("以小数形式输出单双精度实数c=%f\n", c);
	printf("以指数形式输出单双精度实数c=%e\n", c);

	printf("输出字符d=%c\n", d);
	printf("输出字符串str=%s\n", str);

	printf("以十六进制形式输出a的地址p=%p\n", p);

}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401165656977.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115378074)

## （2）flags总结

flags  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021040116575593.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115378074)

```c
int main()
{
            
            
	int a=10;
	printf("%6d（默认右对齐）\n", a);
	printf("%06d（默认右对齐，空位用0填充）\n", a);
	printf("%-6d（使用-号左对齐）\n", a);
	cout << endl;

	printf("以八进制形式输出符号整数a=%o\n", a);
	printf("以八进制形式输出符号整数（带前缀）a=%#o\n", a);
	printf("以十六进制形式输出符号整数a=%X\n", a);
	printf("以十六进制形式输出符号整数（带前缀）a=%#X\n", a);
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401190352419.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115378074)

## （3）其他说明符

宽度标识符  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401165926973.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115378074)

精度说明符  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401190519833.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115378074)  
长度说明符  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401190533165.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115378074)

```c
	printf("十进制数a=10指定宽度为6输出:%6d\n", a);
	printf("十进制数a=10指定宽度为6输出，空位用0补齐:%06d\n", a);
	cout << endl;

	printf("十进制数a=10指定精度为6输出，自动默认填充0:%.6d\n", a);
	printf("以小数形式输出单双精度实数，保留3位c=%.3f\n", c);
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401191306868.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115378074)

## （4）练习

![定义一个inta=380](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210401191530792.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F115378074)