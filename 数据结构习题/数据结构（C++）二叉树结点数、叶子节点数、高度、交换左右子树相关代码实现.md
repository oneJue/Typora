二叉树不管是求点数、叶子节点数、高度，或者交换左右子树，都离不开递归的思想，由于是左右子树，所以递归在二叉树中有着非常好的应用。

## 结点数

如果结点为空，则非有效结点，直接return。如果不是空，则+1的基础上，计算由此结点开始的左右子树的结点数，最后返回。


​    
```c++
int Count(BiNode* root)
	{
		int number = 0;
		if (root == NULL)
			number = 0;
		else
			number = Count(root->lchild) + Count(root->rchild) + 1;
		return number;
	}
```


## 叶子节点数

如果结点为空，则为0.如果不为空，且该结点的左右孩子都是空，那么就是叶子结点，置1。如果既不为空，且左右孩子也不为空，那此结点为左右孩子的叶子结点数相加。


​    
```c++
int LeafCount(BiNode* root)
	{
		int number = 0;
		if (root == NULL)
			number = 0;
		else if (root->lchild == NULL && root->rchild == NULL)
			number = 1;
		else
			number = LeafCount(root->lchild) + LeafCount(root->rchild);
		return number;
	}
```


## 树的高度

如果不为空，则结点的高度为左右孩子的高度最大值+1，为空为0.


​    
```c++
	int HighCount(BiNode* root)
	{
		int number = 0;
		if (root != NULL)
		{
			if (HighCount(root->lchild) > HighCount(root->rchild))
				number = HighCount(root->lchild) + 1;
			else
				number = HighCount(root->rchild) + 1;
		}
		else
		{
			return 0;
		}
		return number;
	}
```


## 交换左右子树

思想依旧是递归。


​    
    	void SwapTree(BiNode*root)
    	{
    		BiNode* temp;
    		if (root == NULL)
    			return;
    		else
    		{
    			temp = root->lchild;
    			root->lchild = root->rchild;
    			root->rchild = temp;
    			SwapTree(root->lchild);
    			SwapTree(root->rchild);
    		}
    	}


## 实现的全部代码：


​    
    #include<iostream>
    #include<stack>
    using namespace std;
    struct BiNode
    {
    	char data;
    	BiNode* lchild, * rchild;
    };
    class BiTree
    {
    public:
    	BiTree() { root = Creat(); }
    	void PreOrder() { PreOrder(root); }
    	int Count() { return Count(root); }
    	int LeafCount() { return LeafCount(root); }
    	int HighCount() { return HighCount(root); }
    	void SwapTree() { SwapTree(root); }
    private:
    	BiNode* root;
    	BiNode* Creat()
    	{
    		BiNode* root;
    		char ch;
    		cin >> ch;
    		if (ch == '#')
    			root = NULL;
    		else
    		{
    			root = new BiNode;
    			root->data = ch;
    			root->lchild = Creat();
    			root->rchild = Creat();
    		}
    		return root;
    	}
    	void PreOrder(BiNode* root)
    	{
    		stack<BiNode*>s;
    		while (root != NULL || !s.empty())
    		{
    			while(root != NULL)
    			{
    				cout << root->data;
    				s.push(root);
    				root = root->lchild;
    			}
    			if (!s.empty())
    			{
    				root = s.top();
    				s.pop();
    				root = root->rchild;
    			}
    		}
    	}
    	int Count(BiNode* root)
    	{
    		int number = 0;
    		if (root == NULL)
    			number = 0;
    		else
    			number = Count(root->lchild) + Count(root->rchild) + 1;
    		return number;
    	}
    	int LeafCount(BiNode* root)
    	{
    		int number = 0;
    		if (root == NULL)
    			number = 0;
    		else if (root->lchild == NULL && root->rchild == NULL)
    			number = 1;
    		else
    			number = LeafCount(root->lchild) + LeafCount(root->rchild);
    		return number;
    	}
    	int HighCount(BiNode* root)
    	{
    		int number = 0;
    		if (root != NULL)
    		{
    			if (HighCount(root->lchild) > HighCount(root->rchild))
    				number = HighCount(root->lchild) + 1;
    			else
    				number = HighCount(root->rchild) + 1;
    		}
    		else
    		{
    			return 0;
    		}
    		return number;
    	}
    	void SwapTree(BiNode*root)
    	{
    		BiNode* temp;
    		if (root == NULL)
    			return;
    		else
    		{
    			temp = root->lchild;
    			root->lchild = root->rchild;
    			root->rchild = temp;
    			SwapTree(root->lchild);
    			SwapTree(root->rchild);
    		}
    	}
    };
    int main()
    {
    	BiTree bt;
    	bt.PreOrder();
    	cout << endl;
    	cout << bt.Count() << endl;
    	cout << bt.LeafCount() << endl;
    	cout << bt.HighCount() << endl;
    	bt.SwapTree();
    	bt.PreOrder();
    	return 0;
    }

