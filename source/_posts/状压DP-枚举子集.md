---
title: 状压DP-枚举子集
date: 2020-2-21 21:41:35
mathjax: true
tags:
	- OI
	- DP
	- 题解
categories:
	- OI
	- 题解
	- UVA
---

鉴于状压DP真的没什么好说的，我们还是先看题目。


<!--more-->

**例1**.[UVA 11825][1]  （[原题PDF下载][2]）

### 题目描述

Miracle Corporations has a number of system services running in a distributed computer system which is a prime target for hackers. The system is basically a set of $N$ computer nodes with each of them running a set of $N$ services. Note that, the set of services running on every node is same everywhere in the network. A hacker can destroy a service by running a specialized exploit for that service in all the nodes. 

One day, a smart hacker collects necessary exploits for all these $N$ services and launches an attack on the system. He ﬁnds a security hole that gives him just enough time to run a single exploit in each computer. These exploits have the characteristic that, its successfully infects the computer where it was originally run and all the neighbor computers of that node.

Given a network description, ﬁnd the maximum number of services that the hacker can damage.

### 输入格式

There will be **multiple test cases** in the input ﬁle. A test case begins with an integer $N (1\leq N\leq 16)$, the number of nodes in the network. The nodes are denoted by $0$ to  N−1. Each of the following $N$ lines describes the neighbors of a node. Line $i(0 \leq i <N)$ represents the description of node $i$. The description for node $i$ starts with an integer $m$ (Number of neighbors for node $i$), followed by $m$ integers in the range of $0$ to N−1, each denoting a neighboring node of node $i$.

The end of input will be denoted by a case with $N=0$. This case should not be processed.

### 输出格式

For each test case, print a line in the format, ‘Case X: Y ’, where $X$ is the case number & $Y$ is the maximum possible number of services that can be damaged.

### 样例数据

input
```
3
2 1 2
2 0 2
2 0 1
4
1 1
1 0
1 3
1 2
0
```

output
```
Case 1: 3
Case 2: 2
```

### 题目翻译

在一个计算机网络中，有n台计算机（分别以$0-(n-1)$编号），且给出网络的联通情况。共有$n(n\leq 16)$种服务，每个计算机都运行着这$n$个服务。假如你是一个黑客，你可以侵入任意一台计算机，并且终止这台计算机的任意一项服务。此时这台计算机所直接联通的所有计算机的这一项服务都会被终止。每台电脑只能被入侵一次，每一次只能终止一项服务，求最多能使多少服务瘫痪。（服务瘫痪：这个服务在所有的计算机上都被终止）。

有**多组测试数据**，对于每组数据，第一行一个数$n$，表示计算机数和服务数。接下来$n$行每行一个数$k$，表示第$i$台电脑连接的电脑数为$k$，然后$k$个数表示连接的电脑的编号。对于第$i$组数据，输出格式为：Case "数据组数": "最多能使多少台服务瘫痪"。

### 思路分析

首先，因为要使瘫痪的服务数尽量的多，所以我们考虑使想要终止的服务尽量瘫痪。假设我们终止了第i台计算机的第j个服务，那么它周围一圈的计算机的这个服务都会被终止。因为要使这个服务瘫痪，所以要使所有计算机中除了这个服务终止了的所有计算机这项服务都瘫痪。因为要使终止的服务数最多，所以终止这个服务的计算机尽可能的小。换一种方式，我们假设第i台计算机连着的计算机（包括自己）的几何为$p[i]$，全集$U$为所有计算机，所以我们要使$\complement p[i]$里的计算机的这项服务终止。但是因为终止一个计算机的一项服务可能会影响到其他计算机的这项服务，所以$\complement p[i]$一定是终止这项服务其他的计算机（假定编号为x），也就是p[x]的子集。那么我们要尽可能使$\complement p[i]$和p[x]的非公共元素最少。

为了方便表示集合，我们将集合状压，也就是p[i]这个二进制数的第$j$位（从右往左，$j$从0开始）若为1表示第$i$台计算机与第$j$台计算机相连。所以我们要枚举全集$U$的子集，使得其中一些子集（不重复）的并集为全集。为了使答最优，我们要使得子集的个数尽量少。

所以现在问题变成了：给出全集$U$，求这$n$个集合最多分成多少组，使得每一组集合的并集是全集。

我们开始设计状态：令dp[i]表示状态为$i$（i为一个二进制数，为1的位的几何表示这几台电脑分为一组）时分组。

[1]: https://www.luogu.com.cn/problem/UVA11825
[2]: https://onlinejudge.org/external/118/p11825.pdf