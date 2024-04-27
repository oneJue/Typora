 

- [专栏目录首页：【专栏必读】王道考研408数据结构+计算机算法设计与分析万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121501138?spm=1001.2014.3001.5502)

### 文章目录

- [一：C语言实现](#C_5)
- [二：C++实现](#C_200)
- [三：Java实现](#Java_432)
- - [（1）单链表](#1_435)
  - [（2）双链表](#2_614)
- [四：Python实现](#Python_798)

# 一：C语言实现

头文件

```c
#pragma once
#include <stdio.h>
#include <stdlib.h>
#include <assert.h>
typedef int SlistDataType;

//结点类型定义
typedef struct SlistNode
{
            
            
	SlistDataType data;//数据域
	struct SlistNode* next;//指向下一个结点的指针
}SlNode;

void SlistPushBack(SlNode** head,SlNode** tail, SlistDataType x);//尾插
//void SlistPushBack(SlNode** phead, SlistDataType x);//尾插
void SlistPopBack(SlNode** phead);//尾删
void SlistPushFront(SlNode** phead,SlistDataType x);//头插
void SlistPopFront(SlNode** phead);//头删

SlNode* SlistFind(SlNode* head, SlistDataType x);//找元素

SlNode* Slistinsert(SlNode* pos, SlistDataType x);//任意位置插入
SlNode* Slistdelete(SlNode* pos);//任意位置删除

void SlistPrint(SlNode* head);//打印

```

实现

```c
#include "Slist.h"

void SlistPushBack(SlNode** phead, SlNode** ptail, SlistDataType x)//尾插-带尾节点
{
            
            

	SlNode* NewNode = (SlNode*)malloc(sizeof(SlNode));
	NewNode->data = x;
	NewNode->next = NULL;

	if ((*phead) == NULL)
	{
            
            
		(*phead)= NewNode;
		(*ptail) = NewNode;
	}
	else
	{
            
            
		(*ptail)->next = NewNode;
		(*ptail) = NewNode;
	}
	
}

//void SlistPushBack(SlNode** phead, SlistDataType x)//尾插
//{
            
            
//	SlNode* NewNode = (SlNode*)malloc(sizeof(SlNode));
//	if (NewNode == NULL)
//	{
            
            
//		printf("申请结点失败\n");
//		exit(-1);
//	}
//	NewNode->data = x;
//	NewNode->next = NULL;
//	SlNode* tail =(*phead);
//	if ((*phead)== NULL)
//	{
            
            
//		(*phead) = NewNode;
//	}
//	else
//	{
            
            
//		while (tail->next != NULL)
//		{
            
            
//			tail = tail->next;
//		}
//		tail->next=NewNode;
//	}
//	
//
//
//}

void SlistPopBack(SlNode** phead)//尾删
{
            
            
	if ((*phead) == NULL)
	{
            
            
		printf("错误，空链表");
		exit(-1);
	}
	else if ((*phead)->next == NULL)
	{
            
            
		free(*phead);
		(*phead) = NULL;
	}
	else
	{
            
            
		SlNode* current = (*phead);
		SlNode* pre = NULL;
		while (current->next != NULL)
		{
            
            
			pre = current;
			current = current->next;
		}
		free(current);
		pre->next = NULL;
	}

}
void SlistPushFront(SlNode** phead, SlistDataType x)//头插
{
            
            
	SlNode* newNode = (SlNode*)malloc(sizeof(SlNode));
	newNode->data = x;
	newNode ->next= NULL;
	if (*phead == NULL)
	{
            
            
		(*phead) = newNode;
	}
	else
	{
            
            
		newNode->next = (*phead);
		(*phead) = newNode;
	}
	

	
}
void SlistPopFront(SlNode** phead)//头删
{
            
            
	if ((*phead) == NULL)
	{
            
            
		printf("错误，链表为空\n");
		exit(-1);
	}
	if((*phead)->next==NULL)
	{
            
            
		(*phead) = NULL;
	}
	else
	{
            
            
		(*phead) = (*phead)->next;
	}

}
SlNode* SlistFind(SlNode* head, SlistDataType x)
{
            
            
	SlNode* current = head;
	while (current->data != x)
	{
            
            
		current = current->next;
	}
	return current;

}
SlNode* Slistinsert(SlNode* pos, SlistDataType x)
{
            
            
	SlNode* newNode = (SlNode*)malloc(sizeof(SlNode));
	newNode->data = x;
	newNode->next = NULL;
	newNode->next = pos->next;
	pos->next = newNode;

}
SlNode* Slistdelete(SlNode* pos)
{
            
            
	assert(pos);
	if (pos->next != NULL)
	{
            
            
		SlNode* current = pos->next;
		pos->next = current->next;
		free(current);
		current = NULL;
	}


}
void SlistPrint(SlNode* head)//打印
{
            
            
	SlNode* current = head;//申请一个遍历指针，因为头指针不能乱跑
	while (current!= NULL)
	{
            
            
		printf("%d->", current->data);
		current = current->next;//指针后移
	}
	printf("NULL");
}


```

# 二：C++实现

 -    C++中链表对应STL中的list，所以这里以实现list为例

```cpp
#pragma once
#include<iostream>
namespace MyList
{
            
            
    // List的节点类
    template<class T>
    struct ListNode
    {
            
            
        ListNode(const T& val = T())
            :_pPre(nullptr)
            ,_pNext(nullptr)
            ,_val(val)
        {
            
            }
        ListNode<T>* _pPre;
        ListNode<T>* _pNext;
        T _val;
    };
    //List的迭代器类
    template<class T, class Ref, class Ptr>
    struct ListIterator
    {
            
            
        typedef ListNode<T>* PNode;
        typedef ListIterator<T, Ref, Ptr> Self;
        ListIterator(PNode pNode = nullptr)
            :_pNode(pNode)
        {
            
            }
        ListIterator(const Self& l)
            :_pNode(l._pNode)
        {
            
            }
        T& operator*()
        {
            
            
            return _pNode->_val;
        }
        T* operator->()
        {
            
            
            return &_pNode->_val;
        }
        Self &operator++()
        {
            
            
            _pNode = _pNode->_pNext;
            return *this;
        }
        Self operator++(int)
        {
            
            
            Self temp(_pNode);
            _pNode = _pNode->_pNext;
            return temp;
        }
        Self& operator--()
        {
            
            
            _pNode = _pNode->_pPre;
            return *this;
        }
        Self& operator--(int)
        {
            
            
            Self temp(_pNode);
            _pNode = _pNode->_pPre;
            return temp;
        }
        bool operator!=(const Self& l)
        {
            
            
            return _pNode != l._pNode;
        }
        bool operator==(const Self& l)
        {
            
            
            return _pNode == l._pNode;
        }
        PNode _pNode;
    };

    //list类
    template<class T>
    class list
    {
            
            
        typedef ListNode<T> Node;
        typedef Node* PNode;
    public:
        typedef ListIterator<T, T&, T*> iterator;
        typedef ListIterator<T, const T&, const T&> const_iterator;
    public:
        ///
        // List的构造
        list()
        {
            
            
            CreateHead();
        }
        list(int n, const T& value = T())
        {
            
            
            CreateHead();
            for (int i = 0; i < n; ++i) {
            
            
                push_back(value);
            }
        }
        template <class Iterator>
        list(Iterator first, Iterator last)
        {
            
            
            CreateHead();
            while (first != last) {
            
            
                push_back(*first);
                ++first;
            }
        }
        list(const list<T>& l)
        {
            
            
            CreateHead();
            list<T>temp(l.begin(), l.end());
            swap(temp);
        }
        list<T>& operator=(const list<T> l)
        {
            
            
            swap(l);
            return *this;
        }
        ~list()
        {
            
            
            clear();
            delete _pHead;
            _pHead = nullptr;
        }
        ///
        // List Iterator
        iterator begin()
        {
            
            
            return iterator(_pHead->_pNext);
        }
        iterator end()
        {
            
            
            return iterator(_pHead);
        }
        const_iterator begin() const
        {
            
            
            return iterator(_pHead->_pNext);
        }
        const_iterator end() const
        {
            
            
            return iterator(_pHead);
        }
        ///
        // List Capacity
        size_t size()const
        {
            
            
            size_t size = 0;
            ListNode* p = _pHead->_pNext;
            while (p != _pHead) {
            
            
                size++;
                p = p->_pNext;
            }
            return size;
        }
        bool empty()const
        {
            
            
            return size() == 0;
        }
        
        // List Access
        T& front()
        {
            
            
            return _pHead;
        }
        const T& front()const
        {
            
            
            return _pHead;
        }
        T& back()
        {
            
            
            return _pHead->_pPre;
        }
        const T& back()const
        {
            
            
            return _pHead->_pPre;
        }
        
        // List Modify
        void push_back(const T& val) {
            
             insert(begin(), val); }
        void pop_back() {
            
             erase(--end()); }
        void push_front(const T& val) {
            
             insert(begin(), val); }
        void pop_front() {
            
             erase(begin()); }
        // 在pos位置前插入值为val的节点
        iterator insert(iterator pos, const T& val)
        {
            
            
            PNode newNode = new Node(val);
            PNode cur = pos._pNode;
            newNode->_pPre = cur->_pPre;
            newNode->_pNext = cur;
            newNode->_pPre->_pNext = newNode;
            cur->_pPre = newNode;
            return iterator(cur);
        }
        // 删除pos位置的节点，返回该节点的下一个位置
        iterator erase(iterator pos)
        {
            
                
            PNode Pnode = pos._pNode;
            PNode next = Pnode->_pNext;
            next->_pPre = Pnode->_pPre;
            Pnode->_pPre->_pNext = next;
            delete Pnode;
            Pnode = nullptr;
            return iterator(next);
        }
        void clear()
        {
            
            
            iterator it = begin();
            while (it != end()) {
            
            
                it = erase(it);
                ++it;
            }
        }
        void swap(list<T>& l)
        {
            
            
            swap(l._pHead);
        }
    private:
        void CreateHead()
        {
            
            
            _pHead = new Node;
            _pHead->_pNext = _pHead;
            _pHead->_pPre = _pHead;
        }
        PNode _pHead;
    };
};

```

# 三：Java实现

## （1）单链表

```java
package MyLinkedList;


public class MySingleList {
            
            
    //结点定义
    static class Node{
            
            
        public int val;
        public Node next;
        public Node(int val){
            
            
            this.val = val;
        }
    }
    public Node head;//头结点引用

    //构造函数
    public MySingleList(){
            
            
        this.head = null;
    }

    //检查位置是否合法
    private void checkIndex(int index){
            
            
        if(index < 0 || index > this.size()){
            
            
            throw new IndexNottLegelException("非法位置");
        }
    }

    //找寻前置结点（依靠位置）
    private Node findIndexSubOfOne(int index){
            
            
        Node cur = head;
        while(index-1 != 0){
            
            
            cur = cur.next;
            index--;
        }
        return cur;
    }

    //找寻前置结点（依靠关键字）
    private Node findIndexPreOfOne(int key){
            
            
        Node cur = this.head;
        while(cur.next != null){
            
            
            if(cur.next.val == key){
            
            
                return cur;
            }
            cur = cur.next;
        }
        return null;
    }

    //打印单链表
    public void display(){
            
            
        Node cur = this.head;
        while(cur != null){
            
            
            System.out.print(cur.val + " -> ");
            cur = cur.next;
        }
        System.out.println();
    }

    //返回单链表长度
    public int size(){
            
            
        int count = 0;
        Node cur = this.head;
        while(cur != null){
            
            
            count ++;
            cur = cur.next;
        }
        return count;
    }

    //查找该链表是否存在key这个结点
    public boolean contains(int key){
            
            
        Node cur = this.head;
        while(cur != null){
            
            
            if(cur.val == key){
            
            
                return true;
            }
            cur = cur.next;
        }
        return false;
    }

    //头插法建立单链表
    public void creatHead(int data){
            
            
        Node node = new Node(data);
        node.next = head;
        head = node;
    }

    //尾插法建立单链表
    public void creatTail(int data){
            
            
        Node node = new Node(data);
        if(head == null){
            
            
            head = node;
        }else{
            
            
            Node cur = head;
            while(cur.next != null){
            
            
                cur = cur.next;
            }
            cur.next = node;
        }



    }

    //任意位置插入
    public void addIndex(int index, int data){
            
            
        checkIndex(index);
        if(index == 0){
            
            
            //等价于头插
            this.creatHead(data);
            return;
        }
        if(index == this.size()){
            
            
            //等价于尾插
            this.creatTail(data);
            return;
        }
        //非头插、非尾插，且index合法
        Node node = new Node(data);
        Node cur = findIndexSubOfOne(index);
        node.next = cur.next;
        cur.next = node;
    }

    //删除第一次关键字为key结点
    public void remove(int key){
            
            
        //如果head.val == key
        if(this.head.val == key){
            
            
            this.head = this.head.next;
            return;
        }

        Node cur = findIndexPreOfOne(key);
        if(cur == null){
            
            return;}
        Node del = cur.next;
        cur.next = del.next;
    }

    //删除所有值为key的结点
    public void removeAll(int key){
            
            
        if(this.head == null) return;
        Node cur = this.head.next;
        Node prev = this.head;
        while(cur != null){
            
            
            if(cur.val == key){
            
            
                prev.next = cur.next;
                cur = cur.next;
            }else{
            
            
                prev = cur;
                cur =cur.next;
            }
        }
        //如果head.val == key
        if(this.head.val == key){
            
            
            this.head = this.head.next;
            return;
        }
    }

    //清空链表
    public void clear(){
            
            
        this.head = null;//连锁销毁
    }
}



```

## （2）双链表

 -    Java中双链表表对应其集合框架中的`LinkedList`，所以这里以实现`LinkedList`为例

```java
package myLinkedList;

public class MyLinkedList {
            
            
    //结点定义
    static class ListNode{
            
            
        public int val;
        public ListNode prev;
        public ListNode next;
        public ListNode(int val){
            
            
            this.val = val;
        }
    }

    //头结点和尾节点
    public ListNode head;
    public ListNode tail;

    //显示链表
    public void disPlay(){
            
            
        ListNode cur = this.head;
        while(cur != null){
            
            
            System.out.print(cur.val + "---");
            cur = cur.next;
        }
        System.out.println();
    }
    //返回某个索引对应的结点
    private ListNode findIndexNode(int index){
            
            
        ListNode cur = head;
        while(index != 0){
            
            
            cur = cur.next;
            index--;
        }
        return cur;
    }

    //判断某个结点是否在链表中
    public boolean contains(int key){
            
            
        ListNode cur = this.head;
        while(cur != null){
            
            
            if(cur.val == key){
            
            
                return true;
            }
            cur = cur.next;
        }
        return false;
    }

    //返回链表长度
    public int size(){
            
            
        int count = 0;
        ListNode cur = this.head;
        while(cur != null){
            
            
            count++;
            cur = cur.next;
        }
        return count;
    }

    //头插法
    public void createHead(int data){
            
            
        ListNode node = new ListNode(data);
        if(this.head == null){
            
            
            this.head = node;
            this.tail = node;
        }else{
            
            
            node.next = this.head;
            this.head.prev = node;
            head = node;
        }
    }

    //尾插法
    public void createTail(int data){
            
            
        ListNode node = new ListNode(data);
        if(this.head == null){
            
            
            this.head = node;
            this.tail = node;
        }else{
            
            
            this.tail.next = node;
            node.prev = this.tail;
            this.tail = node;
        }
    }

    //给定索引插入对应位置
    public void insertByIndex(int index, int data){
            
            
        ListNode node = new ListNode(data);
        if(index < 0 || index > this.size()){
            
            
            throw  new IndexNotLegalException("非法位置");
        }
        if(index == 0){
            
            
            createHead(data);
            return;
        }
        if(index == size()){
            
            
            createTail(data);
            return;
        }
        ListNode cur = findIndexNode(index);
        node.next = cur;
        cur.prev.next = node;
        node.prev = cur.prev;
        cur.prev = node;
    }

    //删除第一次出现关键字为key结点
    public void remove(int key){
            
            
        ListNode cur = this.head;
        while(cur != null){
            
            
            //如果找到就开始删除
            if(cur.val == key){
            
            
                //如果删除的第一个元素
                if(cur == this.head){
            
            
                    this.head = this.head.next;
                    //防止删除后没有元素导致空指针异常，所以这里要进行判断
                    if(head != null) {
            
            
                        this.head.prev = null;
                    }
                }else {
            
            
                    cur.prev.next = cur.next;
                    //如果删除的是最后一个结点
                    if(cur.next == null){
            
            
                        this.tail =this.tail.prev;
                    }else {
            
            
                        cur.next.prev = cur.prev;
                    }
                }
                return;
            }
            cur = cur.next;
        }
    }

    //删除所有关键字为key的结点
    public void removeAll(int key){
            
            
        ListNode cur = this.head;
        while(cur != null){
            
            
            //如果找到就开始删除
            if(cur.val == key){
            
            
                //如果删除的第一个元素
                if(cur == this.head){
            
            
                    this.head = this.head.next;
                    //防止删除后没有元素导致空指针异常，所以这里要进行判断
                    if(head != null) {
            
            
                        this.head.prev = null;
                    }
                }else {
            
            
                    cur.prev.next = cur.next;
                    //如果删除的是最后一个结点
                    if(cur.next == null){
            
            
                        this.tail =this.tail.prev;
                    }else {
            
            
                        cur.next.prev = cur.prev;
                    }
                }
            }
            cur = cur.next;
        }
    }

    //清空链表
    public void clear(){
            
            
        ListNode cur = this.head;
        while(cur != null){
            
            
            ListNode curNext = cur.next;
            cur.next = null;
            cur.prev = null;
            cur = curNext;
        }
        this.head = null;
        this.tail = null;
    }


}

```

# 四：Python实现

```python
class Node(object):

    def __init__(self, data, next=None):
        self.data = data
        self.next = next

    def __str__(self):
        return ('当前节点值为:%s' % self.data)


class LinkedList(object):

    def __init__(self, head=None):
        self.head = head

    def __len__(self):
        curr_node = self.head
        counter = 0
        while curr_node is not None:
            counter += 1
            curr_node = curr_node.next
        return counter

    # 在链表前面插入节点
    def insert_to_front(self, data):
        if data is None:
            return None
        # 原来的头结点 , 是新头节点的next
        node = Node(data, self.head)
        self.head = node
        return node

    # 从链表后面插入节点
    def append(self, data):
        if data is None:
            return None
        node = Node(data)
        if self.head is None:
            self.head = node
            return node
        # 从头节点开始寻找当前链表尾指针
        curr_node = self.head
        while curr_node.next is not None:
            curr_node = curr_node.next
        # 链表尾指针指向新插入的node
        curr_node.next = node
        return node

    def find(self, data):
        if data is None:
            return None
        curr_node = self.head
        while curr_node is not None:
            if curr_node.data == data:
                return curr_node
            curr_node = curr_node.next
        return None

    def delete(self, data):
        if data is None:
            return None
        if self.head is None:
            return
        #  当要删除的元素是头节点时
        if self.head.data == data:
            self.head = self.head.next
        prev_node = self.head
        curr_node = self.head.next
        while curr_node is not None:
            if curr_node.data == data:
                prev_node.next = curr_node.next
                return
            prev_node = curr_node
            curr_node = curr_node.next
        return None

    def print_list(self):
        if self.head is None:
            return
        curr_node = self.head
        while curr_node is not None:
            print(curr_node.data)
            curr_node = curr_node.next

    def get_all_data(self):
        if self.head is None:
            return
        curr_node = self.head
        data = []
        while curr_node is not None:
            data.append(curr_node.data)
            curr_node = curr_node.next
        return data


go = LinkedList()

while True:
    num = int(input('请输入1-6:'))
    if num == 1:
        data = input('请输入要插入的data:')
        go.insert_to_front(data)
    elif num == 2:
        data = input('请输入要添加的data:')
        go.append(data)
    elif num == 3:
        data = input('请输入要查找的data:')
        go.find(data)
    elif num == 4:
        data = input('请输入要删除的data:')
        go.delete(data)
    elif num == 5:
        print('查看长度')
        go.print_list()
    elif num == 6:
        print('打印')
        go.get_all_data()
    elif num == 0:
        break
    else:
        print('请输入正确的命令符')

```