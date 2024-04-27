 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- - [（1）debug与release](#1debugrelease_4)
  - [（2）调试一个程序](#2_13)
  - [（3）总结-gdb选项](#3gdb_75)

## （1）debug与release

程序的发布方式有`debug`和`release`两种模式，`release`没有调试信息，不能进行调试，体积较小，`debug`携带调试信息，可以进行调试，但是文件较大

- Linux中，使用gcc/g++编译的程序，默认使用的是release模式，所以就不能直接使用gdb进行调试。如果想要调试，必须在进行gcc/g++编译时，携带`-g`选项
- 如下，同一个文件分别使用`debug`（带-d的）和`release`，其中`debug`的文件大小明显大于`release`  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128230226250.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)
- 使用`readelf -S`命令可以查看`debug`文件  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210128230450389.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)

## （2）调试一个程序

如下是一个计算1-100的和的C语言程序，进行编译（不要忘记`-g`）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129135706485.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129135807290.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)  
**1：开启调试**

- 输入`gdb [文件名]`，进入调试状态

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021012913593511.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)  
**2：显示源代码**

- 输入`list(简写为l)`，**显示源代码**便于观察，**每次显示10行**，按`Enter`向下翻
- 输入`list n(简写为l n)`，显示第n-5行第n+5行

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021012914011666.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)  
**3：断点**

- 输入`break n(简写为b n)`，在第n行**打上一个断点**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129140749531.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)
- 输入`info b`，可以**查看断点信息**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129141121925.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)

- 输入`d [断点编号]`，**删除**对应编号的断点
- 输入`disable breakpoint [断点编号]`，**禁用**对应编号的断点；输入`disable breakpoints`，禁用所有断点  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129154037456.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)
- 输入`enable breakpoint [断点编号]`，**启用**对应编号的断点；输入`enable breakpoints`，启用所有断点  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129154419410.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)

**4：运行程序**  
_①：开启运行_

- 输入`run(简写为 r)`，程序开始运行，如果没有断点将直接输入程序结果，如果右端点，程序将**停止到第一个断点处**。
- 类似于在VS中，在某行上按下了`F5`打上了断点，然后再按`F5`，程序将跳转到此断点处，但还没有执行  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129142417803.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)

_②：逐语句，逐过程执行_

如上，此时来到一个断点处，类似于VS，你会有两种选择：要么逐语句`F10`，不进入此函数，直接执行此函数跳到下一行语句，要么逐过程`F11`进入此函数执行

- 输入`next（简写为n）`，程序会到下一句，  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129143132515.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)
- 若输入`step(简写为s)`，程序会进入这个函数

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129143610938.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)

_③：查看变量_

- 上述程序再运行过程，若需要查看变量的值，则输入`p [变量名]`  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129144655745.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)
- 若需要动态查看变量名，则输入`display [变量名]`，接着再执行程序，程序在执行过程除了显示当前执行行的语句外，还会显示执行过程中的变量  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129145018902.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)
- 在动态查看中若不想查看某个变量了，取消的方式和设置的方式稍微有点区别。可以发现设置display后，**gdb为每个要展示的变量都设置了相应的编号**，所以取消时输入`undisplay [变量名所对应的编号]`；如果要全部取消则直接输入`display`  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129145600604.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)

_④：执行中跳转_

- 上述，进入sum函数，就在不断执行循环，如果临时决定想要跳出这个循环，或者直接到某一行，那么就输入`until n`，其中n表示行号  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129150449756.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)
- 如果想要从一个断点，跳转到下一个断点，则输入`c`  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210129155115396.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)
- 如果进入了某个函数，想要立即结束掉这个函数直接返回到调用处，则输入`finish`。  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021012916000527.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116149255)

## （3）总结-gdb选项

| 选项 | 描述 |
| --- | --- |
| list \[行号\] | 显示源代码 |
| list \[函数名\] | 显示某函数的源代码 |
| run | 运行程序 |
| next | 逐语句执行 |
| step | 进入函数 |
| break \[行号\] | 在某行出设置断点 |
| break \[函数名\] | 在某函数开头设置断点 |
| info break | 查看断点信息 |
| finish | 结束当前函数，返回调用处 |
| p \[\]变量\] | 打印变量的值 |
| c | 跳转到下一个断点 |
| d \[断点编号\] | 删除对应编号的断点 |
| delete breakpoints | 删除所有断点 |
| disable/enable breakpoint \[断点编号\] | 禁用/启用对应编号断点 |
| display \[变量名\] | 跟踪变量，执行时每次都显示 |
| undisplay | 取消跟踪 |
| until \[行号\] | 跳转到对应行 |
| quit | 退出 |