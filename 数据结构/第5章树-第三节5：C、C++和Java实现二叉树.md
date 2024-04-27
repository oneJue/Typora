 

- [专栏目录首页：【专栏必读】王道考研408数据结构+计算机算法设计与分析万字笔记、题目题型总结、注意事项、目录导航和思维导图](https://zhangxing-tech.blog.csdn.net/article/details/121501138?spm=1001.2014.3001.5502)

### 文章目录

- [一：C语言实现](#C_19)
- [二：C++实现](#C_235)
- [三：Java实现](#Java_904)

**注意：本文实现设计如下二叉树基本操作**

- 前序、中序和后序遍历（递归）
- 前序、中序和后序遍历（非递归）
- 层次遍历
- 二叉树构建
- 获取树结点数目
- 获取树叶子结点个数
- 求第 k k k层结点个数
- 求二叉树高度
- 判断某个元素是否在二叉树内
- 

# 一：C语言实现

```c
#define  _CRT_SECURE_NO_WARNINGS 1
#include"Binary.h"
#include"Queue.h"
BTNode* BuyNode(BTDataType x)
{
            
            
	BTNode* newnode = (BTNode*)malloc(sizeof(BTNode));
	if (newnode == NULL)
	{
            
            
		printf("malloc fail\n");
		exit(-1);
	}
	newnode->left = NULL;
	newnode->right = NULL;
	newnode->data = x;
	return newnode;
}
BTNode* CreatBinaryTree()
{
            
            
	BTNode* nodeA = BuyNode('A');
	BTNode* nodeB = BuyNode('B');
	BTNode* nodeC = BuyNode('C');
	BTNode* nodeD = BuyNode('D');
	BTNode* nodeE = BuyNode('E');
	BTNode* nodeF = BuyNode('F');
	BTNode* nodeG = BuyNode('G');
	nodeA->left = nodeB;
	nodeA->right = nodeC;
	nodeB->left = nodeD;
	nodeC->left = nodeE;
	nodeC->right = nodeF;
	nodeF->left = nodeG;
	return nodeA;
}
void PrevOrder(BTNode* root)
{
            
            
	if (root == NULL) //这里不用assert,否则遇到空时，就过不去了
	{
            
            
		printf("NULL ");
		return;
	}
	printf("%c ", root->data);
	PrevOrder(root->left);
	PrevOrder(root->right);
}
void InOrder(BTNode* root)
{
            
            
	if (root == NULL)
	{
            
            
		printf("NULL ");
		return;
	}
	InOrder(root->left);
	printf("%c ", root->data);
	InOrder(root->right);
}
void PostOrder(BTNode* root)
{
            
            
	if (root == NULL)
	{
            
            
		printf("NULL ");
		return;
	}
	PostOrder(root->left);
	PostOrder(root->right);
	printf("%c ", root->data);
}
int BinaryTreeSize(BTNode* root)
{
            
            
	//if (root == NULL)
	//{
            
            
	//	return 0;
	//}
	//return BinaryTreeSize(root->left) + BinaryTreeSize(root->right) + 1;
	return root == NULL ? 0 : BinaryTreeSize(root->left) + BinaryTreeSize(root->right) + 1;
}
int BinaryTreeLeafSize(BTNode* root)
{
            
            
	if (root == NULL) //这个条件不能忘
	{
            
            
		return 0;
	}
	if (root->left == NULL && root->right == NULL)
	{
            
            
		return 1;
	}
	return BinaryTreeLeafSize(root->left) + BinaryTreeLeafSize(root->right);
}
//二叉树第K层结点个数
int BinaryTreeLevelKSize(BTNode* root, int k)
{
            
            
	assert(k >= 1);
	if (root == NULL)
	{
            
            
		return 0;
	}
	if (k == 1)
	{
            
            
		return 1;
	}
	int left = BinaryTreeLevelKSize(root->left, k - 1);
	int right = BinaryTreeLevelKSize(root->right, k - 1);
	return left + right;
	//return BinaryTreeLevelKSize(root->left, k - 1) + BinaryTreeLevelKSize(root->right, k - 1);
}
//二叉树的深度/高度
int BinaryTreeDepth(BTNode* root)
{
            
            
	if (root == NULL)
	{
            
            
		return 0;
	}
	int leftDepth = BinaryTreeDepth(root->left);
	int rightDepth = BinaryTreeDepth(root->right);
	return leftDepth > rightDepth ? leftDepth + 1 : rightDepth + 1;
}
BTNode* BinaryTreeFind(BTNode* root, BTDataType x)
{
            
            
	if (root == NULL)
	{
            
            
		return NULL;
	}
	if (root->data == x)
	{
            
            
		return root;
	}
	BinaryTreeFind(root->left, x);
	BinaryTreeFind(root->right, x);
	return NULL;
}
//层序遍历
void BinaryTreeLevelOrder(BTNode* root)
{
            
            
	if (root == NULL)
	{
            
            
		return;
	}
	Queue q;
	QueueInit(&q);
	QueuePush(&q, root);
	while (!QueueEmpty(&q))
	{
            
            
		BTNode* front = QueueHead(&q);
		QueuePop(&q);//Pop掉的是存放树节点的地址,而不是把树节点Pop掉了
		printf("%c ", front->data);
		if (front->left)
		{
            
            
			QueuePush(&q, front->left);
		}
		if (front->right)
		{
            
            
			QueuePush(&q, front->right);
		}
	}
	printf("\n");
	QueueDestroy(&q);
}
//判断是不是完全二叉树
bool BinaryTreeComplete(BTNode* root)
{
            
            
	Queue q;
	QueueInit(&q);
	QueuePush(&q, root);
	while (!QueueEmpty(&q))
	{
            
            
		BTNode* front = QueueHead(&q);//front是存放树节点的指针，树节点为空，front不一定为空
		QueuePop(&q);
		if (front == NULL)
		{
            
            
			break;
		}
		else
		{
            
            
			//NULL也放入
			QueuePush(&q, front->left);
			QueuePush(&q, front->right);
		}
	}
	while (!QueueEmpty(&q))
	{
            
            
		BTNode* front = QueueHead(&q);
		QueuePop(&q);
		if (front)
		{
            
            
			QueueDestroy(&q);
			return false;
		}
	}
	QueueDestroy(&q);
	return true;
}
void BinaryTreeDestroy(BTNode* root)
{
            
            
	if (root == NULL)
	{
            
            
		return;
	}
	//后序遍历
	BinaryTreeDestroy(root->left);
	BinaryTreeDestroy(root->right);
	free(root);
	root = NULL;
 
	//前序遍历
	/*BTNode* lf = root->left;
	BTNode* rt = root->right;
	free(root);
	root = NULL;
	BinaryTreeDestroy(lf);
	BinaryTreeDestroy(rt);*/
}
```

# 二：C++实现

```cpp
#include <iostream>
#include<stack>
#include<queue>
using namespace std;


//二叉树结点
template<class T>
struct BinTreeNode
{
            
            
    T data;
    BinTreeNode* leftchild, * rightchild;
    BinTreeNode() : leftchild(0), rightchild(0) {
            
            }
    BinTreeNode(T x, BinTreeNode* l = 0, BinTreeNode * r = 0) : data(x), leftchild(l), rightchild(r) {
            
            }
};

//后序非递归遍历用到的结构
template<class T>
struct stkNode {
            
            
    BinTreeNode<T>* ptr;
    int tag; //0代表左，1代表右
    stkNode(BinTreeNode<T>* N = 0) : ptr(N), tag(0) {
            
            }
};

template<class T>
class BinaryTree {
            
            
public:
    BinaryTree() : root(0) {
            
            }
    ~BinaryTree() {
            
            }

    //建树
    void Create();
    void Create(BinTreeNode<T>*& BT, T endValue);
    //删除树
    void destroy();
    void destroy(BinTreeNode<T>* subTree);

    //前序,中序,后序递归遍历算法
    void PreOrder(void(*visti)(BinTreeNode<T>* p)) const;
    void InOrder(void(*visit)(BinTreeNode<T>* p)) const;
    void PostOrder(void (*visit)(BinTreeNode<T>* p)) const;
    //重载前序，中序，后序递归排序算法
    void PreOrder(BinTreeNode<T>* subTree, void (*visit)(BinTreeNode<T>* p)) const;
    void InOrder(BinTreeNode<T>* subTree, void (*visit)(BinTreeNode<T>* p)) const;
    void PostOrder(BinTreeNode<T>* subTree, void (*visit)(BinTreeNode<T>* p)) const;
    //前序,中序，后序非递归算法
    void PreOrderNonRecursion(void(*visit)(BinTreeNode<T>* p));
    void InOrderNonRecursion(void(*visit)(BinTreeNode<T>* p));
    void PostOrderNonRecursion(void(*visit)(BinTreeNode<T>* p));

    //层次遍历
    void LevelOrder(void (*visit)(BinTreeNode<T>* p));

    //求树高
    int Height() const;
    int Height(BinTreeNode<T>* subTree) const;
    int HeightNoRecursion();

    //求节点个数
    int Size() const;                       //从根开始
    int Size(BinTreeNode<T>* subTree) const;      //前序遍历顺序
    int SizeNonRecursionByLevelOrder();     //非递归方法,层次遍历顺序
    int SizeNonRecursionByPreOrder();       //非递归方法,前序遍历顺序

    //求叶子结点总数
    int LeavesSize() const;   //从根开始
    int LeavesSize(BinTreeNode<T>* t) const;   //前序遍历顺序
    int LeavesSizeNonRecursionByLevelOrder();
    int LeavesSizeNonRecursionByPreOrder();

    //广义表输出二叉树
    void PrintBinTree();
    void PrintBinTree(BinTreeNode<T>* BT);

private:
    BinTreeNode<T>* root;
};


//从根开始建立树
template<class T>
void BinaryTree<T>::Create()
{
            
            
    cout << "请输入结束的标志" << endl;
    T endValue;
    cin >> endValue;
    cout << "请输入二叉树结点的值 (0结束)" << endl;
    Create(root, endValue);
}


//前序顺序建立二叉树
template<class T>
void BinaryTree<T>::Create(BinTreeNode<T>*& BT, T endValue)
{
            
            

    T x;
    cin >> x;
    if (x == endValue)
    {
            
            
        BT = 0;
    }
    else
    {
            
            
        BT = new BinTreeNode<T>(x);
        Create(BT->leftchild, endValue);
        Create(BT->rightchild, endValue);
    }
}


//从根开始删除
template<class T>
void BinaryTree<T>::destroy()
{
            
            
    destroy(root);
}

//删除以subtree为根的树
template<class T>
void BinaryTree<T>::destroy(BinTreeNode<T>* subTree)
{
            
            
    if (subTree != 0)
    {
            
            
        destroy(subTree->leftchild);
        destroy(subTree->rightchild);
        delete subTree;
    }
    root = 0;  //不然删完遍历会报错
}

///    其他函数     //

//前序,中序,后序递归遍历算法
template<class T>
void BinaryTree<T>::PreOrder(void (*visit)(BinTreeNode<T>* p)) const
{
            
            
    if (root == 0)
    {
            
            
        cout << "该树为空" << endl;
        return;
    }
    PreOrder(root, visit);
}


template<class T>
void BinaryTree<T>::InOrder(void (*visit)(BinTreeNode<T>* p)) const
{
            
            
    if (root == 0)
    {
            
            
        cout << "该树为空" << endl;
        return;
    }
    InOrder(root, visit);
}


template<class T>
void BinaryTree<T>::PostOrder(void (*visit)(BinTreeNode<T>* p)) const
{
            
            
    if (root == 0)
    {
            
            
        cout << "该树为空" << endl;
        return;
    }
    PostOrder(root, visit);
}


//前序，中序，后序遍历重载

template<class T>
void BinaryTree<T>::PreOrder(BinTreeNode<T>* subTree, void (*visit)(BinTreeNode<T>* p)) const
{
            
            
    if (subTree != 0)
    {
            
            
        visit(subTree);
        PreOrder(subTree->leftchild, visit);
        PreOrder(subTree->rightchild, visit);
    }
}


template<class T>
void BinaryTree<T>::InOrder(BinTreeNode<T>* subTree, void (*visit)(BinTreeNode<T>* p)) const
{
            
            
    if (subTree != 0)
    {
            
            
        InOrder(subTree->leftchild, visit);
        visit(subTree);
        InOrder(subTree->rightchild, visit);
    }
}


template<class T>
void BinaryTree<T>::PostOrder(BinTreeNode<T>* subTree, void(*visit)(BinTreeNode<T>* p)) const
{
            
            
    if (subTree != 0)
    {
            
            
        PostOrder(subTree->leftchild, visit);
        PostOrder(subTree->rightchild, visit);
        visit(subTree);
    }
}


//非递归前序，中序，后序遍历算法
template<class T>
void BinaryTree<T>::PreOrderNonRecursion(void(*visit)(BinTreeNode<T>* p))
{
            
            
    if (root == 0)
    {
            
            
        cout << "该树为空" << endl;
        return;
    }
    stack<BinTreeNode<T>*> S;
    BinTreeNode<T>* p = root;
    do
    {
            
            
        //一直往左,访问并记录结点以便返回
        while (p != 0)
        {
            
            
            S.push(p);
            visit(p);
            p = p->leftchild;
        }
        if (!S.empty())
        {
            
            
            p = S.top();
            S.pop();
            p = p->rightchild;
        }
    } while (p != 0 || !S.empty());
    cout << endl;
}


template<class T>
void BinaryTree<T>::InOrderNonRecursion(void(*visit)(BinTreeNode<T>* p))
{
            
            
    if (root == 0)
    {
            
            
        cout << "该树为空" << endl;
        return;
    }
    stack<BinTreeNode<T>*> S;
    BinTreeNode<T>* p = root;
    do {
            
            
        //一直往左,访问并记录结点以便返回
        while (p != 0)
        {
            
            
            S.push(p);
            p = p->leftchild;
        }
        if (!S.empty())
        {
            
            
            p = S.top();
            S.pop();
            visit(p);
            p = p->rightchild;
        }
    } while (p != 0 || !S.empty());
    cout << endl;
}


template<class T>
void BinaryTree<T>::PostOrderNonRecursion(void(*visit)(BinTreeNode<T>* p))
{
            
            
    if (root == 0)
    {
            
            
        cout << "该树为空" << endl;
        return;
    }
    stack<stkNode<T>> S;
    stkNode<T> w;
    BinTreeNode<T>* p = root;
    do
    {
            
            
        while (p != 0)
        {
            
            
            w.ptr = p;
            w.tag = 0;
            S.push(w);
            p = p->leftchild;
        }
        bool continuing = true;
        while (continuing && !S.empty())
        {
            
            
            w = S.top();
            S.pop();
            p = w.ptr;
            switch (w.tag)
            {
            
            
                case 0:
                    w.tag = 1;
                    S.push(w);
                    continuing = false; //停止这个while循环,让它去找最左的左子树0
                    p = p->rightchild;
                    break;
                case 1:
                    visit(p);
                    break;
            }
        }
    } while (!S.empty());
    cout << endl;
}


//层次遍历
template<class T>
void BinaryTree<T>::LevelOrder(void(*visit)(BinTreeNode<T>* p))
{
            
            
    queue<BinTreeNode<T>*> Q;
    BinTreeNode<T>* p = root;
    Q.push(p);//指向根节点的指针入队
    while (!Q.empty())
    {
            
            
        p = Q.front();
        Q.pop();
        visit(p);
        if (p->leftchild != 0)
        {
            
            
            Q.push(p->leftchild);
        }

        if (p->rightchild != 0)
        {
            
            
            Q.push(p->rightchild);
        }
    }
}


//求树高
template<class T>
int BinaryTree<T>::Height() const
{
            
            
    return Height(root);
}


template<class T>
int BinaryTree<T>::Height(BinTreeNode<T>* subTree) const
{
            
            
    if (subTree == 0)
    {
            
            
        return 0;
    }
    int leftchildHeight = Height(subTree->leftchild);
    int rightchildHeight = Height(subTree->rightchild);
    return (leftchildHeight > rightchildHeight) ? leftchildHeight + 1 : rightchildHeight + 1;
}


//非递归求树高,层次遍历顺序
template<class T>
int BinaryTree<T>::HeightNoRecursion()
{
            
            
    int depth = 0;
    BinTreeNode<T>* p = root;
    queue<BinTreeNode<T>*> q;
    q.push(p);  //根指针入队
    while (!q.empty())
    {
            
            
        depth++;
        int width = q.size();
        for (int i = 0; i < width; i++)
        {
            
            
            p = q.front();
            q.pop();
            if (p->leftchild != 0)
                q.push(p->leftchild);
            if (p->rightchild != 0)
                q.push(p->rightchild);
        }
    }
    return depth;
}


//求结点个数
template<class T>
int BinaryTree<T>::Size() const
{
            
            
    return Size(root);
}


template<class T>
int BinaryTree<T>::Size(BinTreeNode<T>* subTree) const
{
            
            
    if (subTree == 0)
        return 0;
    return 1 + Size(subTree->leftchild) + Size(subTree->rightchild);
}


//非递归求节点个数,层次遍历顺序
template<class T>
int BinaryTree<T>::SizeNonRecursionByLevelOrder()
{
            
            
    //如果树根为空，则节点数为0
    if (root == 0)
        return 0;
    //如果不空，则可以执行下面的操作
    int size = 1;  //至少有根节点
    BinTreeNode<T>* p = root;
    queue<BinTreeNode<T>*> Q;
    Q.push(p);  根指针入队
    while (!Q.empty())
    {
            
            
        int width = Q.size();  //获取当前层次宽度,也就是知道下一层次的所有节点个数
        for (int i = 0; i < width; i++)
        {
            
            
            p = Q.front();     //获取队顶元素
            Q.pop();          //弹出队顶元素
            if (p->leftchild != 0)
            {
            
            
                size++;
                Q.push(p->leftchild);
            }
            if (p->rightchild != 0)
            {
            
            
                size++;
                Q.push(p->rightchild);
            }
        }
    }
    return size;
}


template<class T>
int BinaryTree<T>::SizeNonRecursionByPreOrder()
{
            
            
    if (root == 0)
        return 0;
    stack<BinTreeNode<T>*> S;
    BinTreeNode<T>* p = root;
    int size = 0;  //结点计数器
    do
    {
            
            
        while (p != 0)
        {
            
            
            S.push(p);
            size++;
            p = p->leftchild;
        }
        if (!S.empty())
        {
            
            
            p = S.top();
            S.pop();
            p = p->rightchild;
        }
    } while (p != 0 || !S.empty());
    return size;
}



//求叶子结点数目
template<class T>
int BinaryTree<T>::LeavesSize() const
{
            
            
    return LeavesSize(root);
}



template<class T>
int BinaryTree<T>::LeavesSize(BinTreeNode<T>* t) const
{
            
            
    if (t == 0)
        return 0;
    if (t->leftchild == 0 && t->rightchild == 0)
        return 1;
    return LeavesSize(t->leftchild) + LeavesSize(t->rightchild);
}


template<class T>
int BinaryTree<T>::LeavesSizeNonRecursionByLevelOrder()
{
            
            
    //如果树根为空，则节点数为0
    if (root == 0)
        return 0;
    //初始时叶子结点计数器置0
    int size = 0;
    BinTreeNode<T>* p = root;
    queue<BinTreeNode<T>*> Q;
    Q.push(p);   //根指针入队
    while (!Q.empty())
    {
            
            
        int width = Q.size();       //获取当前层次宽度,也就是知道下一层次的所有节点个数
        for (int i = 0; i < width; i++)
        {
            
            
            p = Q.front();       //获取队顶元素
            Q.pop();			//弹出队顶元素
            if (p->leftchild == 0 && p->rightchild == 0)
            {
            
            
                size++;
            }
            else
            {
            
            
                if (p->leftchild != 0)
                    Q.push(p->leftchild);
                if (p->rightchild != 0)
                    Q.push(p->rightchild);
            }
        }
    }
    return size;
}


template<class T>
int BinaryTree<T>::LeavesSizeNonRecursionByPreOrder()
{
            
            
    if (root == 0)
        return 0;
    stack<BinTreeNode<T>*>S;
    BinTreeNode<T>* p = root;
    int size = 0;
    do {
            
            
        while (p != 0)
        {
            
            
            S.push(p);
            //叶子结点呢
            if (p->leftchild == 0 && p->rightchild == 0)
            {
            
            
                size++;
                break;
            }
            p = p->leftchild;
        }
        if (!S.empty())
        {
            
            
            p = S.top();
            S.pop();
            p = p->rightchild;
        }
    } while (p != 0 || !S.empty());
    return size;
}


//广义表输出二叉树
template<class T>
void BinaryTree<T>::PrintBinTree()
{
            
            
    PrintBinTree(root);
}


template<class T>
void BinaryTree<T>::PrintBinTree(BinTreeNode<T>* BT)
{
            
            
    if (BT != 0)
    {
            
            
        cout << BT->data;
        if (BT->leftchild != 0 || BT->rightchild != 0)
        {
            
            
            cout << "(";
            PrintBinTree(BT->leftchild);  // 递归输出左子树
            if (BT->rightchild != 0)
            {
            
            
                cout << ",";
                PrintBinTree(BT->rightchild); // 递归输出右子树
            }
            cout << ")";
        }
    }
}


//输出结点data值的函数
template<class T>
void PrintData(BinTreeNode<T>* p)
{
            
            
    cout << p->data << " ";
}


int main()
{
            
            
    BinaryTree<char> a;
    a.Create();

    cout << endl << endl;
    cout << "前序递归遍历" << endl;
    a.PreOrder(PrintData);

    cout << endl << endl;
    cout << "中序递归遍历" << endl;
    a.InOrder(PrintData);

    cout << endl << endl;
    cout << "后序递归遍历" << endl;
    a.PostOrder(PrintData);

    cout << endl << endl << endl;
    cout << "前序非递归遍历" << endl;
    a.PreOrderNonRecursion(PrintData);

    cout << endl;
    cout << "中序非递归遍历" << endl;
    a.InOrderNonRecursion(PrintData);

    cout << endl;
    cout << "后序非递归遍历" << endl;
    a.PostOrderNonRecursion(PrintData);

    cout << endl;
    cout << "层次遍历" << endl;
    a.LevelOrder(PrintData);

    cout << endl;
    cout << "递归求树的高度" << endl;
    cout << "height: " << a.Height();

    cout << endl << endl;
    cout << "非递归求树的高度" << endl;
    cout << "height: " << a.HeightNoRecursion();

    cout << endl << endl << endl << endl;
    cout << "递归求结点个数" << endl;
    cout << "size: " << a.Size();

    cout << endl << endl;
    cout << "层次顺序非递归求结点个数" << endl;
    cout << "size: " << a.SizeNonRecursionByLevelOrder();

    cout << endl << endl;
    cout << "前序顺序非递归求结点个数" << endl;
    cout << "size: " << a.SizeNonRecursionByPreOrder();


    cout << endl << endl;
    cout << "***************************************************" << endl;
    cout << endl << endl;
    cout << "递归求叶子结点总数" << endl;
    cout << "leavesSize: " << a.LeavesSize() << endl;

    cout << endl << endl;
    cout << "层次顺序非递归方法求叶子结点总数" << endl;
    cout << "leavesSize: " << a.LeavesSizeNonRecursionByLevelOrder() << endl;

    cout << endl << endl;
    cout << "前序顺序非递归方法求叶子结点总数" << endl;
    cout << "leavesSize: " << a.LeavesSizeNonRecursionByPreOrder() << endl;


    cout << endl << endl;
    cout << "广义表输出" << endl;
    a.PrintBinTree();

    cout << endl;
}

```

# 三：Java实现

```java
package myBinaryTree;

import sun.reflect.generics.tree.Tree;

import java.time.temporal.Temporal;
import java.util.*;

public class MyBinaryTree {
            
            
    static class TreeNode {
            
            
        public char val;
        public TreeNode left;
        public TreeNode right;

        public TreeNode(char val) {
            
            
            this.val = val;
        }
    }

    //根节点
    public TreeNode root;

    //根据字符串序列创建二叉树（字符串序列表示某二叉树的先序遍历序列，例如ABC##DE#G##F###,其中
    //#表示空格，代表空树）
    public TreeNode createTree(String str){
            
            
        TreeNode root = null;
        if(str.charAt(i) != '#'){
            
            
            root = new TreeNode(str.charAt(i));
            i++;
            root.left = createTree(str);
            root.right = createTree(str);
        }else{
            
            
            i++;
        }
        return root;
    }

    //前序遍历-递归实现
    public void preOrder(TreeNode root) {
            
            
        if (root == null) {
            
            
            return;
        }
        System.out.println(root.val + "；");
        preOrder(root.left);
        preOrder(root.right);
    }

    //前序遍历-非递归实现
    public void preOrderNoRecursive(TreeNode root) {
            
            
        Stack<TreeNode> stack = new Stack<>();//临时栈
        TreeNode cur = root;
        //遇到结点就访问然后入栈，并转向左节点
        while (cur != null || !stack.isEmpty()) {
            
            
            while (cur != null) {
            
            
                System.out.println(root.val + "：");
                stack.push(cur);
                cur = cur.left;
            }
            //循环停止时，走到了最左面的结点
            TreeNode tmp = stack.peek();
            stack.pop();
            //如果它没有右节点，那么就继续出下一个结点
            //如果它有右节点，那么右节点进入循环，然后入栈再重复
            cur = tmp.right;
        }
    }

    //中序遍历
    public void inOrder(TreeNode root) {
            
            
        if (root == null) {
            
            
            return;
        }
        preOrder(root.left);
        System.out.println(root.val + "；");
        preOrder(root.right);
    }

    //中序遍历-非递归实现
    public void inOrderNoRecursive(TreeNode root) {
            
            
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()) {
            
            
            while (cur != null) {
            
            
                stack.push(cur);
                cur = cur.left;
            }
            TreeNode tmp = stack.peek();
            stack.pop();
            System.out.println(tmp.val + "：");

            cur = tmp.right;
        }
    }

    //后序遍历
    public void postOrder(TreeNode root) {
            
            
        if (root == null) {
            
            
            return;
        }
        preOrder(root.left);
        preOrder(root.right);
        System.out.println(root.val + "；");
    }

    //后序遍历-非递归实现
    //将前序遍历改为“根右左”，然后再反转结果，就是后序遍历
    public void postOrderNoRecursive(TreeNode root) {
            
            
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()) {
            
            
            while (cur != null) {
            
            
                System.out.println(root.val + "：");
                stack.push(cur);
                cur = cur.right;
            }
            TreeNode tmp = stack.peek();
            stack.pop();
            cur = tmp.left;

            // Collections.reverse(ret);

        }
    }

    //获取树中结点的个数
    public int treeSize(TreeNode root) {
            
            
        if(root == null) return 0;
        return treeSize(root.left) + treeSize(root.right) + 1;
    }

    //获取树中叶子结点个数
    public int treeLeafNodeSize(TreeNode root){
            
            
        if(root == null) return 0;
        if(root.left == null && root.right == null) return 1;
        return treeLeafNodeSize(root.left) + treeLeafNodeSize(root.right);
    }

    //求第k层结点个数
    public int theNumOfk(TreeNode root, int k){
            
            
        if(root == null) return 0;
        if(k == 1) return 1;
        return theNumOfk(root.left, k-1) + theNumOfk(root.right, k-1);
    }

    //获取二叉树高度
    public int treeHeight(TreeNode root){
            
            
        if(root == null) return 0;
        if(root.left == null && root.right == null) return 1;
        int leftHeight = treeHeight(root.left);
        int rightHeight = treeHeight(root.right);
        return leftHeight > rightHeight ? leftHeight + 1 :rightHeight + 1;
     }

     //判断某个元素是否存在
    public TreeNode find(TreeNode root, char val) {
            
            
        if (root == null) return null;
        if (root.val == val) return root;

        TreeNode find_left = find(root.left, val);
        if (find_left != null) return find_left;
        TreeNode find_right = find(root.right ,val);
        if(find_right != null) return find_right;
        return null;
    }

    //层次遍历
    public void levelTraversal(TreeNode root){
            
            
        Queue<TreeNode> queue = new LinkedList<>(); //借助队列实现
        TreeNode temp;
        queue.offer(root);
        while(!queue.isEmpty()){
            
            
            temp = queue.poll();
            System.out.println(temp.val + ":");
            if(temp.left != null){
            
            
                queue.offer(temp.left);
            }
            if(temp.left != null){
            
            
                queue.offer(temp.right);
            }
        }
    }
}

```