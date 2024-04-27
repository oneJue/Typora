 

- [专栏目录首页：【专栏必读】王道考研408数据结构+计算机算法设计与分析万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121501138?spm=1001.2014.3001.5502)

### 文章目录

- [一：栈的实现](#_4)
- - [（1）C语言实现](#1C_6)
  - [（2）C++实现](#2C_73)
  - [（3）Java实现](#3Java_302)
  - [（4）Python实现](#4Python_376)
- [二：队列实现](#_407)
- - [（1）C语言实现](#1C_409)
  - - [A：链式队](#A_411)
    - [B：循环队列](#B_561)
  - [（2）C++实现](#2C_633)
  - - [A：链式队](#A_636)
    - [B：循环队列](#B_710)
  - [（3）Java实现](#3Java_776)
  - - [A：链式队](#A_779)
    - [B：循环队列](#B_836)

# 一：栈的实现

## （1）C语言实现

```c
void StackInit(ST* ps)
{
            
            
	assert(ps);
	ps->a = NULL;
	ps->top = ps->capacity = 0;
}
void StackDestory(ST* ps)
{
            
            
	assert(ps);
	free(ps->a);
	ps->a = NULL;
	ps->top = ps->capacity = 0;
}

void StackPush(ST* ps, STDataType x)
{
            
            
	assert(ps);
	if (ps->top == ps->capacity)
	{
            
            
		int newCapacity = ps->capacity == 0 ? 4 : (ps->capacity) * 2;
		STDataType* tmp = (STDataType*)realloc(ps->a, sizeof(STDataType)*newCapacity);

		if (tmp == NULL)
		{
            
            
			printf("realloc fail\n");
			exit(-1);
		}

		ps->a = tmp;
		ps->capacity = newCapacity;
	}

	ps->a[ps->top] = x;
	ps->top++;
}
void StackPop(ST* ps)
{
            
            
	assert(ps);
	assert(!StackEmpty(ps));

	ps->top--;
}

STDataType StackTop(ST* ps)
{
            
            
	assert(ps);
	assert(!StackEmpty(ps));

	return ps->a[ps->top - 1];
}
bool StackEmpty(ST* ps)
{
            
            
	assert(ps);
	return ps->top == 0;
}
int StackSize(ST* ps)
{
            
            
	assert(ps);
	return ps->top;
}

```

## （2）C++实现

```cpp
#include<iostream>
#include<cstdio> 
#include<cstdlib>
#define MaxSize 100
#define Ok 1
#define Error 0
#define OverFlow -2
using namespace std;
 
typedef int Status;
typedef int SElemType;
typedef struct{
            
            
	SElemType *top;		//栈顶指针 
	SElemType *base;	//栈底指针 
	int stacksize;		//栈可用最大容量 
}SqStack;
SqStack S,T;
 
//初始化 
Status InitStack(SqStack &S){
            
            
	S.base = (SElemType*)malloc(sizeof(SElemType) * MaxSize);
	if(!S.base)
		exit(OverFlow);			//存储分配失败 
	S.top = S.base;			//栈顶指针等于栈底指针
	S.stacksize = MaxSize;
	return Ok; 
} 
 
//判断栈是否为空
Status StackEmpty(SqStack S){
            
            
	if(S.top == S.base)
		return Ok;
	else
		return Error;
} 
 
//求顺序栈的长度
int StackLength(SqStack S){
            
            
	return S.top - S.base;
} 
 
//清空顺序栈
Status ClearStack(SqStack &S){
            
            
	if(S.base)
		S.top = S.base;
	return Ok;
}
 
//销毁顺序栈
Status DestroyStack(SqStack &S){
            
            
	if(S.base){
            
            
		delete S.base;		//释放内存 
		S.stacksize = 0;		//清空栈容量 
		S.base = S.top = NULL;		//指针指向空 
	}
	return Ok;
}
 
 //顺序栈的入栈
 Status Push(SqStack &S,SElemType e)  
{
            
              
    if(S.top - S.base >= S.stacksize)		//栈满，扩容 
        {
            
              
        S.base = (SElemType *)realloc(S.base,(S.stacksize * 2)*sizeof(SElemType));  
        if(!S.base)  
            exit(OverFlow);  
        S.top = S.base + S.stacksize;  
        S.stacksize *= 2;  
        }  
    *S.top++ = e;  
    return Ok;  
}
 
 //顺序栈的出栈 
 Status Pop(SqStack &S,SElemType &e){
            
            
 	if(S.top == S.base)		
 		return Error;
 	e = *--S.top;
 	return Ok;
 }
 
 //取栈顶元素
 Status GetTop(SqStack S,SElemType &e)  
{
            
              
    if(S.top==S.base)		//栈为空 
        return Error;  
    e = *(S.top-1);  
    return Ok;  
} 
 
//输出顺序栈中的元素
Status Print(SqStack S){
            
            
	SElemType *p = S.top;
	while(p != S.base)
		cout<< *--p << " ";
	cout<<endl;
	return Ok;
}
 
//进制转换
Status Num(SqStack &S,int e,int num){
            
            
	while(e){
            
            
		Push(S,e % num);
		e /= num;
	}
}
 
//输出转换后的数
Status NumStack(SqStack &S){
            
            
	SElemType *p = S.top;
	while(!StackEmpty(T)){
            
            
		int ans;
		Pop(T,ans);
		if(ans < 10)
			cout<< ans;
		else{
            
            
			char c;
			c = ans - 10 + 'A';
			cout<< c;
		}
	}
	cout<<endl;
} 
 
 
int main()
{
            
            
	int order,init = 0,des = 0,len,e;
	cout<< "*--------------栈----------------*"<<endl;
	cout<< "*---------1.初始化栈-------------*"<<endl;
	cout<< "*---------2.销毁栈---------------*"<<endl;
	cout<< "*---------3.清空栈---------------*"<<endl;
	cout<< "*---------4.栈判空---------------*"<<endl;
	cout<< "*---------5.求栈长度-------------*"<<endl;
	cout<< "*---------6.获取栈顶元素---------*"<<endl;
	cout<< "*---------7.插入一个元素---------*"<<endl;
	cout<< "*---------8.删除一个元素---------*"<<endl;
	cout<< "*---------9.输出所有元素---------*"<<endl;
	cout<< "*---------10.进制转换------------*"<<endl;
	do{
            
            
	cout<< "请输入指令：";
	cin>> order;
	if(order == 0 || order > 10)
		cout<< "没有该指令" <<endl;
	else if(!init && order > 1 && order != 10)
		cout<< "请先初始化栈" <<endl;
	else if(des && order > 1 && order != 10)
		cout<< "栈已销毁，请先初始化" <<endl;
	else
		switch(order)
		{
            
            
			case 1:
				InitStack(S);
				init=1;
				des=0;
				cout<< "初始化完成！" <<endl;
				break;
			case 2:
				DestroyStack(S);
				des = 1;
				cout<< "栈已销毁" <<endl;
				break;
			case 3:
				ClearStack(S);
				cout<< "栈已清空" <<endl;
				break;
			case 4:
				if(!StackEmpty(S))
					cout<< "栈非空" <<endl;
				else
					cout<< "栈为空" <<endl;
				break;
			case 5:
				len=StackLength(S);
				cout<< "栈长度为：" << len <<endl;
				break;
			case 6:	
				if(GetTop(S,e))
					cout<< "栈顶元素为：" << e <<endl; 
				else
					cout<< "空栈，无栈顶元素" <<endl; 
				break;
			case 7:
				cout<< "请输入插入元素：";
				cin>> e;
				Push(S,e);
				cout<< "入栈成功" <<endl;
				break;
			case 8:
				if(Pop(S,e))
					cout<< "栈顶元素" << e << "出栈成功" <<endl;
				else
					cout<< "栈为空" <<endl;
				break;
			case 9:
				cout<< "栈为：";
				Print(S);
				break;
			case 10:
				int num; 
				cout<< "输入一个十进制数：";
				cin>> e;
				cout<< "输入要转换的进制：";
				cin>> num;
				InitStack(T);
				cout<< "10进制数" << e << "转换为" << num <<"进制数后为：";
				if(e < 0)
				{
            
            
					cout<< '-';
					e = -e;
				}
				if(!e)
					cout<< 0;
				Num(T,e,num);
				NumStack(T);
				break;
			default:
				cout<< "程序已退出" <<endl;
				break;
		}
	}while(order>=0);
	return Ok;
}
```

## （3）Java实现

```java
package myStack;

import java.util.Arrays;

public class MyStack {
            
            
    public int[] elem;
    public int usedSize;//当前栈中有效元素个数，也即top指针
    public static final int DEFAULT_CAPACITY = 10;//默认容量

    public MyStack(){
            
            
        elem = new int[DEFAULT_CAPACITY];
    }

    //压栈
    public void push(int val){
            
            
        //如果栈满则扩容
        if(isFull()){
            
            
            elem = Arrays.copyOf(elem, 2*elem.length);
        }

        //放置元素，并移动指针
        elem[usedSize] = val;
        usedSize++;
    }

    //判满
    public boolean isFull(){
            
            
        if(elem.length == usedSize){
            
            
            return true;
        }
        return false;
    }

    //出栈
    public int pop(){
            
            
        //判空
        if(isEmpty()){
            
            
            throw new StackEmptyException("栈空！无法删除");
        }

        int retVal = elem[usedSize-1];;
        usedSize--;
        return retVal;
    }

    //判空
    public boolean isEmpty(){
            
            
        if(usedSize == 0){
            
            
            return true;
        }
        return false;
    }

    //查看栈顶元素
    public int peek(){
            
            
        //判空
        if(isEmpty()){
            
            
            throw new StackEmptyException("栈空！无法删除");
        }
        return elem[usedSize-1];
    }

    //返回栈顶有效元素个数
    public int size(){
            
            
        return usedSize;
    }
}

```

## （4）Python实现

```python
class Stack:
    def __init__(self):
        #我们定义空列表，实现栈的进入进出操作
        self.items = []
 
    def isEmpty(self):
        #判断是否为空栈
        return self.items == []
 
    def push(self,item):
        #向栈中加入元素，新的元素自动加入栈底
        self.items.append(item)
 
    def pop(self):
        #移出栈顶元素。
        return self.items.pop()
 
    def peek(self):
        #返回栈顶元素
        return self.items[len(self.items)-1]
 
    def size(self):
        #返回栈的长度
        return len(self.items)
```

# 二：队列实现

## （1）C语言实现

### A：链式队

```c
//队列的链表式实现
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <assert.h>     //现算表达式，如果为假，向stderr打印出错信息并通过abort终止程序

typedef int QDateType;  //定义别名

typedef struct QueueNode//结构体，队列中的元素：值和指向下个元素的指针
{
            
            
	QDateType val;
	struct QueueNode* next;
}QueueNode;

typedef	struct Queue //队列
{
            
            
	QueueNode* head; //头指针
	QueueNode* tail; //尾指针
}Queue;
//队列初始化
void QueueInti(Queue* pq)
{
            
            
	assert(pq); //非空
	pq->head = pq->tail = NULL;
}
//队列销毁
void QueueDestory(Queue* pq)
{
            
            
	assert(pq);
	QueueNode* cur = pq->head;
	while (cur)
	{
            
            
		QueueNode* next = cur->next;
		free(cur);
		cur = next;
	}
	pq->tail = pq->head = NULL;
}
//队尾插入元素
void QueuePush(Queue* pq, QDateType x)
{
            
            
	assert(pq);

	QueueNode* newNode = (QueueNode*)malloc(sizeof(QueueNode));
	if (NULL == newNode)
	{
            
            
		printf("malloc error\n");
		exit(-1);
	}
	newNode->val = x;
	newNode->next = NULL;

	if (pq->tail == NULL)
	{
            
            
		assert(pq->head == NULL);
		pq->head = pq->tail = newNode;
	}
	else
	{
            
            
		pq->tail->next = newNode;
		pq->tail = newNode;
	}

}
//队首删除元素
void QueuePop(Queue* pq)
{
            
            
	assert(pq);
	assert(pq->head && pq->tail);
	if (pq->head->next == NULL)
	{
            
            
		free(pq->head);
		pq->head = pq->tail = NULL;
	}
	else
	{
            
            
		QueueNode* next = pq->head->next;
		free(pq->head);
		pq->head = next;
	}
}
//队列是否为空
bool QueueEmpty(Queue* pq)
{
            
            
	assert(pq);

	return pq->head == NULL;
}
//取出队首元素-不删除
QDateType QueueFront(Queue* pq)
{
            
            
	assert(pq);
	assert(pq->head);

	return pq->head->val;
}
//取出队尾元素-不删除
QDateType QueueBack(Queue* pq)
{
            
            
	assert(pq);
	assert(pq->tail);

	return pq->tail->val;
}
//队列中元素数目
int QueueSize(Queue* pq)
{
            
            
	assert(pq);
	QueueNode* cur = pq->head;
	int count = 0;
	while (cur)
	{
            
            
		cur = cur->next;
		count++;
	}
	return count;
}


int main() 
{
            
            
    Queue q;
    QueueInti(&q);
    QueuePush(&q, 1);
    QueuePush(&q, 2);
    QueuePush(&q, 3);
    QueuePush(&q, 4);
    QueuePush(&q, 5);
    QueuePush(&q, 6);
	int n1 = QueueSize(&q);
	printf("初试元素数目：%d\n", n1);           //6
	//弹出并删除队首的3个元素：1 2 3，剩下4 5 6
    QueuePop(&q);
    QueuePop(&q);
    QueuePop(&q);
    int n2 = QueueSize(&q);
    printf("当前元素数目：%d\n", n2);           //3
    printf("当前队首元素%d\n", QueueFront(&q)); //4
    printf("当前队尾元素%d\n", QueueBack(&q));  //6
  
    system("pause");
    return 0;
}

```

### B：循环队列

```c
#include<stdio.h>
#include<stdlib.h>
#include<assert.h>
#include"cqueue.h"
void Init(PQueue pq) {
            
            
	assert(pq!=NULL);
	pq->base = (ELEM_TYPE*)malloc(MAX_SIZE * sizeof(ELEM_TYPE));
	assert(pq->base != NULL);
	pq->front = 0;
	pq->rear = 0;
}
bool Push(PQueue pq, ELEM_TYPE val) {
            
            
	assert(pq != NULL);
	if (IsFull(pq)) {
            
            
		return false;
	}
	pq->base[pq->rear] = val;
	pq->rear = (pq->rear + 1) % MAX_SIZE;
	return true;
}
bool Pop(PQueue pq) {
            
            
	assert(pq != NULL);
	if (IsEmpty(pq)) {
            
            
		return false;
	}
	pq->front=(pq->front+1)%MAX_SIZE;
	return true;
}
bool Top(PQueue pq, ELEM_TYPE* rtval) {
            
            
	assert(pq != NULL);
	if (IsEmpty(pq)) {
            
            
		return false;
	}
	*rtval = pq->base[pq->front];
	return true;
}
bool IsEmpty(PQueue pq) {
            
            
	assert(pq != NULL);
	return pq->front == pq->rear;
}
bool IsFull(PQueue pq) {
            
            
	assert(pq != NULL);
	return (pq->rear + 1) % MAX_SIZE == pq->front;
}
int Get_length(PQueue pq) {
            
            
	assert(pq != NULL);
	int count= (pq->rear - pq->front + MAX_SIZE) % MAX_SIZE;//前面是防止出现负数，后面是防止正数加多
	return count;
}
void Clear(PQueue pq) {
            
            
	assert(pq != NULL);
	pq->front = pq->rear = 0;
}
void Destory(PQueue pq) {
            
            
	assert(pq != NULL);
	free(pq->base);
	pq->front = pq->rear = 0;
}
void show(PQueue pq) {
            
            
	assert(pq != NULL);
	for (int i = pq->front;i!= pq->rear;i=(i+1)%MAX_SIZE) {
            
            
		printf("%5d",pq->base[i]);
	}
	printf("\n");
}
```

## （2）C++实现

### A：链式队

```cpp
template<typename T>
class queue {
            
            
private:
	struct queueNode {
            
            
		T data;
		queueNode* next;
		queueNode(T x) :data(x), next(nullptr) {
            
            };
	};
	queueNode* Front;//队列的队首指针
	queueNode* rear;//对尾指针
	int count;
public:
	queue();
	bool empty();
	int size();
	void push(const T &node);
	void pop();
	T front();

};

template<typename T>
queue<T>::queue() :Front(NULL), rear(NULL), count(0) {
            
            };//构造函数的初始化：必须初始化指针，否则引发错误

template<typename T>
bool queue<T>::empty() {
            
            //首指针为空则队列空
	if (Front == NULL)
		return true;
	else
		return false;
}

template<typename T>
int queue<T>::size() {
            
            
	return count;
}

template<typename T>
void queue<T>::push(const T &node) {
            
            
	if (Front == NULL)
		Front = rear = new queueNode(node);
	else if (node == NULL)
		return;
	else {
            
            //通过尾指针实现插入
		queueNode* newNode = new queueNode(node);
		rear->next = newNode;
		rear = newNode;
	}
	count++;
}

template<typename T>
void queue<T>::pop() {
            
            
	if (Front ==NULL)
		cout << "queue is empty";
	else {
            
            //通过首指针实现删除
		queueNode* node = Front;
		Front = Front->next;
		delete node;
		count--;
	}
}

template<typename T>
T queue<T>::front() {
            
            
	return Front->data;//注意返回的是首指针所指节点的数据域，而不是首节点。
}

```

### B：循环队列

```cpp
#include<iostream>
#define MaxSize 10
 
using namespace std;
 
typedef struct SqQueue{
            
            
	int data[MaxSize];
	int front,rear;
};
 
//初始化 
void InitSqQueue(SqQueue &Q){
            
            
	Q.front = 0;
	Q.rear = 0;
}
 
//入队
bool EnQueue(SqQueue &Q,int x){
            
            
	if((Q.rear+1)%MaxSize==Q.front)  //判断是否为满 
		return false;
	
	Q.data[Q.rear] = x;
	Q.rear = (Q.rear+1) % MaxSize;
	return true;
} 
 
//出队
bool DeQueue(SqQueue &Q,int &x){
            
            
	if(Q.front == Q.rear)
		return false;
		
	x = Q.data[Q.front];
	Q.front = (Q.front+1)%MaxSize;
	return true;
} 
 
//查询队头
bool GetHead(SqQueue Q,int &x){
            
            
	if(Q.front == Q.rear)
		return false;
		
	x = Q.data[Q.front];
	return true;
} 
 
int main(){
            
            
	SqQueue Q;
	InitSqQueue(Q);
	
	EnQueue(Q,2);
	EnQueue(Q,3);
	int x;
	DeQueue(Q,x);
	cout << x << endl;
	int y = 2;
	GetHead(Q,y);
	cout << y << endl;
	
	return 0;
}
```

## （3）Java实现

### A：链式队

```java
package myQueue;

public class MyQueue {
            
            
    //结点定义
    static class Node{
            
            
        public int val;
        public Node next;
        public Node(int val){
            
            
            this.val = val;
        }
    }

    //队列头结点和尾节点
    public Node front;
    public Node rear;

    //入队
    public void offer(int val){
            
            
        Node node = new Node(val);
        if(this.front == null){
            
            
            this.front = node;
            this.rear = node;
        }else{
            
            
            this.rear.next = node;
            this.rear = node;
        }
    }

    //出队
    public int poll(){
            
            
        if(this.front == null){
            
            
            return -1;
        }
        int retVal = this.front.val;
        if(this.front.next == null){
            
            
            this.front = this.rear =null;
        }else {
            
            
            this.front = this.front.next;
        }
        return retVal;
    }

    //查看队头元素
    public int peek(){
            
            
        if(this.front == null){
            
            
            return -1;
        }
        return this.front.val;
    }
}

```

### B：循环队列

```java
package myCircularQueue;

public class MyCircularQueue {
            
            
    public int[] elem;
    public int usedSize;
    public int front;//队头
    public int rear;//队尾

    public MyCircularQueue(int k){
            
            
        this.elem = new int[k];
    }

    //入队
    public boolean enQueue(int value){
            
            
        if(this.isFull()){
            
            
            return false;
        }
        this.elem[rear] = value;
        this.rear = (this.rear+1) % (this.elem.length);
        return true;
    }

    //出队
    public boolean deQueue(){
            
            
        if(this.isEmpty()){
            
            
            return false;
        }
        this.front = (this.front+1)%(this.elem.length);
        return true;
    }

    //获取队头元素
    public int getFront(){
            
            
        if(isEmpty()) {
            
            
            return -1;
        }
        return this.elem[this.front];
    }

    //获取队尾元素
    public int getRear(){
            
            
        if(isEmpty()) {
            
            
            return -1;
        }
        int index = this.rear-1;
        if(this.rear == 0) {
            
            
           index = this.elem.length - 1;
        }
        return this.elem[index];
    }

    //判空
    public boolean isEmpty(){
            
            
        return this.rear == this.front;
    }
    //判满
    public boolean isFull(){
            
            
        return (this.rear+1)%(this.elem.length) == this.front;
    }
}


```