[toc]

# 最大子列和问题

## 算法1：直接法

~~~c
#include <stdio.h>
#define list_size 6
int maxsubseqsum1(int list[],int n);
void scanf_date(int list[],int n);
int main()
{
	int list[list_size];
	scanf_date(list,list_size);
	printf("最大和:%d",maxsubseqsum1(list,list_size));
 }

int maxsubseqsum1(int list[],int n)
{
	int i,j,k,l;
	int thissum,maxsum=0;
	for(i=0;i<n;i++)
	{
		for(j=i;j<=n;j++)
		{
			thissum=0;
			for(k=i;k<=j;k++)
			{
				thissum+=list[k];
			}
			if(thissum>maxsum)
			{
				maxsum=thissum;
			}
		}
	}
	return maxsum;
}
void scanf_date(int list[],int n)
{
	int i;
	printf("请输入六位整型数：");
	for(i=0;i<list_size;i++)
	{
		scanf("%d",&list[i]); 	
	}

}
~~~

## 算法2：部分存储中间值的穷举

~~~c
#include <stdio.h>
#define list_size 6
int maxsubseqsum1(int list[],int n);
void scanf_date(int list[],int n);
int main()
{
	int list[list_size];
	scanf_date(list,list_size);
	printf("最大和:%d",maxsubseqsum1(list,list_size));
 }

int maxsubseqsum1(int list[],int n)
{
	int i,j,k;
	int thissum,maxsum=0;
	for(i=0;i<n;i++)
	{
		thissum=0;
		for(j=i;j<n;j++)
		{
			thissum+=list[j];
			if(thissum>maxsum)
			{
				maxsum=thissum;
			}
		}
		
	}
	return maxsum;
}
void scanf_date(int list[],int n)
{
	int i;
	printf("请输入六位整型数：");
	for(i=0;i<list_size;i++)
	{
		scanf("%d",&list[i]); 	
	}

}
~~~

## 算法3：分而治之

~~~c
#include <stdio.h>
#define list_size 6
void scanf_date(int list[],int n);
int maxsubseqsum3(int list[],int n);
int divideandconquer(int list[],int left,int right);
int maxsum(int a,int b,int c);

int main()
{
	int list[list_size];
	scanf_date(list,list_size);
	printf("最大和为：%d",maxsubseqsum3(list,list_size));
}

void scanf_date(int list[],int n)
{
	int i;
	printf("输入六个整形数：");
	for(i=0;i<list_size;i++)
	{
		scanf("%d",&list[i]);
	 } 
}

int maxsubseqsum3(int list[],int n)
{
	return divideandconquer(list,0,list_size-1);
}

int divideandconquer(int list[],int left,int right)
{
	int maxleftsum,maxrightsum;
	int leftbordersum,rightbordersum;
	int maxleftbordersum,maxrightbordersum;
	int center,i;
/*	if(left==right)											//着重复习和思考 
	{
		if(list[left]>0) return list[left];
		else return 0;
	}
	center=(left+right)/2;
	maxleftsum=divideandconquer(list,left,center);
	maxrightsum=divideandconquer(list,center+1,right);
	leftbordersum=maxleftbordersum=0;
	for(i=center;i>=left;i--)
	{
		leftbordersum+=list[i];
		if(leftbordersum>maxleftbordersum)
		{
			maxleftbordersum=leftbordersum;
		}
	}*/
	rightbordersum=maxrightbordersum=0;
	for(i=center+1;i<=right;i++)
	{
		rightbordersum+=list[i];
		if(rightbordersum>maxrightbordersum)
		{
			maxrightbordersum=rightbordersum;
		}
	}
	return maxsum(maxleftbordersum,maxrightbordersum,maxleftbordersum+maxrightbordersum);
}

int maxsum(int a,int b,int c)
{
	return a>b?a>c?a:c:b>c?b:c;
}
~~~

## 算法4：在线处理

~~~c
#include <stdio.h>
#define list_size 6

void scanf_date(int list[],int n);
int maxsubseqsum4(int list[],int n);

int main()
{
	int list[list_size];
	scanf_date(list,list_size);
	printf("最大和为：%d",maxsubseqsum4(list,list_size));
 } 
 
void scanf_date(int list[],int n)
 {
	int i;
	printf("请输入六位整数型：");
	for(i=0;i<n;i++)
	{
		scanf("%d",&list[i]);
	 } 
 }
int maxsubseqsum4(int list[],int n)
 {
	int i;
	int thissum=0,maxsum=0;
	for(i=0;i<=n;i++)
	{
		thissum+=list[i];
		if(thissum>maxsum) maxsum=thissum;
		else if(thissum<0) thissum=0;
	}
	return maxsum;
 }
~~~

