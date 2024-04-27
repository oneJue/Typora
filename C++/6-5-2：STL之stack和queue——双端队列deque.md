 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [双端队列-deque](#deque_4)

# 双端队列-deque

> deque是一种双向开口的连续线性空间。所谓双向开口，意思就是可以在头尾两端分别进行元素的插入和删除操作

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416165355515.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675751)

deque的出现是为了解决融合vector和list，以达到取长补短的目的（**它想要解决vector插入删除元素效率慢以及list不能随机访问的缺点**），但是就目前看，它的确失败了。

deque的实现相当复杂，尤其它的迭代器，可谓是设计的精华，所以我们的主要目的就是去了解一下它的底层，同时明白为什么deque不能替代vector和list，而适合作为适配器使用。

**deque是由一段一段的定量的连续空间构成，一旦需要在在deque的前端或者尾端增加新的空间，则会再申请一定量连续的空间，拼接在其前部或尾部。deque的目的就是要在这写分段的定量连续空间上，维护出一种整体连续的假象，每一段连续的空间我们称之为一个缓冲区**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416170139143.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675751)  
可以发现，deque采用map作为主控，map中的每个元素都是一个指针，指向我们前面说到过的连续空间，  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416170500438.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675751)  
正是因为这样的设计，所以导致deque的迭代器异常复杂，因为我们知道deque的连续其实是一个假象。因为迭代器必须时时刻刻判断自己是否已经处于缓冲区的边缘，如果超越边界，它就必须跳到下一个缓冲区上。  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416190247163.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675751)

因此它的实现时这样子的  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021041619060614.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675751)  
假设现在产生了一个deque对象，并且令其缓冲区的大小为32，所以每个缓冲区可以容纳4个元素，假设经过一些插入操作后，deque有了20个元素，那么20个元素就需要20/8=3个缓冲区。如下map，因此有三个结点，其中有两个迭代器（start和finish）分别就在这几个结点之间游走，游走到某一个结点上之后，其迭代器中还有first和last用于维护结点对应的缓冲区  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416191316351.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675751)  
迭代器在游走缓冲区时最关键的操作就是一旦游走的边缘，就要去下一个缓冲区了  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416191500523.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675751)  
deque最为麻烦的就是插入元素的操作，插入元素时如果缓冲区的空间更多，那么自然尾插即可，但是一旦缓冲区不够的话就会开辟新的缓冲区，同时涉及其他操作  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416191936624.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675751)  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210416191949645.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F117675751)  
**deque在实际生活中不会引用太频繁，因此他也无法代替vector和list，但是对于stack而言，由于stack只会设计一端的元素操作，如果用deque适配，那么就很方便；同时对于list而言，由于list只会设计一端插入一段删除的操作，如果用deque适配，那么也很方便**

因此我们的参数列表可以这样写

```cpp
#include <deque>
template<class T, class Container = deque<T>>
```