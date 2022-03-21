[toc]

# 多项式相加

## 顺序存储方式

~~~c
#include <stdio.h>
#include <stdlib.h> 
#define elementype int
#define mixsize 100
struct list
{
	elementype xi[mixsize];
	elementype zhi[mixsize];
	int last;
};
typedef struct list list;

list *creat_list(int n)//创建顺序结构的链表		//指针函数 
{
	int i;
	list *p;
	p=(list *)malloc(sizeof(list));
	p->last=n;
	for(i=0;i<p->last;i++)	scanf("%d %d",&p->xi[i],&p->zhi[i]);
	return p;
}

list *print(list *p)
{
	int i;
	for(i=0;i<p->last;i++) printf("%-8d %-8d\n",p->xi[i],p->zhi[i]);
}

list sum_list(list *p1,list *p2)//多项式相加并且输出 
{
	int i=0,j=0;
	while(i!=p1->last||j!=p2->last)
	{
		if((i!=(p1->last))&&(j!=(p2->last)))
		{
			if((p1->zhi[i])>(p2->zhi[j]))
			{
				printf("%dx^%d + ",p1->xi[i],p1->zhi[i]);
				i++;
				continue;
			}
			if((p1->zhi[i])<(p2->zhi[j]))
			{
				printf("%dx^%d + ",p2->xi[j],p2->zhi[j]);
				j++;
				continue;
			}
			if((p1->zhi[i])==(p2->zhi[j]))
			{
				printf("%dx^%d + ",p1->xi[i]+p2->xi[j],p1->zhi[i]);
				i+=1;
				j+=1;
				continue;
			}	
		}
		if(i==p1->last)
		{
			if(j+1==p2->last)
			{
				printf("%dx^%d	",p2->xi[j],p2->zhi[j]);
				j++;
			}
			else
			{
				printf("%dx^%d + ",p2->xi[j],p2->zhi[j]);
				j++;
			}
			
			continue;
		}
		if(j==p2->last)
		{
			if(i+1==p1->last)
			{
				printf("%dx^%d	",p1->xi[i],p1->zhi[i]);
				i++;	
			}
			else
			{
				printf("%dx^%d + ",p1->xi[i],p1->zhi[i]);
				i++;
			}
			continue;
		}
	}
}
int main()
{
	list *creat_list(int n);
	list sum_list(list *p1,list *p2);
	list *print(list *p);
	
	list *p1,*p2;
	printf("\n输入第一个多项式中每项的系数 指数：\n");
	p1=creat_list(3);
	printf("\n输入第一个多项式中每项的系数 指数：\n");
	p2=creat_list(4);
	printf("\n第一个多项式每项的系数 指数：\n");
	print(p1);
	printf("\n第二个多项书每项的系数 指数：\n");
	print(p2);
	printf("\n两个多项式相加后的结果为：\n");
	sum_list(p1,p2);
	return 0;
}
/*
9 12
15 8
3 2
26 19
-4 8
-13 6
82 0
*/ 
~~~

## 链式存储方式

~~~c
#include <stdio.h>
#include <stdlib.h>
#define one 3
#define two 4
struct xiang
	{
		int xi;
		int zhi;
		struct xiang *next;	
	};
typedef struct xiang xiang;
typedef xiang *pt;

pt l(int n)//创建链表和插入 
{
	pt head,s,r;//head 头节点，s 当前节点，r 上一节点
	int i;
	if((head=(pt)malloc(sizeof(xiang)))==NULL) 
	{
		printf("error!");
		return 0;
	 } //检查head是否有被分配空间 
	head->next=NULL; 				//为什么不是head=NULL **** 
	r=head;//头结点置空
	for(i=0;i<n;i++)
	{
		if((s=(pt)malloc(sizeof(xiang)))==NULL)
		{
			printf("error!");
			return 0;
		}//检查s是否有被分配空间 
		scanf("%d %d",&s->xi,&s->zhi);
		r->next=s;							//head->next又不是空指针了 **** 
		r=s;//建立链表中两两之间的联系 并将链表往后移 
	 } 
	r->next=NULL;//尾结点置空 
	return head;//指向头节点以便于查找 
}

void print(pt list)//输出链表 
{
	pt p;
	p=list->next;
	while(p!=NULL)
	{
		printf("%-8d %-8d\n",p->xi,p->zhi);
		p=p->next;
	} 
//	return list;
}

void print_(pt list1,pt list2)//多项式相加并输出 
{
	pt p1,p2;
	p1=list1->next;
	p2=list2->next;
	while(p1!=NULL||p2!=NULL)
	{
		if(p1!=NULL&&p2!=NULL)//修改了if换成if-else 
		{
			if(p1->zhi>p2->zhi)
			{
				printf("%dx^%d + ",p1->xi,p1->zhi);
				p1=p1->next;
			}
			else 
			{
				if(p1->zhi<p2->zhi)
				{
					printf("%dx^%d + ",p2->xi,p2->zhi);
					p2=p2->next;
				}
				else
				{
					if(p1->zhi==p2->zhi)
					{
						printf("%dx^%d + ",p1->xi+p2->xi,p1->zhi);
						p1=p1->next;
						p2=p2->next;
					}
				}	
			}
		}
		if(p1==NULL&&p2!=NULL)
		{
			if(p2->next==NULL)
			{
				printf("%dx^%d	",p2->xi,p2->zhi);
				p2=p2->next;
			}
			else
			{
				printf("%dx^%d + ",p2->xi,p2->zhi);
				p2=p2->next;
			}
		}
		if(p2==NULL&&p1!=NULL)
		{
			if(p1->next==NULL)
			{
				printf("%dx^%d	",p1->xi,p1->zhi);
				p1=p1->next;
			}
			else
			{
				printf("%dx^%d + ",p1->xi,p1->zhi);
				p1=p1->next;
			}
					
		}
	}
}
int main()
{
	pt l(int n);
	void print(pt list);
	void print_(pt list1,pt list2);
	
	pt l1,l2;
	printf("\n输入第一个多项式中每项的系数 指数：\n");
	l1=l(one);
	printf("\n输入第一个多项式中每项的系数 指数：\n");
	l2=l(two);
	printf("\n第一个多项式每项的系数 指数：\n");
	print(l1);
	printf("\n第二个多项书每项的系数 指数：\n");
	print(l2);
	printf("\n两个多项式相加后的结果为：\n");
	print_(l1,l2);
	return 0;
 } 
/*
9 12
15 8
3 2
26 19
-4 8
-13 6
82 0
*/ 
~~~

# 链表——创建，插入

~~~c
#include <stdio.h>
#include <stdlib.h>
#define datatype int
#define N 5
struct list
{
	datatype data;
	struct list *next;
};
typedef struct list list;
typedef list *list_p;

list_p creat_list(int n)//创造链表 并且插入链表 
{
	list_p head,r,s;//head 头结点；r 前一结点；s 现结点；
	int i;
	if((head=(list_p)malloc(sizeof(list)))==NULL)
	{
		printf("error!");
		return 0;
	 } 
	head->next=NULL;
//	head=NULL; 
	r=head;//头结点置空
	for(i=0;i<n;i++)
	{
		if((s=(list_p)malloc(sizeof(list)))==NULL)
		{
			printf("error!");
			return 0;
		}
		scanf("%d",&s->data);
		r->next=s;//建立前一结点和现结点的联系 
		r=s; //将现结点变成前一结点，以便于现结点重新赋值 
	 } 
	 r->next=NULL;//尾结点置空
	 return head; 
}

void print(list_p p1)//输出链表 
{
	list_p p;
	p=p1->next;
	while(p!=NULL)
	{
		printf("%-8d",p->data);
		p=p->next;
	}
}
int main()
{
	list_p creat_list(int n);
	void print(list_p p1);
	
	list_p no_1;
	printf("输入你想创建链表的五个数：\n");
	no_1=creat_list(N);
	printf("你所创建的链表是：\n");
	print(no_1);
	return 0;
}

~~~

