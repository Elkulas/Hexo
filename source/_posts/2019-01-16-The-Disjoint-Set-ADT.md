---
title: The Disjoint Set ADT
date: 2019-01-16 17:32:12
tags:
categories: 
- FDS
mathjax: true
description: Illustration of the Disjoint Set ADT, and a sample problem from PTA
---

# Problem

## Introduction

We have a network of computers and a list of bi-directional connections. Each of these connections allows a file transfer from one computer to another. Is it possible to send a file from any computer on the network to any other?

## Input Specification

Each input file contains one test case. For each test case, the first line contains N (2≤N≤$10^4$), the total number of computers in a network. Each computer in the network is then represented by a positive integer between 1 and N. Then in the following lines, the input is given in the format:

```
I c1 c2  
```

where `I` stands for inputting a connection between `c1`and `c2`; or

```
S
```

where `S` stands for stopping this case.

## Output Specification

For each `C` case, print in one line the word "yes" or "no" if it is possible or impossible to transfer files between `c1`and `c2`, respectively. At the end of each case, print in one line "The network is connected." if there is a path between any pair of computers; or "There are `k` components." where `k` is the number of connected components in this network.

### Sample Input 1:

```in
5
C 3 2
I 3 2
C 1 5
I 4 5
I 2 4
C 3 5
S
```

### Sample Output 1:

```out
no
no
yes
There are 2 components.
```

### Sample Input 2:

```
5
C 3 2
I 3 2
C 1 5
I 4 5
I 2 4
C 3 5
I 1 3
C 1 5
S
```

### Sample Output 2:

```
no
no
yes
yes
The network is connected.
```

## Solution

这道题就是最基本的Disjoint Set ADT，对于``I`` 便使用union，对于``C``便对两个节点分别使用``Find``，然后比较两个节点是不是有同一个root。

对于检查有几个Group，只需要在``S``之后检查所有点下的-1数量。只有一个root说明全连接，有几个root便是说明有多少个components。

## AD Code

```c
#define _CRT_SECURE_NO_WARNINGS
#include "stdio.h"

#define MAX_NUM 10001

void Compare(int S[], int a, int b);
void Connect(int S[], int a, int b);
void Setunion(int S[],int a, int b);
int Find(int S[], int X);

int main()
{
	int N;
	int S[MAX_NUM];
	int i;
	char str;
	int node1, node2;
	int maxcount = 0;

	// input the total number
	scanf("%d", &N);
	// initialization
	for (i = 0; i < N + 1; i++)
		S[i] = -1;
	while (1)
	{
		scanf("%c", &str);
		// getting the Stop signal
		if (str == 'S')
		{
			//printf("\n");
			break;
		}
		if (str == 'C')
		{
			scanf("%d %d", &node1, &node2);
			Compare(S, node1, node2);
		}

		if (str == 'I')
		{
			scanf("%d %d", &node1, &node2);
			Connect(S, node1, node2);
		}
	}

	// 
	for (i = 1; i < N + 1; i++)
	{
		if (S[i] == -1)
			maxcount++;
	}
	if (maxcount == 1)
		printf("The network is connected.");
	else
		printf("There are %d components.", maxcount);
	system("pause");
}

void Setunion(int S[], int a, int b)
{
	S[b] = a;
}

int Find(int S[], int X)
{
	for (; S[X] > 0; X = S[X]);
	return X;
}

void Connect(int S[], int a, int b)
{
	Setunion(S, Find(S, a), Find(S, b));
}

void Compare(int S[], int a, int b)
{
	if (Find(S, a) == Find(S, b))
		printf("yes\n");
	else
		printf("no\n");
}
```

