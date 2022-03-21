[toc]

# 堆栈

## 顺序堆栈

~~~c
//谢玲红
//2020.10.27
//顺序堆栈 
#include <stdio.h>
#include <stdlib.h>
#define datatype int
struct stack 
{
	int maxsize;
	datatype* data;	//为什么不用数组形式，因为数组需要有原始的限定容量[]，而指针可以后期自定义容量 
	int top;
};
typedef struct stack stack;

int main()
{
	stack* push(int n);
	void pop(stack* p);
	
	stack* p;
	printf("五个整数的入栈顺序是："); 
	p=push(5);
	printf("五个整数的出栈顺序是："); 
	pop(p);
 } 
 
stack* push(int n)	//创建栈并且入栈 
{
	stack* p=(stack*)malloc(sizeof(stack));
	p->data=(datatype*)malloc(n*sizeof(stack));
	p->maxsize=n;
	p->top=-1;	//栈的底端开始进入入 
	while(p->top!=p->maxsize-1) scanf("%d",&p->data[++(p->top)]);//由于top初始值为-1，所以要先加一 
	return p;
 } 
 
void pop(stack* p)
{
	stack* pt;
	pt=p;
	if(pt->top==-1)	printf("error!");
	else while(pt->top!=-1) printf("%6d",pt->data[(pt->top)--]);
}

~~~

## 顺序双栈

~~~c
//谢玲红
//2020.10.27
//顺序双栈
#include <stdio.h>
#include <stdlib.h>
#define elementtype int
struct stack
{
	int maxsize;
	elementtype* data;
	int top1;
	int top2;
};
typedef struct stack stack;

int main()
{
	void scan(stack* p);
	void print(stack* p);
	stack* makestack(int n);
	bool push(stack* p,int tag,elementtype x);
	
	stack* p;
	p=makestack(7);
	scan(p);
	print(p);
 } 
 
 void scan(stack* p)	//用循环将所有的整数入栈 
 {
 	bool push(stack* p,int tag,elementtype x);
 	
 	int tag;
 	elementtype x;
 	printf("输入所放栈（栈一：1 栈二：2）和入栈整数：\n"); 
 	while(p->top1+1!=p->top2)
 	{
 		scanf("%d %d",&tag,&x);
 		push(p,tag,x);
	 }
 }
 
void print(stack* p)	//用循环将所有的整数出栈 
 {
 	elementtype pop(stack* p,int tag);
 	
 	printf("栈一出栈："); 
 	while(p->top1!=-1)
 	{
 		printf("%d",pop(p,1)) ;
	 }
	 printf("\n栈二出栈："); 
	 while(p->top2!=p->maxsize)
	 {
	 	printf("%d",pop(p,2)) ;
	 }
 	
 }
 
 stack* makestack(int n)	//建立空栈 
 {
	stack* p=(stack*)malloc(sizeof(stack));
	p->data=(elementtype*)malloc(n*sizeof(elementtype));  
	p->maxsize=n;
	p->top1=-1;
	p->top2=n;
	return p;
 }
 
bool push(stack* p,int tag,elementtype x)	//单个整数入栈 
 {
 	if(p->top1+1==p->top2) 
		{
			printf("full!");
			return false;
		}
	if(tag==1)
	{
		
		p->data[++(p->top1)]=x;
		return true;
	}
	else
	{
		p->data[--(p->top2)]=x;
		return true;
	}
  } 
  
elementtype pop(stack* p,int tag)	//单个整数出栈 
  {
	if(tag==1)
	{
		if(p->top1==-1)
		{
			printf("zero!");
			return false;
		}
		return p->data[(p->top1)--];
	 } 
	 else
	 {
 		if(p->top2==p->maxsize)
 		{
 			printf("zero!");
 			return false;
		 }
		 return p->data[(p->top2)++];
	 }
  }
~~~

## 链式堆栈

~~~c
//谢玲红
//2020.10.28
//链式堆栈
#include <stdio.h>
#include <stdlib.h>
#define elementtype int
struct stack
{
	elementtype data;
	struct stack* next;
 } ;
 typedef struct stack stack;
 
 int main()
 {
 	stack* makestack();
 	void scan(stack* p,int n);
 	void print(stack* p);
 	bool push(stack* p,elementtype x);
 	elementtype Delete(stack* p);
 	
 	int i;
 	stack* p=makestack();
 	printf("入栈的5个整数为：\n");
 	for(i=0;i<5;i++)	//用循环不断入栈 
 	{
 		elementtype x;
 		scanf("%d",&x);
 		push(p,x);
	 }
/*	push(p,1);
	push(p,2);
	push(p,3);
	push(p,4);
	push(p,5);*/
	printf("出栈的5个整数为：\n");	 
 	while(p->next!=NULL)	//用循环不断出栈并且删除顶节点 
 	{
 		printf("%d",Delete(p));
	 }
 }
 
 stack* makestack()	//创建一个空表 
 {
 	stack* p=(stack*)malloc(sizeof(stack));
	p->next=NULL; 
	return p;
 }
 
bool push(stack* p,elementtype x)	//单个入栈整数 
 {
 	stack* pt;
	 if((pt=(stack*)malloc(sizeof(stack)))==NULL) return false;
	 else
	 {
	 	pt->data=x;
	 	pt->next=p->next;
	 	p->next=pt;
	 	return true;
	 }
 }
 
elementtype Delete(stack* p)	//删除顶节点并且出栈顶节点的整数 
 {
 	stack* pt;
 	elementtype x;
 	if(p->next==NULL) return false;
 	else 
 	{
		pt=p->next;
		x=pt->data;
		p->next=pt->next;
		free(pt);
		return x;
	}
  } 
~~~

# 堆栈应用——表达式求值

~~~c
//谢玲红
//2020.10.28
//堆栈应用 表达式求值
#include <stdio.h>
#include <stdlib.h>
#define elementtype char
#define maxsize 100
struct stack
{
	elementtype data;
	struct stack* next;
 };
struct list
{
	elementtype data;
	struct list* next;
};
 typedef struct stack stack;
 typedef struct list list;

stack* makestack();
bool push(stack* p,elementtype x);
elementtype pop(stack* p);
list* makelist();
list* insert(list* head,elementtype x);
bool change(stack* p,elementtype x,list* pt);
bool last_insert(stack* p,list* pt);
bool count(list* p,stack* pt);
bool push_num(stack* p,int x);

 int main()
 {
 	stack* p=makestack();
 	list* pt=makelist();
 	stack* p1=makestack();
 	char x='0';
 	printf("输入中缀表达式,以“#”开头，以“;”结束：\n");
 	while(x!=';')
 	{
 		scanf("%c",&x);
 		change(p,x,pt);
	 }
	last_insert(p,pt);
	printf("\n"); 
	printf("后缀表达式：\n");
	list* pp=pt;
	while(pp->next!=NULL) 
	{
		pp=pp->next;
		printf("%c ",pp->data); 
	}
	count(pt,p1);
 }
 
 stack* makestack()	//创建一个堆栈空表 
 {
 	stack* p=(stack*)malloc(sizeof(stack));
 	p->next=NULL;
 	return p;
 }
 
 bool push(stack* p,elementtype x)	//将x数据压入栈p中 
 {
 	stack* pt=(stack*)malloc(sizeof(stack));
 	pt->next=p->next;
 	p->next=pt;
 	pt->data=x;
//	printf("[%c]",pt->data);
 	return true;
 }
 
  bool push_num(stack* p,int x)	//将x数据压入栈p中 
 {
 	stack* pt=(stack*)malloc(sizeof(stack));
 	pt->next=p->next;
 	p->next=pt;
 	pt->data=x;
//	printf("[%c]",pt->data);
 	return true;
 }
 elementtype pop(stack* p)	//让顶置数据出栈并删除顶置数据 
 {
 	stack* pt;
 	elementtype x;
 	if(p->next==NULL) return 0;
 	else
	{
		pt=p->next;
		x=pt->data;
		p->next=pt->next;
		free(pt);
//		printf("(%c)",x);
		return x;
	}
 }
 
 list* makelist()	//创建一个空的线性表 ：存储中缀表达式转换成后缀表达式之后的表达式顺序 
 {
 	list* head;
 	if((head=(list*)malloc(sizeof(list)))==NULL) return 0;
	head->next=NULL;
	return head; 
 }
 
list* insert(list* head,elementtype x)	//插入数据  
 {
 	list* r,*s;
 	r=head;
 	if((s=(list*)malloc(sizeof(list)))==NULL) return 0;
 	while(r->next!=NULL) 
 	{
 		r=r->next;
	 }
	s->data=x;	//区别于：r->next=s;	r=s;（有内部循环，所以要不断替换） 
	s->next=r->next;
	r->next=s;
//	printf("{%c}",s->data);
	return head; 
 }
 
 // 将中缀表达式转变成后缀表达式 
bool change(stack* p,elementtype x,list* pt)	//	利用堆栈，将数据入栈出栈插入到线性表 
 {
 	bool push(stack* p,elementtype x);
 	elementtype pop(stack* p);
 	list* insert(list* head,elementtype x);
//	printf("%c",x);
	int y;
 	if(x=='*'||x=='/'||x=='%') y=1;
	else if(x=='+'||x=='-') y=2;
		else if(x=='(') y=6;
			else if(x==')') y=7;
				else if(x=='#') y=8;
					else if(x==';') y=9;
						else y=10;
 	switch (y)
 	{
 		case 1: push(p,x);break;
 		case 2:
 			while((p->next->data=='*')||(p->next->data=='/')||(p->next->data=='%')) 
 			{insert(pt,pop(p));}
			push(p,x);
			break;
		case 6: push(p,x);break;
		case 7:
			while(p->next->data!='(') insert(pt,pop(p));
			pop(p);
			break;
		case 8:
			push(p,x);
			break;
		case 9: 
			break;
		default: 
			insert(pt,x);
	 }
	return true;
 }
 
bool last_insert(stack* p,list* pt)		//将堆栈中剩余的运算符插入到线性表中，得出完整的后缀表达式 
{
	list* insert(list* head,elementtype x);
	elementtype pop(stack* p);
	while(p->next!=NULL) insert(pt,pop(p));
	return true;
}

//计算后缀表达式 
bool count(list* p,stack* pt)
{
	bool push_num(stack* p,int x);
	elementtype pop(stack* p);
	list* now;
	elementtype x;
	int a,b,c;
	if(p==NULL) return false;
	now=p->next;
	while(p->data!='#')
	{
		x=p->data;
		int y;
	 	if(x=='*') y=1;
	 	else if(x=='/') y=2;
	 		else if(x=='%') y=3;
				else if(x=='+') y=4;
					else if(x=='-') y=5;
						else y=10;
	 	switch (y)
	 	{
	 		case 1: b=pop(pt);a=pop(pt);c=a*b;push_num(pt,c);break;
	 		case 2: b=pop(pt);a=pop(pt);c=a/b;push_num(pt,c);break;	
			case 3: b=pop(pt);a=pop(pt);c=a%b;push_num(pt,c);break;
			case 4: b=pop(pt);a=pop(pt);c=a+b;push_num(pt,c);break;
			case 5: b=pop(pt);a=pop(pt);c=a-b;push_num(pt,c);break;
			default: c=(int)x-48;push(pt,c);//因为x是字符数据类型，根据数字的0-9的ASCII码值为48-57，
			//所以为了把它变成数字0-9，就将它的ASCII码值减去48 
		}
		p=p->next;
	}
	printf("\n\n计算结果：\n%d",pop(pt));
	return true;
}
/*
#a+b/(c-d)*(e+f)%g;
#2+4/(3-1)*(5+4)%2;
*/ 
~~~

