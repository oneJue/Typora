 

- [指导获取：密码7281](https://url18.ctfile.com/f/22722418-803125355-edf378)
- [专栏目录首页：【专栏必读】王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121004242?spm=1001.2014.3001.5502)
- [王道考研408计算机组成原理万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/120664162?spm=1001.2014.3001.5502)
- [王道考研408数据结构+计算机算法设计与分析万字笔记](https://blog.csdn.net/qq_39183034/article/details/121501138?spm=1001.2014.3001.5501)
- [王道考研408计算机网络+湖科大教书匠计算机网络+网络编程万字笔记](https://zhangxing-tech.blog.csdn.net/article/details/125668174)

### 文章目录

- [一：时间片轮转调度算法（RR）](#RR_14)
- [二：优先级调度算法（HPF）](#HPF_102)
- [三：多级反馈队列调度算法（MFQ）](#MFQ_159)
- [总结](#_245)

**进程调度算法也称为CPU调度算法，操作系统内存在着多种调度算法，有的调度算法适用于作业调度，有的调度算法适用于进程调度，有的两者都适用。常见的调度算法有（本节介绍适合于分时系统、交互式系统的调度算法）：**

- 时间片轮转调度算法（RR）
- 最高优先级调度算法（HPF）
- 多级反馈队列调度算法（MFQ）

# 一：时间片轮转调度算法（RR）

**时间片轮转调度算法（RR）：公平地、轮流地为各个进程服务，让每个进程在一定时间间隔内都可以得到响应。按照各进程到达就绪队列的顺序，轮流让各个进程执行一个时间片\(Quantum\)，若进程未在一个时间片内执行完毕，则剥夺处理机，然后将其放入就绪队列重新排队**

- 用于**进程调度**（因为只有作业放入内存建立了相应进程后，才能被分配处理机时间片）
- 若进程未能在时间片内运行完毕，则会被强行剥夺，属于**抢占式算法**，由时钟装置发出**时钟中断**通知CPU时间片已到

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F317f0eacee9f4dbeaa736c9026f6274f.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

**例如，下面有4个进程 P 1 P\_\{1\} P1​、 P 2 P\_\{2\} P2​、 P 3 P\_\{3\} P3​和 P 4 P\_\{4\} P4​，他们到达就绪队列的到达时间和运行时间如下表所示，使用 P n \( m \) P\_\{n\}\(m\) Pn​\(m\)表示当前进程的剩余运行时间为 m m m**

| 进程 | 到达时间 | 运行时间 |
| --- | --- | --- |
| P 1 P\_\{1\} P1​ | 0 | 5 |
| P 2 P\_\{2\} P2​ | 2 | 4 |
| P 3 P\_\{3\} P3​ | 4 | 1 |
| P 4 P\_\{4\} P4​ | 5 | 6 |

**假设时间片大小为2，其变化过程如下**

- 在0时刻， P 1 \( 5 \) P\_\{1\}\(5\) P1​\(5\)。 P 1 P\_\{1\} P1​到达，**让 P 1 P\_\{1\} P1​上处理机运行一个时间片**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F98ebdc0d2d6342008f98f22449e8e4be.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在2时刻，有 P 2 \( 4 \) P\_\{2\}\(4\) P2​\(4\)， P 1 \( 3 \) P\_\{1\}\(3\) P1​\(3\)。 **P 2 P\_\{2\} P2​到达就绪队列**，同时此时 **P 1 P\_\{1\} P1​的时间片用完，被剥夺处理机，重新放回就绪队列队尾** （需要注意这种情况中**默认新到达的进程先进入就绪队列**）  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F86601f89f5614357800dc23a80ef8c23.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 此时 P 2 P\_\{2\} P2​处于就绪队列队头，**因此它上处理机**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4920dde0fb40473392916bb8715b7bd8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在4时刻， P 1 \( 3 \) P\_\{1\}\(3\) P1​\(3\)， P 3 \( 1 \) P\_\{3\}\(1\) P3​\(1\)， P 2 \( 2 \) P\_\{2\}\(2\) P2​\(2\)。 **P 3 P\_\{3\} P3​到达就绪队列**， P 3 P\_\{3\} P3​插入到就绪队列队尾，而此时 P 2 P\_\{2\} P2​时间片也已经到了，因此 P 2 P\_\{2\} P2​下处理机插入到队尾。**此时 P 1 P\_\{1\} P1​处于队头，因此 P 1 P\_\{1\} P1​处理机**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa218585d2ec24daf9c46f56e26c33971.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在5时刻， P 3 \( 1 \) P\_\{3\}\(1\) P3​\(1\)， P 2 \( 2 \) P\_\{2\}\(2\) P2​\(2\)， P 4 \( 6 \) P\_\{4\}\(6\) P4​\(6\)。 **P 4 P\_\{4\} P4​到达就绪队列**， P 4 P\_\{4\} P4​插入到就绪队列队尾。**此时不进行调度，因为 P 1 P\_\{1\} P1​还处于运行态，它的时间片还没有用完**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F8191b29a5aba4e9eb7487bf222460da1.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在6时刻， P 3 \( 1 \) P\_\{3\}\(1\) P3​\(1\)， P 2 \( 2 \) P\_\{2\}\(2\) P2​\(2\)， P 4 \( 6 \) P\_\{4\}\(6\) P4​\(6\)， P 1 \( 1 \) P\_\{1\}\(1\) P1​\(1\)。 **P 1 P\_\{1\} P1​的时间片用完，下处理机，重新放回队尾，然后 P 3 P\_\{3\} P3​开始运行**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffeb1ec65a06645059e0a29777b819aa3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在7时刻， P 2 \( 2 \) P\_\{2\}\(2\) P2​\(2\)， P 4 \( 6 \) P\_\{4\}\(6\) P4​\(6\)， P 1 \( 1 \) P\_\{1\}\(1\) P1​\(1\)。虽然 P 3 P\_\{3\} P3​的时间片还没用完，但是它已经结束了，**所以 P 3 P\_\{3\} P3​主动放弃处理机， P 2 P\_\{2\} P2​上处理机**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F94f36350ae594a85b06d29d9b67358d8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在9时刻， P 4 \( 6 \) P\_\{4\}\(6\) P4​\(6\)， P 1 \( 1 \) P\_\{1\}\(1\) P1​\(1\)。 P 2 P\_\{2\} P2​时间片用完，而恰好 P 2 P\_\{2\} P2​也已经结束，**此时 P 4 P\_\{4\} P4​上处理机**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe0d9c765b411416ba2a48fa680cbfc1d.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在11时刻， P 1 \( 1 \) P\_\{1\}\(1\) P1​\(1\)， P 4 \( 4 \) P\_\{4\}\(4\) P4​\(4\)。 P 1 P\_\{1\} P1​时间片用完，放回队尾，P1上处理机  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fbb710cfc173f4d22b28e43384f2091a3.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在12时刻， P 4 \( 4 \) P\_\{4\}\(4\) P4​\(4\)。 P 1 P\_\{1\} P1​运行完毕主动放弃处理机， P 4 P\_\{4\} P4​上处理机  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F633fb6b24948449484d28b1b414000c4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在14时刻，**就绪队列为空， P 4 P\_\{4\} P4​会继续再运行1个时间片**

- 在16时刻，所有进程运行完毕

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fb8ab8346d79547b2a463f8b9cd95b8b9.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

**假设时间片大小为5，其变化过程如下**

- 在0时刻， P 1 \( 5 \) P\_\{1\}\(5\) P1​\(5\)。 P 1 P\_\{1\} P1​到达，**让 P 1 P\_\{1\} P1​上处理机运行一个时间片**
- 在2时刻， P 2 \( 4 \) P\_\{2\}\(4\) P2​\(4\)。 P 2 P\_\{2\} P2​到达，但是 P 1 P\_\{1\} P1​的时间片还未结束，暂不调度
- 在4时刻， P 2 \( 4 \) P\_\{2\}\(4\) P2​\(4\)， P 3 \( 1 \) P\_\{3\}\(1\) P3​\(1\)。 P 3 P\_\{3\} P3​到达，但是 P 1 P\_\{1\} P1​的时间片还未结束，暂不调度
- 在5时刻， P 2 \( 4 \) P\_\{2\}\(4\) P2​\(4\)， P 3 \( 1 \) P\_\{3\}\(1\) P3​\(1\)， P 4 \( 6 \) P\_\{4\}\(6\) P4​\(6\)。 P 4 P\_\{4\} P4​到达，同时 P 1 P\_\{1\} P1​运行结束，接着让 P 2 P\_\{2\} P2​上处理机
- 在9时刻， P 3 \( 1 \) P\_\{3\}\(1\) P3​\(1\)， P 4 \( 6 \) P\_\{4\}\(6\) P4​\(6\)。 P 4 P\_\{4\} P4​运行结束，主动放弃处理机发生调度。让 P 3 P\_\{3\} P3​上处理机
- 在10时刻， P 4 \( 6 \) P\_\{4\}\(6\) P4​\(6\)。 P 3 P\_\{3\} P3​运行结束，主动放弃处理机，发生调度。让 P 4 P\_\{4\} P4​上处理机
- 在15时刻。 P 4 P\_\{4\} P4​时间片用光，且就绪队队列为空，故让 P 4 P\_\{4\} P4​继续下一个时间片
- 在16时刻。所有进程运行结束

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Ffb8fdd3093c3470ba053b957dbc80df4.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

**时间片大小的影响**

- **时间片太大**：使得每个进程都可以在一个时间片内完成，此时RR算法就退化为了FCFS算法，并且会增大进程响应时间
- **时间片太小**：由于进程调度、切换是有时间代价的，因此如果时间片太小，会导致进程切换过于频繁，系统会花大量时间来处理进程切换

**优缺点**

- **优点：** 公平、响应快
- **缺点：** 由于高频率的进程切换，因此会有一定开销，且不能区分任务的紧急程度

**是否会导致饥饿现象：不会**

# 二：优先级调度算法（HPF）

随着计算机的发展，特别是实时操作系统的出现，越来越多的应用场景需要根据**任务的紧急程度（优先级）** 来决定处理顺序。进程的优先级可以被划分为

- **静态优先级**：创建进程的时候，就已经确定好了优先级，整个运行过程优先级都不会发生变化
- **动态优先级**：根据进程的动态变化调整优先级（比如当进程的运行时间增加则降低其优先级、进程等待时间增加则提高其优先级等等）

---

**优先级调度算法（HPF） ：每个作业/进程都有各自的优先级，调度时选择优先级最高的作业/进程**

- 可用于**作业调度、进程调度，甚至I/O调度**
- 有**抢占式**（当就绪队列中出现优先级高的进程，当前进程被挂起，调度优先级高的进程）和**非抢占**（当就绪队列中出现优先级高的进程，运行完当前进程，再选择优先级高的进程）两种

**比如，下面有4个进程 P 1 P\_\{1\} P1​、 P 2 P\_\{2\} P2​、 P 3 P\_\{3\} P3​和 P 4 P\_\{4\} P4​，他们到达就绪队列的到达时间、运行时间及进程优先数，如下表所示**

- 一般来说，优先数越大，优先级越高

| 进程 | 到达时间 | 运行时间 | 优先数 |
| --- | --- | --- | --- |
| P 1 P\_\{1\} P1​ | 0 | 7 | 1 |
| P 2 P\_\{2\} P2​ | 2 | 4 | 2 |
| P 3 P\_\{3\} P3​ | 4 | 1 | 3 |
| P 4 P\_\{4\} P4​ | 5 | 4 | 2 |

**采用非抢占式优先级调度算法，意味着只有进程主动放弃处理机时才发生调度，因此其过程也显而易见**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fcf92fceedc1a46b8b9929d981da7209e.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)  
**采用抢占式优先级调度算法，除了进程主动放弃处理机时会发生调度之外，当新进程到达时（就绪队列改变）也要考虑是否会发生调度，其变化过程如下**

- 在0时刻， P 1 P\_\{1\} P1​到达， **P 1 P\_\{1\} P1​上处理机**
- 在2时刻， P 2 P\_\{2\} P2​到达，使就绪队列发生改变，同时**由于 P 2 P\_\{2\} P2​的优先级要高于 P 1 P\_\{1\} P1​，因此 P 1 P\_\{1\} P1​被剥夺回到就绪队列， P 2 P\_\{2\} P2​上处理机**
- 在4时刻， P 3 P\_\{3\} P3​到达，其优先级高于 P 2 P\_\{2\} P2​，因此 P 2 P\_\{2\} P2​回到就绪队列， P 3 P\_\{3\} P3​上处理机
- 在5时刻， **P 3 P\_\{3\} P3​结束运行，主动放弃处理机**，同时恰好 P 4 P\_\{4\} P4​到达，**这里 P 2 P\_\{2\} P2​和 P 4 P\_\{4\} P4​优先级是一样的，但是 P 2 P\_\{2\} P2​要比 P 4 P\_\{4\} P4​更早进入就绪队列，所以 P 2 P\_\{2\} P2​上处理机**
- 在7时刻， P 2 P\_\{2\} P2​完成， P 4 P\_\{4\} P4​上处理机
- 在11时刻， P 4 P\_\{4\} P4​完成， P 1 P\_\{1\} P1​上处理机
- 在16时刻，所有进程结束运行

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F9ebc0e55c0e142e48220c6ea794dcb7a.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

**进程优先级的设置原则**

- **系统进程大于用户进程**：系统进程作为系统的管理者，理应拥有更高的优先级
- **交互型进程大于非交互型进程（前台进程大于后台进程）**：类比使用手机时的前台应用程序和后台应用程序
- **I/O密集型进程大于计算密集型进程**：I/O密集型进程是指那些会频繁使用I/O设备的进程；计算密集型进程是指那些频繁使用CPU的进程。如果将I/O密集型进程设置得更高，就更有可能让I/O设备尽早开始投入工作，进而提升系统的整体效率

**优缺点**

- **优点：** 使用优先级区分紧急程度、重要程度、适用于实时操作系统，可以灵活调整对各种作业/进程的偏好程度
- **缺点：** 有可能导致低优先级进程得不到运行

**是否会导致饥饿：会**

# 三：多级反馈队列调度算法（MFQ）

**多级反馈队列调度算法（MFQ）：MFQ算法其实是RR算法和HPF算法的综合和发展**

- **多级**：**表示有多个队列，每个队列优先级从高到低，同时优先级越高时间片越短**
- **反馈**：**表示如果有新的进程加入优先级高的队列时，立刻停止当前正在运行的进程，转而去运行优先级高的队列**

**算法具体规则**

- 设置了多个队列，赋予每个队列**不同的优先级**，每个队列**优先级从高到低**，同时**优先级越高时间片越短**
- **新的进程会被放入到第一级队列的末尾，按FCFS算法的原则排队等待被调度，如果在第一级队列规定的时间片内没有运行完毕，则将其转入第二级队列的末尾，依次类推，直至完成**
- **当较高优先级的队列为空时，才调度较低优先级队列中的进程。如果进程运行时，有新的进程进入较高优先级队列时，则停止当前运行的进程并将其移入原队列的末尾，接着让较高优先级的进程运行**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F91a2c03a5bdd493a9c0bc90af3bbea05.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

**比如，下面有3个进程 P 1 P\_\{1\} P1​、 P 2 P\_\{2\} P2​和 P 3 P\_\{3\} P3​，他们到达就绪队列的到达时间和运行时间，如下表所示**

| 进程 | 到达时间 | 运行时间 |
| --- | --- | --- |
| P 1 P\_\{1\} P1​ | 0 | 8 |
| P 2 P\_\{2\} P2​ | 1 | 4 |
| P 3 P\_\{3\} P3​ | 5 | 1 |

**根据“算法规则”中的叙述，建立优先级队列，其优先级和时间片分配如下**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0cbfa025a84c48fd9b16fa52e0dd1d88.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

**整个变化过程如下**

- P 1 P\_\{1\} P1​到达，**进入第1级队列，按FCFS算法分配时间片**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7dff9d85fc244b9ba25719f1b398c87e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 第1级队列其时间片只有1，因此1个时刻后， P 1 P\_\{1\} P1​时间片用完。**但是其剩余时间还有7，还未运行完毕，所以会进入第2级队列**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F011748d8283a4ff8a013dcfd8628d83d.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 同时在1时刻， P 2 P\_\{2\} P2​到达并进入第1级队列，**对于 P 1 P\_\{1\} P1​来说，它处在第2级队列，由于此时第1级队列不为空，所以它不会被调度，此时先调度第1级队列中的 P 2 P\_\{2\} P2​**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F688dd3e283a54c89afac249fdc2ab468.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- P 2 P\_\{2\} P2​的一个时间片用完之后继续放到第2级队列  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2f31f179f5114312b97a3741d0df24e5.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在2时刻没有新的进程到来，**同时对于 P 1 P\_\{1\} P1​和 P 2 P\_\{2\} P2​来说相对于它们所在的更高一级的优先级队列，也就是第1级队列是空的，因此为它们分配时间片，按照FCFS原则， P 1 P\_\{1\} P1​被调度，时间片为2**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fce0482b5b8ee48518556ff41a9e665e6.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- P 1 P\_\{1\} P1​时间片到后还没有完成运行，因此会被放入第3级队列  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe72bd87502354707bfc5033025b4fae5.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 接着 P 2 P\_\{2\} P2​被调度  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fc759a68644e64f41a971fb094f84ddfe.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 在 P 2 P\_\{2\} P2​运行1个时间片后，**也即在5时刻 P 3 P\_\{3\} P3​到达并进入第1级队列，此时更高优先级队列中有进程存在，所以处在处理机上的 P 2 P\_\{2\} P2​会被剥夺下来，仍然回到其所在的队列队尾，接着让 P 3 P\_\{3\} P3​上处理机**  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F35317e4e97e74f08840658039752f760.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- P 3 P\_\{3\} P3​运行1个时间片后结束运行  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fff6b35387a4d466aa7bb859fcae0486e.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 然后 P 2 P\_\{2\} P2​继续被调度，其剩余时间为2，所以正好运行2个时间片后结束运行  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F065ddfc85dc94e43be77c7c010198245.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 此时第1级、第2级队列为空，因此处在第3级队列 P 1 P\_\{1\} P1​此时可以被调度，因此分配4个时间片  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fa94d836ca20642618f4be3ae038c54d2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

- 4个时间片结束之后， P 1 P\_\{1\} P1​剩余时间还有1，**而此时它已经处在了最下级的队列了，因此重新放回最下级队列队尾即可**，然后结束运行  
  ![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Feefa0627a6d44cab8b79f29f6eab65a2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)

**从以上的叙述中大家可以感受到**：

- 短作业可能可以在第一级队列很快地被处理完
- 长作业如果第一级处理不完，可以移入下一级等待。等待时间虽然变长了，但是运行时间也会变长

**多级反馈队列的优点**

- **终端型作业用户**：短作业优先（例如命令行输入命令）
- **短批处理作业用户**：周转时间较短
- **长批处理作业用户**：经过前面几个队列得到部分执行，不会长期得不到处理

**是否会导致饥饿：会**

---

# 总结

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F4d348cf73e154edc9b79eee89ad97989.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F121131983)