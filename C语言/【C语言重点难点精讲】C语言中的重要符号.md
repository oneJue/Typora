 

### 文章目录

- [一：续接符和转义符](#_2)
- - [（1）续接符](#1_3)
  - [（2）转义字符](#2_25)
- [二：单引号和双引号](#_37)
- [三：逻辑运算符](#_54)
- [四：位运算](#_81)
- [四：左移右移](#_97)
- [五：前置++和后置++](#_117)
- [六：优先级](#_138)

# 一：续接符和转义符

## （1）续接符

如果一行写不下了可以使用续接符`\`进行换行

```c
int main()
{
            
            
  int a=1;
  int b=2;
  int c=3;
  if(1==a &&\//注意后面不要出现任何符号
     2==b &&\//注意后面不要出现任何符号
     3==c){
            
            
  printf("1\n");
  }else{
            
            
    printf("2\n");
  }
  return 0;

}

```

## （2）转义字符

常见转义字符  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fd383884267264fd8978ab1f89c37386c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_15%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)

**关于`\r`和`\`t**它们是不一样的

- 转义字符`\r`表示**回车**，回车的意思是回到本行的第一个字符处
- 转义字符`\n`表示**换行**，换行的意思是到下一行对应位置再输入

# 二：单引号和双引号

**第一：** 正常情况下单引号是字符，双引号是字符串

**第二：** 注意一点，C99规定，像’`1`'这样的叫做整形常量，被看作为了`int`型

```c
int main()
{
            
            
	printf("%d\n", sizeof('1'));//整型常量
	char c = 'abcd';//发生截断
	printf("%d\n", sizeof(c));
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F130d8631d27a48be8e85e9587290e53c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)

# 三：逻辑运算符

- 逻辑与`&&`：两个条件必须同时成立，有一个条件不成立则不成立
- 逻辑或`||`：有一个条件成立则成立。两个条件都不成立则不成立

他们会产生短路现象，从左向右判定时，**对于逻辑与来说，如果第一个已经判定不成立了那么就不需要看后面的了，对于逻辑或来说，如果第一个已经成立了那么就不需要看后面的了**

如下，可以使用这种短路，在不使用`if`的情况下进行逻辑判断

```c
int main()
{
            
            
	int flag = 0;
	scanf("%d", &flag);
	flag && show();
	//如果flag输入为1，那么还需要继续调用show进行判断
	//如果flag输入为0，那么不需要继续调用show
	flag || show();
	//如果flag输入为1，那么不需要继续调用show
	//如果flag输入为0，那么还需要继续调用show进行判断
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa0c6326d695045d4a1478729ba818afc.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)

# 四：位运算

**第一：** 位运算基本规则如下  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F5f7be9d5a404477cbceeb3df295887a9.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)

**第二：** 异或运算支持交换律和结合律

```c
int main()
{
            
            
	printf("%d\n", 5 ^ 4 ^ 5);
	printf("%d\n", 5 ^ 5 ^ 4);
	printf("%d\n", 5 ^( 5 ^ 4 ));
}
```

# 四：左移右移

**第一：** 左移和右移的基本规则

**`<<`左移**：最高位丢弃，最低位补零  
**`>>`右移**：

- 无符号数：最低位丢弃，最高位补零（逻辑右移）
- 有符号数：最低位丢弃，最高位补符号位（算数右移）

**第二：** 相关演示

左移  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fad4511a4cb5442b3b98575658bcd648c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)  
逻辑右移  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F60a2dcda8086478aa57cc2f830a951d8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)  
算数右移：最高位补1  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F11ee1e40628f43c3b53df248be82cac7.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)

# 五：前置++和后置++

**第一：** 后置++是先使用后自增，前置++是先自增后++

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb161ab2b39524aa0bcaedcd3af60821c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa7c16855666e42299f58e639a82c287c.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)  
**第二：** 汇编角度分析  
后置++是先使用后自增  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe012b8f19af14f489e5bc1f0a9db8a1f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)

- 如果没有人使用，那么直接自增  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe4354b323a8c4f169ec3b39548369ce4.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)

前置++是先自增后++  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb9d589e9017c444ca5e23a6da0c377aa.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)

# 六：优先级

**第一：** ！ > 算术运算符 > 关系运算符 > \&\& > || > 赋值运算符  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F3a4ff08bd9164d29b1b6498c8d8b13f8.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F44bba17138044b2a895772af2a388b16.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F84cc685c14d94c3b927524afb997630b.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F10e894638f1b42078dbd55d1e87add9a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120702100)