[toc]

# 线性表

## 顺序表——建立，插入，删除，查找

~~~c
//谢玲红
//2020.10.20 
//线性表顺序表
#include <stdio.h>
#include <stdlib.h>
#define ElementType int
#define mixsize 100
struct list_shunxu
{
	ElementType data[mixsize];
	int size;
};
typedef list_shunxu list;

int main()
{
	list *Makelist(int n);
	bool Insert(list *p,int i,ElementType n);
	bool Delete(list *p,int i);
	ElementType Find(list * p,ElementType n);
	void print(list *p);
	
	list *p=NULL;
	printf("\n你想创建的线性表的5个整数：\n"); 
	p=Makelist(5);
	printf("\n线性表的5个整数为：\n");
	print(p);
	printf("\n在线性表中第二个数据插入数据5：\n"); 
	Insert(p,2,5);
	print(p);
	printf("\n在线性表中删除第三个数据：\n");
	Delete(p,3);
	print(p);
	printf("\n在线性表中数据3所在的位置是：\n");
	printf("%d",Find(p,3));
	return 0;
 } 
 
list *Makelist(int n)//建立一个表 
{
	list *p; 
	int i;
	p=(list *)malloc(sizeof(list));
	p->size=n;	
	for(i=0;i<p->size;i++) scanf("%d",&p->data[i]);
	return p;
}

bool Insert(list *p,int i,ElementType n)//插入数据 ，在p表中的第i个数据插入数据n
{
	int j;
	if(i<1||i>p->size+1)//error:if(i<1||i>p->size)
	{
		printf("insert number was error!");
		return false;//用布尔型返回结果 
	}
	if(p->size==mixsize)//error:if(p->size>mixsize)
	{
		printf("list length was error!");
		return false;
	}
	for(j=p->size;j>=i;j--)	p->data[j]=p->data[j-1];
	p->data[i-1]=n;
	p->size++;//error:表长增加 
	return false;//用布尔型返回结果 
} 

bool Delete(list *p,int i)//删除数据，删除在p表中第i个数据 
{
	int j;
	if(i<1||i>p->size) 
	{
		printf("detele number was error!");
		return false;
	}
	for(j=i-1;j<p->size;j++) p->data[j]=p->data[j+1];
	p->size--;
	return true;
}

ElementType Find(list * p,ElementType n)//查找数据，查找p表中的数据n
{
	int j;
	for(j=1;j<=p->size;j++)	if(p->data[j-1]==n) return j;
	return NULL;
}

void print(list *p)
{
	int i;
	for(i=0;i<p->size;i++) printf("%d ",p->data[i]);
	printf("\n");
}
~~~

## 单向链表——创建，插入，删除，查找，求长度

~~~c
//谢玲红
//2020.10.20 
//线性表单向链表 
#include <stdio.h>
#include <stdlib.h>
#define ElementType int 
struct list_lianbiao
{
	ElementType data;
	struct list_lianbiao* next;
};
typedef struct list_lianbiao list;

int main()
{
	list* Creatlist(int n);
	int Lenth(list* p);
	bool Insert(list* p, int i, ElementType n); 
	bool Detele(list *p,int i);
	int Find_number(list* p, ElementType n);
	ElementType Find_data(list *p,int i);
	void print(list *p);
	
	list *p;
	printf("\n你所要创建的线性表的5个数据：\n");
	p=Creatlist(5);
	printf("\n线性表的5个数据是：\n");
	print(p);
	printf("\n线性表的长度为：\n");
	printf("%d \n",Lenth(p));
	printf("\n在线性表中第一个位置插入数据5\n");
	Insert(p,1,5);
	print(p);
	printf("\n在线性表中删除第3个数据\n");
	Detele(p,3);
	print(p);
	printf("\n在线性表中数据4的位置：\n");
	printf("%d \n",Find_number(p,4));
	printf("\n在线性表中第2个位置上的数据是：\n");
	printf("%d \n",Find_data(p,2));
}

list* Creatlist(int n)//创建新表，有n个结点
{
	int i; 
	list* head, * r, * s;
	if ((head = (list*)malloc(sizeof(list))) == NULL) printf("error!");
	head->next = NULL;//头指针置空 
	r = head;
	for (i=0;i<n;i++)
	{
		if ((s = (list*)malloc(sizeof(list))) == NULL) printf("error!");
		scanf("%d", &s->data);
		r->next = s;
		r = s;
	}
	r->next = NULL;
	return head;
}

int Lenth(list* p)//求表长
{
	int count=0;
	list* pt;
	pt = p->next;
	while (pt != NULL)
	{
		pt = pt->next;
		count++;
	}
	return count;
}

bool Insert(list* p, int i, ElementType n)//插入数据，在p表的定位i上插入数据n
{
	list* pt, * s;
	int j = 0;
	pt = p;
	while (pt != NULL && j < i - 1 )//定位在i-1的结点 此时i没有给定范围
	{
		pt = pt->next;
		j++;
	}
//	if(pt==NULL)
//	if(j!=i-1)
	if (pt == NULL || j != i - 1)//我认为pt==NULL和j!=i-1是等价的【疑问】
	{
		printf("error!");
		return false;
	}
	else
	{
		s = (list*)malloc(sizeof(list));
		s->data = n;
		s->next = pt->next;
		pt->next = s;
		return true;
	}
}

bool Detele(list *p,int i)//删除数据，删除数据n
{
	list* pre,*s;
	int j=0;
	pre = p;
	while (pre != NULL && j < i - 1)
	{
		pre = pre->next;
		j++;
	}
	if (pre == NULL || j != i - 1 || pre->next == NULL)//判断条件，就是有点迷
	{
		printf("error!");
		return false;
	}
	else
	{
		s = pre->next;
		pre->next = s->next;
		free(s);
		return true;
	}
 } 

int Find_number(list* p, ElementType n)//查找p表中的数据n所在位置 
{
	int count=0;
	list* pt;
	pt = p;
	while (pt != NULL && pt->data != n) 
	{
		pt = pt->next;
		count++;
	}
	if (pt != NULL) return count;
	else return NULL;
}

ElementType Find_data(list *p,int i)//查找p表中的第i个位置的数据
{
	int j=0;
	list *pt;
	pt=p;
	while(pt!=NULL&&j<i) 
	{
		pt=pt->next;
		j++;
	}
	if(pt!=NULL) return pt->data;
	else return NULL;
 } 

void print(list *p)
{
	list *pt;
	pt=p->next;
	while(pt!=NULL)
	{
		printf("%d ",pt->data);
		pt=pt->next;
	}
	printf("\n");
}
~~~

## 循环链表——创建，查找，删除，插入

~~~c
//谢玲红
//2020.10.22
//线性表循环链表 
#include <stdio.h>
#include <stdlib.h>
#define ElementType int 
struct list_lianbiao
{
	ElementType data;
	struct list_lianbiao* next;
};
typedef struct list_lianbiao list;

int main()
{
	list* Creatlist(int n);
	int Lenth(list* p);
	bool Insert(list* p, int i, ElementType n); 
	bool Detele(list *p,int i);
	int Find_number(list* p, ElementType n);
	ElementType Find_data(list *p,int i);
	void print(list *p);
	
	list *p;
	printf("\n你所要创建的循环线性表的5个数据：\n");
	p=Creatlist(5);
	printf("\n循环线性表的5个数据循环一遍是：\n");
	print(p);
	printf("\n循环线性表的长度为：\n");
	printf("%d \n",Lenth(p));
	printf("\n在循环线性表中第一个位置插入数据5后，前十个数据是：\n");
	Insert(p,1,5);
	print(p);
	printf("\n在循环线性表中删除第3个数据\n");
	Detele(p,3);
	print(p);
	printf("\n在循环线性表中数据4的位置：\n");
	printf("%d \n",Find_number(p,4));
	printf("\n在循环线性表中第2个位置上的数据是：\n");
	printf("%d \n",Find_data(p,2));

}

list* Creatlist(int n)//创建新表，有n个结点
{
	int i; 
	list* head, * r, * s, * tail;
	if ((head = (list*)malloc(sizeof(list))) == NULL) printf("error!");	
	head->next=NULL;		//头指针置空   ***	
	r=head;				

	for (i=0;i<n;i++)
	{
		if ((s = (list*)malloc(sizeof(list))) == NULL) printf("error!");
		scanf("%d", &s->data);
		r->next=s; 
		r=s;
	} 
	r->next=head->next;		//实现头尾循环 *** 
	return head;
}

int Lenth(list* p)//求表长
{
	int count=1;
	list* pt;
	pt = p->next->next;
	while (pt != p->next)
	{
		count++;
		pt = pt->next;
	}
	return count;
}

bool Insert(list* p, int i, ElementType n)//插入数据，在p表的定位i上插入数据n
{
	list* pt, * s;
	int j = 0;
	pt = p;
	while ( j < i - 1 )//定位在i-1的结点 此时i没有给定范围
	{
		pt = pt->next;
		j++;
	}
//	if(pt==NULL)
//	if(j!=i-1)
	if (j != i - 1)//我认为pt==NULL和j!=i-1是等价的【疑问】
	{
		printf("error!");
		return false;
	}
	else
	{
		s = (list*)malloc(sizeof(list));
		s->data = n;
		s->next = pt->next;
		pt->next = s;
		return true;
	}
}

bool Detele(list *p,int i)//删除数据，删除数据n
{
	list* pre,*s;
	int j=0;
	pre = p;
	while (pre != NULL && j < i - 1)
	{
		pre = pre->next;
		j++;
	}
	if (pre == NULL || j != i - 1 || pre->next == NULL)//判断条件，就是有点迷
	{
		printf("error!");
		return false;
	}
	else
	{
		s = pre->next;
		pre->next = s->next;
		free(s);
		return true;
	}
 } 

int Find_number(list* p, ElementType n)//查找p表中的数据n所在位置 
{
	int count=0;
	list* pt;
	pt = p;
	while (pt != NULL && pt->data != n) 
	{
		pt = pt->next;
		count++;
	}
	if (pt != NULL) return count;
	else return NULL;
}

ElementType Find_data(list *p,int i)//查找p表中的第i个位置的数据
{
	int j=0;
	list *pt;
	pt=p;
	while(pt!=NULL&&j<i) 
	{
		pt=pt->next;
		j++;
	}
	if(pt!=NULL) return pt->data;
	else return NULL;
 } 

void print(list *p)
{
	int i;
	list *pt;
	pt=p->next;
	for(i=0;i<10;i++)
	{
		printf("%d ",pt->data);
		pt=pt->next;
	}
	printf("\n");
}
~~~

## 双向链表——创建，查找，删除，插入

~~~c
//谢玲红
//2020.10.22
//线性表双向链表 
#include <stdio.h>
#include <stdlib.h>
#define ElementType int 
struct list_lianbiao
{
	ElementType data;
	struct list_lianbiao* next;
	struct list_lianbiao* previous;
};
typedef struct list_lianbiao list;

int main()
{
	list* Creatlist(int n);
	int Lenth(list* p);
	bool Insert(list* p, int i, ElementType n); 
	bool Detele(list *p,int i);
	int Find_number(list* p, ElementType n);
	ElementType Find_data(list *p,int i);
	void print(list *p);
	
	list *p;
	printf("\n你所要创建的双向线性表的5个数据：\n");
	p=Creatlist(5);
	print(p);
	printf("\n双向线性表的长度为：\n");
	printf("%d \n",Lenth(p));
	printf("\n在双向线性表中第一个位置插入数据8\n");
	Insert(p,1,8);
	print(p);
	printf("\n在双向线性表中删除第3个数据\n");
	Detele(p,3);
	print(p);
	printf("\n在双向线性表中数据4的位置：\n");
	printf("%d \n",Find_number(p,4));
	printf("\n在双向线性表中第2个位置上的数据是：\n");
	printf("%d \n",Find_data(p,2));

}

list* Creatlist(int n)//创建新表，有n个结点
{
	int i;
	list* head, * r, * s;
	if ((head = (list*)malloc(sizeof(list))) == NULL) printf("error!");
	head->previous=NULL;		//头指针置空   
	head->next=NULL;			
	r=head;						//有头结点 
	for (i=0;i<n;i++)
	{
		if ((s = (list*)malloc(sizeof(list))) == NULL) printf("error!");
		scanf("%d", &s->data);
		s->previous=r;		//双向 
		r->next=s;
		r=s;
	} 
	r->next=NULL;
	return head;
}

int Lenth(list* p)//求表长
{
	int count=0;
	list* pt;
	pt = p->next;
	while (pt!=NULL)
	{
		count++;
		pt = pt->next;
	}
	return count;
}

bool Insert(list* p, int i, ElementType n)//插入数据，在p表的定位i上插入数据n
{
	list* pt, * s;
	int j = 0;		//零界点多思考和下面的判断多对比 
	pt = p;
	while ( pt!=NULL && j < i - 1 )//定位在i-1的结点 此时i没有给定范围
	{
		pt = pt->next;
		j++;
	}
//	if(pt==NULL)
//	if(j!=i-1)
	if ( pt==NULL || j != i - 1)//我认为pt==NULL和j!=i-1是等价的【疑问】
	{
		printf("error!");
		return false;
	}
	else
	{
		s = (list*)malloc(sizeof(list));
		s->data = n;
		s->previous=pt;		//插入数据 
		s->next=pt->next;
		pt->next->previous=s;
		pt->next=s;
		return true;
	}
}

bool Detele(list *p,int i)	//删除数据，删除数据n
{
	list* pre,*s;
	int j=0;
	pre = p;
	while ( pre!=NULL&&j < i - 1)
	{
		j++;
		pre = pre->next;
	}
	if (pre == NULL || j != i - 1 || pre->next == NULL)//判断条件，就是有点迷
	{
		printf("error!");
		return false;
	}
	else
	{
		s=pre->next;		//删除数据 
		pre->next=s->next;
		s->next->previous=pre;
		free(s);
		return true;
	}
 } 

int Find_number(list* p, ElementType n)//查找p表中的数据n所在位置 
{
	int count=1;
	list* pt;
	pt = p->next;
	while (pt != NULL && pt->data != n) 
	{
		pt = pt->next;
		count++;
	}
	if (pt != NULL) return count;
	else return NULL;
}

ElementType Find_data(list *p,int i)//查找p表中的第i个位置的数据
{
	int j=1;
	list *pt;
	pt=p->next;
	while(pt!=NULL&&j<i) 
	{
		pt=pt->next;
		j++;
	}
	if(pt!=NULL) return pt->data;
	else return NULL;
 } 

void print(list *p)
{
	int i;
	list *pt;
	pt=p->next;
	printf("\n顺序：\n");
	while(pt!=NULL) 
	{
		printf("%d ",pt->data);
		if(pt->next==NULL) break;
		pt=pt->next;
		
	}
	printf("\n");
	printf("\n逆序：\n");
	while(pt!=p)
	{
		printf("%d ",pt->data);
		pt=pt->previous;
	 } 
	 printf("\n");
}
~~~

## 循环双向链表——创建，插入，删除，查找

~~~c
//谢玲红
//2020.10.22
//线性表循环双向链表 
#include <stdio.h>
#include <stdlib.h>
#define ElementType int 
struct list_lianbiao
{
	ElementType data;
	struct list_lianbiao* next;
	struct list_lianbiao* previous;
};
typedef struct list_lianbiao list;

int main()
{
	list* Creatlist(int n);
	int Lenth(list* p);
	bool Insert(list* p, int i, ElementType n); 
	bool Detele(list *p,int i);
	int Find_number(list* p, ElementType n);
	ElementType Find_data(list *p,int i);
	void print(list *p);
	
	list *p;
	printf("\n你所要创建的双向线性表的5个数据：\n");
	p=Creatlist(5);
	print(p);
	printf("\n双向线性表的长度为：\n");
	printf("%d \n",Lenth(p));
	printf("\n在双向线性表中第一个位置插入数据8\n");
	Insert(p,1,8);
	print(p);
	printf("\n在双向线性表中删除第3个数据\n");
	Detele(p,3);
	print(p);
	printf("\n在双向线性表中数据4的位置：\n");
	printf("%d \n",Find_number(p,4));
	printf("\n在双向线性表中第2个位置上的数据是：\n");
	printf("%d \n",Find_data(p,2));

}

list* Creatlist(int n)//创建新表，有n个结点
{
	int i;
	list* head, * r, * s;
	if ((head = (list*)malloc(sizeof(list))) == NULL) printf("error!");
	head->previous=NULL;		//头指针置空   
	head->next=NULL;			
	r=head;						//有头结点 
	for (i=0;i<n;i++)
	{
		if ((s = (list*)malloc(sizeof(list))) == NULL) printf("error!");
		scanf("%d", &s->data);
		s->previous=r;		//双向 
		r->next=s;
		r=s;
	} 
	r->next=head;		//头尾循环 
	head->previous=r;
	return head;
}

int Lenth(list* p)//求表长
{
	int count=0;
	list* pt;
	pt = p->next;
	while (pt!=p)
	{
		count++;
		pt = pt->next;
	}
	return count;
}

bool Insert(list* p, int i, ElementType n)//插入数据，在p表的定位i上插入数据n
{
	list* pt, * s;
	int j = 0;
	pt = p;
	while ( j < i - 1 )//定位在i-1的结点 此时i没有给定范围
	{
		if(pt==p) 
		{
			pt=pt->next;
			continue;
		}
		pt = pt->next;
		j++;
	}
//	if(pt==NULL)
//	if(j!=i-1)
	if ( j != i - 1)//我认为pt==NULL和j!=i-1是等价的【疑问】
	{
		printf("error!");
		return false;
	}
	else
	{
		s = (list*)malloc(sizeof(list));
		s->data = n;
		s->previous=pt;		//插入数据 
		s->next=pt->next;
		pt->next->previous=s;
		pt->next=s;
		return true;
	}
}

bool Detele(list *p,int i)//删除数据，删除数据n
{
	list* pre,*s;
	int j=0;
	pre = p;
	while ( j < i - 1)
	{
		if(pre==p) 
		{
			pre=pre->next;
			continue;
		}
		j++;
		pre = pre->next;
	}
	if (pre == NULL || j != i - 1 || pre->next == NULL)//判断条件，就是有点迷
	{
		printf("error!");
		return false;
	}
	else
	{
		s=pre->next;		//删除数据 
		pre->next=s->next;
		s->next->previous=pre;
		free(s);
		return true;
	}
 } 

int Find_number(list* p, ElementType n)//查找p表中的数据n所在位置 
{
	int count=1;
	list* pt;
	pt = p;
	while (pt != NULL && pt->data != n) 
	{
		if(pt==p) 
		{
			pt=pt->next;
			continue;
		}
		pt = pt->next;
		count++;
	}
	if (pt != NULL) return count;
	else return NULL;
}

ElementType Find_data(list *p,int i)//查找p表中的第i个位置的数据
{
	int j=1;
	list *pt;
	pt=p;
	while(pt!=NULL&&j<i) 
	{
		if(pt==p)
		{
			pt=pt->next;
			continue; 
		}
		pt=pt->next;
		j++;
	}
	if(pt!=NULL) return pt->data;
	else return NULL;
 } 

void print(list *p)
{
	int i;
	list *pt;
	pt=p->next;
	printf("\n顺序：\n");
	for(i=0;i<10;i++) 
	{
		if(pt==p) pt=pt->next;
		printf("%d ",pt->data);
		pt=pt->next;
		
	}
	printf("\n");
	pt=p;
	printf("\n逆序：\n");
	for(i=0;i<10;i++)
	{
		if(pt==p) 
		{
			pt=pt->previous;
			i--;
			continue;
		}
		printf("%d ",pt->data);
		pt=pt->previous;
	 } 
	 printf("\n");
}
~~~



# 广义表(没完成)

~~~c
#include <stdio.h>
#define mixsize 20
struct list_guangyi
{
	int num;
	union rink
	{
		char group;
		char preson[mixsize];
	};
	struct list_guanyi *next;
};
typedef struct list_guanyi list;
typedef union rink rink;

int main()
{
	;
}

list *makelist(int n)//创建一个广义表，一个单位有n个部门 
{
	list *head,*r,*s;
	if((head=(list *)malloc(sizeof(list)))==NULL) 
	{
		printf("error!"); 
		return 0;
	}
	head->next=NULL;
	r=head;
	if((s=(list *)malloc(sizeof(list)))==NULL)
	{
		printf("error!");
		return 0;
	}
	else
	{
		for(i=0;i<n+1;i++)
		{
			scanf("%d",s->num);
			if(s->sum!=0)
			{
				for(i=0;i<4;i++)
				{
					scanf("%c",&s->rink->group->rink->preson);
				}
				
			}
			else scanf("%c",&s->rink->preson[mixsize]);
			r->next=s;
			r=s;
		}
		r->next=NULL;
		return head;
	}
}
~~~

