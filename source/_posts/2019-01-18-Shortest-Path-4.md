---
title: 'Shortest Path [4]'
date: 2019-01-18 19:20:16
tags:
categories: 
- FDS
mathjax: true
description: Illustration of Shortest Path Algorithm, and a sample problem from PTA
---

## Problem

### Introduction

Write a program to find the weighted shortest distances from any vertex to a given source vertex in a digraph. If there is more than one minimum path from v to w, a path with the fewest number of edges is chosen. It is guaranteed that all the weights are positive and such a path is unique for any vertex.

### Format of functions:

```c++
void ShortestDist( MGraph Graph, int dist[], int path[], Vertex S );
```

where `MGraph` is defined as the following:

```c++
typedef struct GNode *PtrToGNode;
struct GNode{
    int Nv;
    int Ne;
    WeightType G[MaxVertexNum][MaxVertexNum];
};
typedef PtrToGNode MGraph;
```

The shortest distance from `V` to the source `S` is supposed to be stored in `dist[V]`. If `V` cannot be reached from `S`, store -1 instead. If `W` is the vertex being visited right before `V` along the shortest path from `S`to `V`, then `path[V]=W`. If `V` cannot be reached from `S`, `path[V]=-1`, and we have `path[S]=-1`.

### Sample program of judge:

```c++
#include <stdio.h>
#include <stdlib.h>

typedef enum {false, true} bool;
#define INFINITY 1000000
#define MaxVertexNum 10  /* maximum number of vertices */
typedef int Vertex;      /* vertices are numbered from 0 to MaxVertexNum-1 */
typedef int WeightType;

typedef struct GNode *PtrToGNode;
struct GNode{
    int Nv;
    int Ne;
    WeightType G[MaxVertexNum][MaxVertexNum];
};
typedef PtrToGNode MGraph;

MGraph ReadG(); /* details omitted */

void ShortestDist( MGraph Graph, int dist[], int path[], Vertex S );

int main()
{
    int dist[MaxVertexNum], path[MaxVertexNum];
    Vertex S, V;
    MGraph G = ReadG();

    scanf("%d", &S);
    ShortestDist( G, dist, path, S );

    for ( V=0; V<G->Nv; V++ )
        printf("%d ", dist[V]);
    printf("\n");
    for ( V=0; V<G->Nv; V++ )
        printf("%d ", path[V]);
    printf("\n");

    return 0;
}

/* Your function will be put here */
```

### Sample Input (for the graph shown in the figure):

![](2019-01-18-Shortest-Path-4\Snipaste_2019-01-18_19-22-39.png)

```
8 11
0 4 5
0 7 10
1 7 40
3 0 40
3 1 20
3 2 100
3 7 70
4 7 5
6 2 1
7 5 3
7 2 50
3
```

### Sample Output:

```out
40 20 100 0 45 53 -1 50 
3 3 3 -1 0 7 -1 0 
```



### Solution



### AD Code

```c++
void ShortestDist(MGraph Graph, int dist[], int path[], Vertex S)
{
	// 定义
	int i;
	int known[MaxVertexNum]; // 记录是否遍历的数组
	int pathlength[MaxVertexNum]; // 记录路径长度的数组

	// 初始化
	for (i = 0; i < Graph->Nv; i++)
	{
		known[i] = 0;
        // 由于函数外未定义dist[]与path[]所以在此初始化
		dist[i] = Graph->G[S][i];
		path[i] = S;
		pathlength[i] = INFINITY;
	}
	// origin
	path[S] = -1;
	dist[S] = 0;
	known[S] = 1;

	while (1)
	{
		int min = INFINITY;
		int v = -1;
		// 寻找距离最小的点以及路径最短的点
        // v = smallest unknown distence vertex
		for (i = 0; i < Graph->Nv; i++)
		{
			if(known[i] == 0)
				if (dist[i] < min)
				{
					min = dist[i];
					v = i;
				}

		}
		// 遍历结束未找到，跳出
		if (v == -1)
			break;

		known[v] = 1;
		
		// 遍历v连接点，若distv+weight更短则进行更新

		for (i = 0; i < Graph->Nv; i++)
		{
			if (known[i] == 0 && dist[v] + Graph->G[v][i] < dist[i])
			{
				dist[i] = dist[v] + Graph->G[v][i];
				path[i] = v;
			}
		}
	}
	// 按照题目需求，将不连结的vertex输出-1
	for (i = 0; i < Graph->Nv; i++)
	{  
		if (dist[i] == INFINITY)
		{
			dist[i] = -1;
			path[i] = -1;
		}
	}
}
```

