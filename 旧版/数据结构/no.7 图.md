## 邻接矩阵

~~~c
//谢玲红
//2020/11/24
//邻接矩阵 (类似于顺序存储） 
#include <stdio.h>
#include <stdlib.h>
#define infinity 65535//无权值设为双字节无符号整数的最大值65535 
#define MaxVertexNum 1000
/*	
	Weight 边的权值
	VertexData 顶点数据
	Adjacency Matrix 邻接矩阵
	Graph=(V,E) 图=(顶点，边) 
	G 矩阵
	Vertex 顶点
*/
typedef int WeightType;
typedef int VertexDataType;
typedef int Vertex;
struct MGraph//邻接矩阵图包含了顶点数，边数，权重，顶点数据 
{
	int Nv;
	int Ne;
	WeightType G[MaxVertexNum][MaxVertexNum];//这个是有向边 
	VertexDataType data[MaxVertexNum];
};
typedef struct MGraph MGraph;
struct Edge//边包含了两个端点，权重数据 
{
		Vertex V1;
		Vertex V2;
		WeightType weight;
};
typedef struct Edge Edge; 

MGraph* CreatGraph(int Nv)//创建一个没有边只有顶点的空图 
{
	MGraph* Graph=(MGraph*)malloc(sizeof(MGraph));
	int v1,v2;
	Graph->Nv=Nv;
	Graph->Ne=0;
	for(v1=0;v1<Nv;v1++)
	{
		for(v2=0;v2<Nv;v2++)
		{
			Graph->G[v1][v2]=infinity;
		}
	}
	return Graph;
}

MGraph* insertGraph()
{
	int Nv;
	int e,v;
	MGraph* Graph;
	/* 顶点 图 */ 
	scanf("%d",&Nv);
	Graph=CreatGraph(Nv);
	/* 边 权重 */ 
	scanf("%d",&Graph->Ne);
	if(!Graph->Ne)//可能出现没有边的情况 
	{
		Edge* E=(Edge*)malloc(sizeof(Edge));
		for(e=0;e<Graph->Ne;e++)
		{
			scanf("%d %d %d ",&E->V1,&E->V2,&E->weight);
			Graph->G[E->V1][E->V2]=E->weight;
			Graph->G[E->V2][E->V1]=E->weight;//如果是无向图需要加上这语句
		}
	}
	/* 顶点数据 */ 
	for(v=0;v<Graph->Nv;v++) scanf("%d",&Graph->data[v]);
	return Graph; 
} 

int main()
{
	;
}
~~~

