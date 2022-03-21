[toc]

# 二叉树

## 层序二叉树创建与遍历——链表（作废）

~~~c
//谢玲红
//2020/11/13
//利用链表队列；层序二叉树创建与遍历（作废） 
#include <stdio.h>
#include <stdlib.h>
#define elementtype int

struct bintree
{
	elementtype data;
	struct bintree* left;
	struct bintree* right;
};
typedef struct bintree bintree;

struct list
{
	struct bintree* data;
	struct list* next;
};
typedef struct list list;

struct queue
{
	struct list* front;
	struct list* rear;
};
typedef struct queue queue;

bintree* creatbintree();
void level_order_traversal(bintree* bt);
void in_order_traversal(bintree* bt);
void pre_order_traversal(bintree* bt);
void post_order_traversal(bintree* bt);
queue* creatqueue();
bool insert(queue* q,bintree* x);
bintree* Delete(queue* q);

int main()
{
	bintree* bt=creatbintree();
	level_order_traversal(bt);
	printf("\n");
	in_order_traversal(bt);
	printf("\n");
	pre_order_traversal(bt);
	printf("\n");
	post_order_traversal(bt);
	printf("\n");
 } 
bintree* creatbintree()//层序方法创建树  
{
	queue* creatqueue();
	bool insert(queue* q,bintree* x);
	bintree* Delete(queue* q);
	
	bintree* t,*t1;
	elementtype x;
	queue* q=creatqueue();
	scanf("%d",&x);
	if(x!=0) 
	{
		t=(bintree*)malloc(sizeof(bintree));
		t->data=x;
		t->left=NULL;
		t->right=NULL;
		insert(q,t);
	}
	else return NULL;
	while(q->front)
	{
		t1=Delete(q);
		scanf("%d",&x);
		if(x==0) {t1->left=NULL;}
		else
		{
			t1->left=(bintree*)malloc(sizeof(bintree));
			t1->left->data=x;
			t1->left->left=NULL;
			t1->left->right=NULL;
			insert(q,t1->left);
		}
		scanf("%d",&x);
		if(x==0) {t1->right=NULL;}
		else
		{
			t1->right=(bintree*)malloc(sizeof(bintree));
			t1->right->data=x;
			t1->right->left=NULL;
			t1->right->right=NULL;
			insert(q,t1->right);
		}
	}
	return t;
}
void level_order_traversal(bintree* bt)//层序遍历输出树 
{
	queue* creatqueue();
	bool insert(queue* q,bintree* x);
	bintree* Delete(queue* q);
	
	queue* qt=creatqueue();
	bintree* t=(bintree*)malloc(sizeof(bintree));
	if(bt==NULL) 
	{
		printf("空！"); 
		return;
	}
	insert(qt,bt);
	while(qt->front)
	{
		t=Delete(qt);
		printf("%d",t->data);
		if(t->left) {insert(qt,t->left);}//！！！ 
		if(t->right) {insert(qt,t->right);} //！！！ 
	}
}
void in_order_traversal(bintree* bt)
{
	if(bt==NULL) return;
	else
	{
		in_order_traversal(bt->left);
		printf("%d",bt->data);
		in_order_traversal(bt->right);
	}

}
void pre_order_traversal(bintree* bt)
{
	if(bt==NULL) return;
	else
	{
		printf("%d",bt->data);
		pre_order_traversal(bt->left);
		pre_order_traversal(bt->right);
	}
}
void post_order_traversal(bintree* bt)
{
	if(bt==NULL) return;
	else
	{
		post_order_traversal( bt->left);
		post_order_traversal( bt->right);
		printf("%d",bt->data);
	}
}
queue* creatqueue()//创建队列，通过队列创建树 
{
	queue* q=(queue*)malloc(sizeof(queue));
	q->front=q->rear=NULL;
	return q;
}
bool insert(queue* q,bintree* x)//入队列 ，入队列的是地址，不是值 
{
	list* qt=(list*)malloc(sizeof(list));
	if(q->front==NULL)
	{
		qt->data=x;
		qt->next=NULL;
		q->front=qt;
		q->rear=qt;
	}
	else
	{
		qt->data=x;
		q->rear->next=qt;
		qt->data->left=qt->data->right=NULL;
		q->rear=q->rear->next;
		q->rear->next=NULL;
	}
//	printf("[%d]",q->rear->data->data);
	return true;
}
bintree* Delete(queue* q)//出队列 ，出队列的是地址，不是值 
{
	list* qt=(list*)malloc(sizeof(list));
	if(q->front!=NULL) 
	{
		if(q->front==q->rear)
		{
			qt=q->front;
			q->front=q->rear=NULL;
//			printf("{%d}",qt->data->data);
			return qt->data;
		}
		else
		{
			qt=q->front;
			q->front=q->front->next;
//			printf("{%d}",qt->data->data);
			return qt->data;
		}
	}
}

/*
1 2 3 4 5 6 7 0 0 8 0 0 9 0 0 0 0 0 0
*/
~~~

## 层序二叉树创建与遍历——顺序

~~~c
//谢玲红
//2020/11/20
//利用顺序储存方式的队列；创建和遍历 层序二叉树  
//二叉搜索树的查找，最值，插入和删除 
#include <stdio.h>
#include <stdlib.h>
#define elementtype int
struct bintree//二叉树结构体 
{
	struct bintree* left;
	struct bintree* right;
	elementtype data;
};
typedef struct bintree bintree;

struct queue//顺序存储方式 的队列结构体 
{
	struct bintree** DATA;
	int front;
	int rear;
	int maxsize;
};
typedef struct queue queue;

struct stack//顺序存储方式 的堆栈结构体 
{
	struct bintree** DATA;
	int top;
	int maxsize;
};
typedef struct stack stack;

queue* creatqueue(int n);
bool insertqueue(queue* Q,bintree* x);
bintree* outputqueue(queue* Q);
void display(queue* Q);
stack* creatstack(int n);
bool pushstack(stack* S,bintree* x);
bintree*  popstack(stack* S);
bintree* creatbintree();
void level_order_traversal(bintree* BT);
void in_order_traversal(bintree* BT) ;
void pre_order_traversal(bintree* BT);
void post_order_traversal(bintree* BT);
void in_order_traversal_(bintree* BT);
void pre_order_printleaves(bintree* BT);
int get_height(bintree* BT);
bintree* find(bintree* BT,elementtype x);
bintree* find_(bintree* BT,elementtype x);
elementtype find_min(bintree* BT);
elementtype find_max(bintree* BT);
bintree* insertbintree(bintree* BT,elementtype x); 
bintree* Deletebintree(bintree* BT,elementtype x);

int main()
{
	int n; 
	static elementtype x;
	printf("二叉树按“1” 二叉搜索树按“2”：");
	scanf("%d",&n); 
	bintree* BT=creatbintree();
	/* 不同方法的遍历 */ 
	level_order_traversal(BT);
	printf("\n\n二叉树的中序递归遍历：");
	in_order_traversal(BT);
	printf("\n\n二叉树的先序递归遍历："); 
	pre_order_traversal(BT);
	printf("\n\n二叉树的后序递归遍历：");
	post_order_traversal(BT);
	in_order_traversal_(BT);
	printf("\n\n二叉树的叶节点：");
	pre_order_printleaves(BT);
	printf("\n\n二叉树的高度：%d",get_height(BT));
	/* 二叉搜索树 */ 
	if(n==2)
	{
		printf("\n\n你想查找的结点是：");
		scanf("%d",&x);
		if(find(BT,x)!=NULL) printf("\n\n递归查找结点地址：%d",BT);
		if(find_(BT,x)!=NULL) printf("	迭代查找结点地址：%d",BT);
		printf("\n\n最小值是：%d",find_min(BT)); 
		printf("	最大值是：%d",find_max(BT));
		printf("\n\n要插入的值是：");
		scanf("%d",&x);
		level_order_traversal(insertbintree(BT,x));
		printf("\n\n要删除的值是：");
		scanf("%d",&x); 
		level_order_traversal(Deletebintree(BT,x));
	}
}

queue* creatqueue(int n)//创建空队列
{
	queue* Q=(queue*)malloc(sizeof(queue));
	Q->DATA=(bintree**)malloc(n*sizeof(bintree*));//*当给数组分配动态空间的时候，一定一定要乘上数组的量 
	Q->front=Q->rear=0;
	Q->maxsize=n;
	return Q;
}

bool insertqueue(queue* Q,bintree* x)//入队列 
{
	if((Q->rear+1)%Q->maxsize==Q->front)
	{
		printf("队列已满！");
		return false;
	}
	else 
	{
		Q->rear=(Q->rear+1)%Q->maxsize;
		Q->DATA[Q->rear]=x;
		return true;
	}
}

bintree* outputqueue(queue* Q)//出队列 
{
	bintree* t;
	if(Q->rear%Q->maxsize==Q->front) 
	{
		printf("队列为空！");
		return NULL;
	}
	else
	{
		Q->front=(Q->front+1)%Q->maxsize;
		t=Q->DATA[Q->front];
		return t;
	} 
}

void display(queue* Q)//展示队列内的数据
{
	int i=Q->front+1;
	if(Q->rear%Q->maxsize!=Q->front)
	{
		printf("队列剩余："); 
		while(i%Q->maxsize && i!=Q->rear || i%Q->maxsize!=Q->rear && i>Q->maxsize )
		{
			i=i%Q->maxsize;
			printf("%d ",Q->DATA[i]->data);
			i++;
		}
		printf("%d\n\n",Q->DATA[Q->rear]->data);//*如果输出的变量没有值，是执行不下去的 
	}
	else printf("队列已空\n\n"); 
}

stack* creatstack(int n)//创建空堆栈 
{
	stack* S=(stack*)malloc(sizeof(stack));
	S->DATA=(bintree**)malloc(n*sizeof(bintree*));
	S->top=-1;
	S->maxsize=n;
	return S;
} 

bool pushstack(stack* S,bintree* x)//插入堆栈 
{
	if(S->top==S->maxsize-1)
	{
		printf("堆栈已满\n");
		return false; 
	}
	else 
	{
		S->DATA[++(S->top)]=x;
		return true;
	}
} 

bintree*  popstack(stack* S)//跳出堆栈 
{
	if(S->top==-1)
	{
		printf("堆栈已空\n");
		return NULL;
	} 
	else bintree* b=S->DATA[(S->top)--];
}

bintree* creatbintree()//创建层序二叉树 
{
	bintree* BT,*T;
	queue* Q=creatqueue(20);
	elementtype x;
	printf("输入层序二叉树（二叉搜索树）的序列："); 
	scanf("%d",&x);//根节点 
	if(x!=0) //根结点为0则二叉树为空 
	{
		BT=(bintree*)malloc(sizeof(bintree));
		BT->data=x;
		BT->left=BT->right=NULL;
		insertqueue(Q,BT);//根结点入队列 
		printf("头结点入队列：%d\n",BT->data);
	}
	else return NULL;
	
	while(Q->rear%Q->maxsize!=Q->front)//队列不为空则还有子结点
	{
		T=outputqueue(Q);//按层序方式，结点出队列 
		printf("接到二叉树上的数是:%d\n",T->data);
		scanf("%d",&x);//输入该结点左孩子 
		if(x==0) T->left=NULL;
		else
		{
			T->left=(bintree*)malloc(sizeof(bintree));
			T->left->data=x;
			T->left->left=T->left->right=NULL;
			insertqueue(Q,T->left);
		}
		if(T->left!=NULL) printf("这个数的左孩子是：%d\n",T->left->data);//*如果输出的变量没有值，则就会停止执行，直接结束 
		else printf("这个数没有左孩子\n"); 
		scanf("%d",&x);//输入该结点右孩子 
		if(x==0) T->right=NULL;
		else
		{
			T->right=(bintree*)malloc(sizeof(bintree));
			T->right->data=x;
			T->right->left=T->right->right=NULL;
			insertqueue(Q,T->right);
		} 
		if(T->right!=NULL) printf("这个数的右孩子是：%d\n",T->right->data);//*如果输出的变量没有值，则就会停止执行，直接结束 
		else printf("这个数没有右孩子\n"); 
		printf("左右孩子入队列\n"); 
		display(Q);//输出此时队列内的数据 
	}
	printf("已建完二叉树\n"); 
	return BT;//返回根结点 
}

void level_order_traversal(bintree* BT)//层序非递归遍历——队列 
{
	
	if(!BT) 
	{
		printf("无二叉树\n");
		return;
	}
	queue* Q=creatqueue(20);
	bintree* T;
	insertqueue(Q,BT);
	printf("\n\n二叉树的层序非递归遍历：");
	while(Q->rear%Q->maxsize!=Q->front)
	{
		T=outputqueue(Q);
		printf("%d",T->data);
		if(T->left)  insertqueue(Q,T->left);
		if(T->right) insertqueue(Q,T->right);
	}	
}

void in_order_traversal(bintree* BT) //中序递归遍历 
{ 
	if(BT)
	{
		in_order_traversal(BT->left);
		printf("%d",BT->data);
		in_order_traversal(BT->right);
	}
}

void pre_order_traversal(bintree* BT)//先序递归遍历 
{
	if(BT)
	{
		printf("%d",BT->data);
		pre_order_traversal(BT->left);
		pre_order_traversal(BT->right);
	}
} 

void post_order_traversal(bintree* BT)//后序递归遍历 
{
	if(BT)
	{
		pre_order_traversal(BT->left);
		pre_order_traversal(BT->right);
		printf("%d",BT->data);
	}
}

void in_order_traversal_(bintree* BT)//中序非递归遍历——堆栈 
{
	 stack* S=creatstack(20);
	 if(BT)
	 {
	 	printf("\n\n二叉树的中序非递归遍历：");
	 	bintree* T=BT; 
		while(T || S->top!=-1)
		{
			while(T)
			{
				pushstack(S,T);
				T=T->left;
			}
			T=popstack(S);
			printf("%d",T->data);
			T=T->right;
		 } 
	 }
}

void pre_order_printleaves(bintree* BT)//叶节点的遍历 
{
	if(BT)
	{
		if(!BT->left && !BT->right)//if条件放在前中后位置，结果都一样 
		{
			printf("%d",BT->data);
		}
		pre_order_printleaves(BT->left);
		pre_order_printleaves(BT->right);
	}
} 

int get_height(bintree* BT)//二叉树的高度 
{
	int hl,hr,hmax;
	if(BT)
	{
		hl=get_height(BT->left);
		hr=get_height(BT->right);
		hmax= hl>hr ? hl : hr ;
		return hmax+1;
	}
	else return 0;
}

bintree* find(bintree* BT,elementtype x)//二叉搜索树的递归查找 
{
	
	if(!BT) return NULL;
	if(x>BT->data) return find(BT->right,x);
		 else if(x<BT->data) return find(BT->left,x);
		 	  else if(x==BT->data) {return BT;}
				   else {return NULL;}
}

bintree* find_(bintree* BT,elementtype x)//二叉搜索树的递归查找 
{
	if(!BT) return NULL;
	while(BT)
	{
		if(x>BT->data) BT=BT->right;
		else if(x<BT->data) BT=BT->left;
		 	 else if(x==BT->data) {return BT;}
				  else {return NULL;}
	}
}

elementtype find_min(bintree* BT)//二叉搜索树的迭代查找最小值 
{
	if(!BT) return NULL;
	else if(BT->left) return find_min(BT->left);
		 else return BT->data;
} 

elementtype find_max(bintree* BT)//二叉搜索树的迭代查找最大值 
{
	if(!BT) return NULL;
	while(BT->right) BT=BT->right;
	return BT->data;
}

bintree* insertbintree(bintree* BT,elementtype x)//二叉搜索树的插入 
{
	if(!BT)
	{
		BT=(bintree*)malloc(sizeof(bintree));//** 不可以bintree* BT 
		BT->data=x;
		BT->left=BT->right=NULL;
	}
	else
	{
		if(x>BT->data) BT->right=insertbintree(BT->right,x);
		else if(x<BT->data) BT->left=insertbintree(BT->left,x);
	}
	return BT;
} 

bintree* Deletebintree(bintree* BT,elementtype x)//二叉搜索树的删除 
{
	if(!BT) printf("二叉树中没有这个值");
	else
	{
		if(x>BT->data) BT->right=Deletebintree(BT->right,x);
		else if(x<BT->data) BT->left=Deletebintree(BT->left,x);
		else//BT是要删除的结点 
		{
			if(BT->left && BT->right)
			{
				elementtype y;
				y=find_min(BT->right);//要删除的结点上的值用该结点右子树中最小值代替 
				BT->data=y;
				BT->right=Deletebintree(BT->right,y);//**删除代替要删除的结点上的那个结点 
			}
			else
			{
				bintree* y=BT;//我咋觉得这一步没必要 
				if(!BT->left) BT=BT->right;//要删除的结点只有右孩子或者没有孩子 
				else BT=BT->left;
				free(y);
			}
		}
	}
	return BT;
}
/* 1 2 3 4 5 6 7 0 0 8 0 0 9 0 0 0 0 0 0 */
/* 5 2 8 1 3 7 9 0 0 0 0 0 0 0 0 */
~~~

# 树应用——最大堆

~~~c
//谢玲红
//2020/11/25
//最大堆创建，插入，删除 
#include <stdio.h>
#include <stdlib.h>
#define maxsize 10000
#define elementtype int
struct Heap
{
	elementtype* data;
	int size;
	int capacity;
};
typedef struct Heap MaxHeap;

MaxHeap* creatMaxHeap(int n)//创建空最大堆 
{
	MaxHeap* H=(MaxHeap*)malloc(sizeof(MaxHeap));
	H->data=(elementtype*)malloc(sizeof(elementtype));
	H->size=0;	
	H->capacity=n;
	H->data[0]=maxsize;
	return H;
}

bool insertMaxHeap(MaxHeap* H,elementtype x)//插入最大堆 
{
	if(H->size==H->capacity) 
	{
		printf("堆已满");
		return false; 
	}
	else
	{
		int i;
		i=++H->size;
		for(;x>H->data[i/2];i/=2)
		{
			H->data[i]=H->data[i/2];
		}
		H->data[i]=x;
	}
	return true;	
}

bool DeleteMaxHeap(MaxHeap* H)//删除根结点，调整最大堆 
{
	if(H->size==0)
	{
		printf("堆已空");
		return false; 
	}
	else
	{
		int parent,child;
		elementtype temp,x;
		temp=H->data[1];//放根结点 
		x=H->data[H->size--];//存储尾结点 
		for(parent=1;parent*2<=H->size;parent=child)//父亲有孩子就循环 
		{
			child=parent*2;
			if(child!=H->size && H->data[child]<H->data[child+1])//再考虑左右孩子中最大值 
			{
				child++;
			}
			if(x>=H->data[child]) break;//最后考虑左右孩子中最大值和尾结点 两者之间的最大值 
			else H->data[parent]=H->data[child];
		}
		H->data[parent]=x;
	}
	return true;
 } 
 
int main()
{	
	MaxHeap* H=creatMaxHeap(50);
	elementtype x;
	int i,j;
	printf("最大堆的个数：");
	scanf("%d",&i); 
	printf("最大堆层序序列是：");
	for(j=1;j<=i;j++)
	{
		scanf("%d",&x); 
		insertMaxHeap(H,x);
	}
	for(j=1;j<=i;j++)
	{
		printf("%d ",H->data[j]);
	}
	printf("删除最大堆中的最大值后的序列是："); 
	DeleteMaxHeap(H);
	i--;
	for(j=1;j<=i;j++)
	{
		printf("%d ",H->data[j]);
	}
}
/* 44 25 31 18 10 */
~~~

## 普通堆转换成最大堆

~~~c
//谢玲红
//2020/11/26
//将普通堆转换成最大堆 
#include <stdio.h>
#include <stdlib.h>
#define elementtype int
#define maxdata 10000
struct Heap
{
	elementtype* data;
	int size;
	int capacity;
};
typedef struct Heap MaxHeap;

MaxHeap* creatHeap(int n)
{
	MaxHeap* H=(MaxHeap*)malloc(sizeof(MaxHeap));
	H->data=(elementtype*)malloc(sizeof(elementtype));
	H->size=0;
	H->capacity=n;
	H->data[0]=maxdata;
	
	int i,m;
	printf("最大堆的个数是：");
	scanf("%d",&m);
	if(m)
	{
		printf("普通堆层序序列是：");
		for(i=1;i<=m;i++)
		{
			scanf("%d",&H->data[++H->size]);
		}
	}
	return H;
}
void precdown(MaxHeap* H,int p)
{
	if(!H) printf("此堆为空");
	else
	{
		int parent,child;
		elementtype x;
		x=H->data[p];
		for(parent=p;parent*2<=H->size;parent=child)
		{
			child=parent*2;
			if(child!=H->size && H->data[child]<H->data[child+1])
			{
				child++;
			}
			if(x>=H->data[child]) break;
			else H->data[parent]=H->data[child];
		}
		H->data[parent]=x;
	} 
}

void buildHeap(MaxHeap* H)
{
	int i;
	for(i=H->size/2;i>0;i--)
	{
		precdown(H,i);
	}
}

int main()
{
	MaxHeap* H=creatHeap(50);
	int i;
	buildHeap(H);
	printf("\n最大堆的层序序列："); 
	for(i=1;i<=H->size;i++)
	{
		printf("%d ",H->data[i]);
	}
}
/* 44 25 31 18 10 */
/* 25 31 10 18 44 */
~~~

# 树的应用——哈夫曼树（未完成）

~~~c
//谢玲红
//2020/11/26
//哈夫曼树
#include <stdio.h>
#include <stdlib.h>
#define mindata -1 
struct HaffumanTree
{
	int weight;
	struct HaffumanTree* left;
	struct HaffumanTree* right;
};
typedef struct HaffumanTree HaffumanTree;
typedef struct HaffumanTree* elementtype;
struct Heap
{
	elementtype* data;
	int size;
	int capacity;
 } ;
typedef struct Heap MinHeap;

/* 7 19 2 6 32 21 10 */

MinHeap* creatHeap(int n)//创建权值 
{
	/* 空普通堆 */ 
	MinHeap* H=(MinHeap*)malloc(sizeof(MinHeap));
	H->data=(elementtype*)malloc(n*sizeof(elementtype));
	H->size=0;
	H->capacity=n;
//	printf("\n____调试程序_____\n");
	H->data[0]->weight=mindata;
//	printf("\n____调试程序_____\n");
	/* 普通堆入值 */ 
	int i,m;
	printf("权值个数：");
	scanf("%d",&m);
	if(m)
	{
		printf("权值序列：");
		for(i=1;i<=m;i++)//**注意用i还是用H->size 较合适 
		{
			scanf("%d",&H->data[++H->size]->weight);//**
			H->data[H->size]->left=H->data[H->size]->right=NULL;
//			printf("\n____调试程序_____\n");
		}
//		printf("\n____调试程序_____\n");
		for(i=1;i<=m;i++)
		{
			printf("%d",H->data[i]->weight);
		}
	}
	
	return H;
}

bool insertMinHeap(MinHeap* H,elementtype x)//插入最小堆 
{
	if(H->size==H->capacity) 
	{
		printf("最小堆已满");
		return false;
	}
	else
	{
		int i=++H->size;
		for(;x<H->data[i/2];i/=2)//**
		{
			H->data[i]->weight=H->data[i/2]->weight;
		}
		H->data[i]=x;
		return true;
	}
}

bool DeleteMinHeap(MinHeap* H)//删除最小堆 
{
	if(H->size==0) 
	{
		printf("最小堆已空");
		return false; 
	}
	else
	{
		int parent,child;
		elementtype temp,x;
		temp=H->data[1]; 
		x=H->data[H->size--];
		for(parent=1;parent*2<=H->size;parent=child)
		{
			child=parent*2;
			if(child!=H->size && H->data[child]->weight>H->data[child+1]->weight) child++;
			if(H->data[child]->weight<=x->weight) H->data[parent]=H->data[child];
			else break;
		}
		H->data[parent]=x;
		return true;
	}	
}

void precdown(MinHeap* H,int p)//局部最小堆的建立 
{
	int parent,child;
	elementtype x;
	x=H->data[p];
	for(parent=p;parent*2<=H->size;parent=child)
	{
		child=parent*2;
		if(child!=H->size && H->data[child]->weight>H->data[child+1]->weight) child++;
		if(H->data[child]->weight<=x->weight) H->data[parent]=H->data[child];
		else break;
	}
	H->data[parent]=x;
}

void buildMinHeap(MinHeap* H)//最小堆的建立 
{
	int i=H->size/2;
	for(i;i>0;i--)
	{
		precdown(H,i);
	}
}

int main()
{
	MinHeap* H=creatHeap(50);
	int i;
	for(i=1;i<=H->size;i++)
	{
		printf("%d",H->data[i]->weight);
	}
}

~~~

