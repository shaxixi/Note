[toc]

# 队列

## 顺序队列 先进先出

~~~c
//谢玲红
//2020/11/4
//顺序队列实现 先进先出 
#include <stdio.h>
#include <stdlib.h>
#define elementtype int
struct queue
{
	elementtype* data;
	int maxsize;
	int front;//队列的头 
	int rear; //队列的尾 
};
typedef struct queue queue; 

void scan(queue* p);
void print(queue* p);
queue* createqueue(int n);
bool insert(queue* p,elementtype x);
elementtype Delete(queue* p);

int main()
{
	queue* p;
	static int n;
	int x;
	printf("队列的总数是:");//确定队列中数据的个数 
	scanf("%d",&n);
	p=createqueue(n+1);//因为队列中有一个地址的内容是空的，所以创建队列的时候要加一 
	printf("进入队列的数据是：");
	while((p->rear+1)%p->maxsize!=p->front)
	{
		scanf("%d",&x);
		insert(p,x); 
	}
	
	printf("出队列的数据是：");
	while(p->front!=p->rear)
	{
		printf("%d",Delete(p));
	}	
}

queue* createqueue(int n)//创造一个空队列 
{
	queue* q=(queue*)malloc(sizeof(queue));
	q->data=(elementtype*)malloc(sizeof(queue));
	q->maxsize=n;
	q->front=q->rear=0;
	return q;
}

bool insert(queue* p,elementtype x)//队列尾插入数据 
{
	if((p->rear+1)%p->maxsize==p->front)
	{
		printf("队列已满！");
		return false; 
	}
	p->rear=(p->rear+1)%p->maxsize;//不和38行调换顺序的原因是：p->front==p->rear时为空，不能存放数据 
	p->data[p->rear]=x;
	return true;
}

elementtype Delete(queue* p)//队列头删除数据 
{
	if(p->front==p->rear) 
	{
		printf("队列已空！");
		return false; 
	}
	p->front=(p->front+1)%p->maxsize;
	return p->data[p->front];
}

~~~

## 链式队列实现

~~~c
//谢玲红
//2020/11/5
//链式队列实现 
#include <stdio.h>
#include <stdlib.h>
#define elementtype int
struct node
{
	elementtype data;
	struct node* next;
 }; 
typedef struct node node;

struct queue
{
	int maxsize;
	node* front;
	node* rear;
};
typedef struct queue queue;
 
queue* creatqueue(int n);
bool insert(queue* p,elementtype x);
elementtype Delete(queue* p);

int main()
{
	queue* p;
	int x=0;
	static int n;
	printf("队列的数据个数是：");
	scanf("%d",&n); 
	p=creatqueue(n);
	printf("输入队列的数据：");
	for(n;n>0;n--)
	{
		scanf("%d",&x);
		insert(p,x);
	}
	while(p->front!=NULL)
	{
		printf("%d ",Delete(p));
	}
}

queue* creatqueue(int n)//创造一个空队列 
{
	queue* p=(queue*)malloc(sizeof(queue));
	p->rear=p->front=NULL;
	p->maxsize=n;
	return p;
}

bool insert(queue* p,elementtype x)//不带头结点的数据进队列 
{
	node* pt=(node*)malloc(sizeof(node));
	if(p->front==NULL)//在本结点放数据 
	{
		pt->data=x;//注意：pt是指向数据和下一个结点的，不是指向头结点和尾结点的 
		pt->next=NULL;
		p->rear=pt;
		p->front=pt;//为了方便判断队列空不空，用front来判断空不空，rear只负责增加就好了 
	}
	else//在下一结点放数据，然后往后推一个结点 
	{
		p->rear->next=pt;
		pt->data=x;
		pt->next=NULL;
		p->rear=p->rear->next;	
	}
	return true;
 } 
 
 elementtype Delete(queue* p)// 不带头结点的数据出队列 
 {
 	node* pt=(node*)malloc(sizeof(node));
 	elementtype x;
 	if(p->front==NULL)
 	{
 		printf("队列已空！");	
 		return 0;
	}
	pt=p->front;
	x=pt->data;
	p->front=pt->next;
	free(pt);
	return x;
  } 
~~~

# 队列应用——十进制转换不用进制

~~~c
//谢玲红
//2020/11/6
//将十进制数转换成二进制，八进制，十六进制 
#include <stdio.h>
#include <stdlib.h>
struct stack
{
	int data;
	struct stack* next;
};
typedef struct stack stack;

stack* creatstack();
bool insert(stack* p,int x);
int Delete(stack* p);
int count_2(stack* p,int x);

int main()
{
	stack* p=creatstack();
	int x;
	printf("输入一个十进制整数：");
	scanf("%d",&x);
	count_2(p,x);
}

stack* creatstack()
{
	stack* p=(stack*)malloc(sizeof(stack));
	p->next=NULL;
	return p;
}

bool insert(stack* p,int x)
{
	stack* pt=(stack*)malloc(sizeof(stack));
	pt->next=p->next;
	pt->data=x;
	p->next=pt;
//	printf("(%d) ",pt->data);
	return true;
}

int Delete(stack* p)
{
	stack* pt=(stack*)malloc(sizeof(stack));
	int x;
	pt=p->next;
	x=pt->data;
	p->next=pt->next;
	free(pt);
//	printf("{%d} ",x);
	return x;
}

int count_2(stack* p,int x)//十进制转化成二进制和八进制 ,十六进制 
{
//	printf("[%d] ",x);
	bool insert(stack* p,int x);
	int Delete(stack* p);
	int y;
	printf("二进制为："); 
	y=x;
	while(y!=0) 
	{
		insert(p,y%2);
		y=y/2;
	}
	while(p->next!=NULL) printf("%d",Delete(p));
	
	printf("\n八进制为：");
	y=x;
	while(y!=0) 
	{
		insert(p,y%8);
		y=y/8;
	}
	while(p->next!=NULL) printf("%d",Delete(p));
	
	printf("\n十六进制为：");
	y=x;
	while(y!=0)
	{
		insert(p,y%16);
		y=y/16;
	}
	while(p->next!=NULL)
	{
		int z=Delete(p);
		switch(z) 
		{
			case 10: printf("A");break;
			case 11: printf("B");break;
			case 12: printf("C");break;
			case 13: printf("D");break;
			case 14: printf("E");break;
			case 15: printf("F");break;
			default: printf("%d",z);
		}
		
	}
}


~~~

