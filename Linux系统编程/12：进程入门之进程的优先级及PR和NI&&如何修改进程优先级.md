 

- [专栏目录首页：【专栏必读】王道考研408操作系统+Linux系统编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)

### 文章目录

- - [（1）什么是进程的优先级](#1_4)
  - [（2）进程优先级如何表示](#2_14)
  - [（3）PRI和NI](#3PRINI_20)
  - - [A：什么是PRI和NI](#APRINI_21)
    - [B：如何修改进程优先级](#B_31)
  - [（4）其他概念](#4_38)

## （1）什么是进程的优先级

**这里首先要区分优先级和权限的关系**：以食堂举例，你能去学生食堂而不能去职工食堂，这是因为你没有权限，你可以去食堂，但是你却排不上队，这是因为你的优先级不够（你跑的够不够快，排的是不是在前面）

换到进程中，**当进程太多时，进程就需要被合理的管理，总不能谁都抢着去占用CPU，所以CPU分配资源的先后顺序就是进程的优先级**  
在计算机中肯定是**进程数大于CPU的数量**，也就是人数太多而资源太少，因此优先级的作用就显得至关重要，它可以让优先级高的进程有优先执行权利，让那些不重要，优先级低的进程等一等，或者按照到其他CPU。

还是遵循操作系统管理的原则——“先描述，再组织”。你在Linux内核源代码里，在task\_struct中也能看见这部分内容  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307192348685.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189083)

## （2）进程优先级如何表示

使用命令`ps \-l`可以列出当前用户创建的进程，比如下面我使用`ps \-l`后会列出两个进程，一个进程就是`bash`，另一个则是我刚刚输入的命令  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307184948806.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189083)  
上面的`UID`实则就是我这用户的身份标识，如果我切换为root用户，可以发现`UID`也换了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307185241264.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189083)

## （3）PRI和NI

### A：什么是PRI和NI

上图中与优先级有关的两个信息是：PRI和NI

- PRI：**代表这个进程执行的优先级，其值越小越早被执行**
- NI：**代表这个进程的NICE值，用于修正进程优先级**

**调整进程时，调整的不是PRI，而是NI，通过修正NI以达到修改PRI。默认情况下PRI是80，nice是0，修正时每修改一次NICE，新的PRI=80+NICE的修正值**  
**NICE的取值范围为\[-20,19\]，因此PRI的范围为\[60,99\]。根据以上描述，NICE为负值时会使优先级变大，NICE为正值会使优先级变小**

### B：如何修改进程优先级

\(一般情况下，不建议修改，因为进程优先级是由操作系统自行完成的\)

这里为了说明清楚，我创建一个C语言文件，让其死循环在后台运行，然后修改其进程优先级。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021030719103054.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189083)  
**修改时，升级root权限，按top进入资源管理器，然后再按r，接着输入要修改的进程的PID，然后输入NICE值，比如下面我将其进程调至最大**（也就是输入负值，由于最小是-20，所以即便下面我输了一个非常小的数，其结果仍然是-20）  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210307191533855.gif&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116189083)

## （4）其他概念

- 竞争性：系统进程数目很多，但CPU资源有限，于是各进程了完成自己的任务，就要相互竞争资源
- 独立性：多个进程同时运行时，独享自己资源，互不干扰
- **并行**：多个进程在多个CPU下分别同时进行运行
- **并发**：多个进程在一个CPU下采用进程切换的方式，在一段时间内，让多个进程都能推进，好像在“同时运行”