---
title: Topological Sort
date: 2019-01-18 16:47:00
tags:
categories: 
- FDS
mathjax: true
description: Illustration of the topological sort, and a sample problem from PTA
---

# Problem

## Introduction

Write a program to find the topological order in a digraph.

## Format of functions:

```c++
bool TopSort( LGraph Graph, Vertex TopOrder[] );
```

where `LGraph` is defined as the following:

```c++
typedef struct AdjVNode *PtrToAdjVNode; 
struct AdjVNode{
    Vertex AdjV;
    PtrToAdjVNode Next;
};

typedef struct Vnode{
    PtrToAdjVNode FirstEdge;
} AdjList[MaxVertexNum];

typedef struct GNode *PtrToGNode;
struct GNode{  
    int Nv;
    int Ne;
    AdjList G;
};
typedef PtrToGNode LGraph;
```

The topological order is supposed to be stored in `TopOrder[]` where `TopOrder[i]` is the `i`-th vertex in the resulting sequence. The topological sort cannot be successful if there is a cycle in the graph -- in that case `TopSort` must return `false`; otherwise return `true`.

Notice that the topological order might not be unique, but the judge's input guarantees the uniqueness of the result.

## Sample program of judge:

```c++
#include <stdio.h>
#include <stdlib.h>

typedef enum {false, true} bool;
#define MaxVertexNum 10  /* maximum number of vertices */
typedef int Vertex;      /* vertices are numbered from 0 to MaxVertexNum-1 */

typedef struct AdjVNode *PtrToAdjVNode; 
struct AdjVNode{
    Vertex AdjV;
    PtrToAdjVNode Next;
};

typedef struct Vnode{
    PtrToAdjVNode FirstEdge;
} AdjList[MaxVertexNum];

typedef struct GNode *PtrToGNode;
struct GNode{  
    int Nv;
    int Ne;
    AdjList G;
};
typedef PtrToGNode LGraph;

LGraph ReadG(); /* details omitted */

bool TopSort( LGraph Graph, Vertex TopOrder[] );

int main()
{
    int i;
    Vertex TopOrder[MaxVertexNum];
    LGraph G = ReadG();

    if ( TopSort(G, TopOrder)==true )
        for ( i=0; i<G->Nv; i++ )
            printf("%d ", TopOrder[i]);
    else
        printf("ERROR");
    printf("\n");

    return 0;
}

/* Your function will be put here */
```

### Sample Input 1 (for the graph shown in the figure):


![](2019-01-18-Topological-Sort\Snipaste_2019-01-18_16-51-58.png)

```
5 7
1 0
4 3
2 1
2 0
3 2
4 1
4 2
```

### Sample Output 1:

```out
4 3 2 1 0 
```

### Sample Input 2 (for the graph shown in the figure):

![](2019-01-18-Topological-Sort\Snipaste_2019-01-18_16-51-44.png)

```
5 8
0 3
1 0
4 3
2 1
2 0
3 2
4 1
4 2
```

### Sample Output 2:

```
ERROR
```



## Solution

属于简单的topological sort，使用队列来简化时间复杂度。对于成环的检测需要在最后判断计数和顶点的总数是否一致。若不一致则说明有顶点没有遍历，存在成环现象。

同时还需要注意本题中图的定义。本题中图的定义是使用Adjust List来进行定义的，所以在初始计算入度的时候需要搞清楚图的定义方式。

## AD Code

```c
bool TopSort(LGraph Graph, Vertex TopOrder[])
{	
	int indegree[MaxVertexNum];
	int i;
	int number = Graph->Nv;
	int counter = 0;
	Vertex Tpnode;
	Vertex Q[MaxVertexNum];
	int head = 0, tail = 0;
	PtrToAdjVNode T;
	
    // 入度矩阵初始化
	for (i = 0; i < MaxVertexNum; i++)
		indegree[i] = 0;
	// 遍历所有图中点，将各个点的入度数据记录到数组
	for (i = 0; i < number; i++)
	{
		T = Graph->G[i].FirstEdge;
		while (T)
		{
			indegree[T->AdjV] ++; //入度增加
			T = T->Next;
		}
	}
	
    // 将起始点也就是入度为零的点进队列
	for (i = 0; i < number; i++)
	{
		if (indegree[i] == 0)
			Q[tail++] = i;
	}
	
    // 判断队列是否为空来控制输出
	while (tail > head)
	{
		// 将队列中当前顶点出列
        Tpnode = Q[head++];
		TopOrder[counter++] = Tpnode;
		T = Graph->G[Tpnode].FirstEdge;
        // 遍历出列点的临近点
		while (T)
		{
			indegree[T->AdjV]--; // 删除的点的临点入度-1；
			if (indegree[T->AdjV] == 0)
				Q[tail++] = T->AdjV; // 零点入队
			T = T->Next; // 遍历所有临点
		}
	}
	// 判断回环
	if (counter != number)
		return false;
	else
		return true;
}
```

