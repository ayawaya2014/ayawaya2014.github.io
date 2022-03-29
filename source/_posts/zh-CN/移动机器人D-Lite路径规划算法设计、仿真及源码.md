---
title: 移动机器人D*Lite路径规划算法设计、仿真及源码
date: 2018-03-01 17:12:06
lang: zh-CN
tags: [Robotics,Github,CSDN]
categories: 科研
katex: true
mathjax: true
---

# Dstar Lite路径规划算法简介
D\*Lite算法是Koenig S和Likhachev M基于LPA\*算法基础上提出的路径规划算法。 LPA\*算法本是基于Dijkstra最短路径算法而产生的定起点、定目标点的路径规划算法。 通过对LPA\*算法的改造，使LPA\*算法的思想能应用到诸如车辆动态导航这样的问题。 
> LPA\*算法区别于其他算法 的一个重要特点是rhs()的定义：
> $$
> rhs(s) = \left\{
>  \begin{array}{ll}
> 0 & \text{if}  s = s_{start} \\
> \text{min}_{s^{'} \in Pred(s)}(g(s^{'})+c(s^{'},s)) & \text{otherwise}
> \end{array} 
> \right.
> $$
> D\*Lite算法继承了rhs()的概念，但D\*Lite算法是从目标节点向起始节点搜索。

<!-- more -->

为了让节点v的启发函数值随着起点位置变化而变化， Koenig S和Likhachev M给出了两种方法：一是，根据新的起点位置，将优先队列中所有节点的启发函数值重新计算；二是，并不重新计算队列中的启发函数值，而是在计算新添加到优先队列中的节点的启发函数值时，加上一个修饰符 ,表示机器人移动距离的叠加。
- - -
** D\* Lite Pseudo Code: **
> ** CaculateKey(*s*) **
>> return [min(*g*(*s*),*rhs*(*s*))+h(*s*<sub>_start_</sub> , s)+k<sub>m</sub>; min(*g*(*s*),*rhs*(*s*))];
>
> __Initialize()__
> > U: =0;
> k<sub>m</sub> =0;
> for all *s* $\in$ S,  *rhs*(*s*) = *g*(*s*) = $\infty$;
> *rhs*(*s<sub>goal</sub>*) = 0;
> U.Insert(*s<sub>goal</sub>*), CaculateKey(*s<sub>goal</sub>*));

> __UpdateVertex$(\mu)$__
> >  if $(\mu \neq s_{goal})$, _rhs_$(\mu)$ = $\text{min}_{s^{'} \in Succ(\mu)}(c(\mu,s^{'})+g(s^{'}))$;
> if $(\mu \in U)$, U.Remove$(\mu)$
> if $(g(\mu) \neq rhs(\mu))$, U.Insert$(\mu, CaculateKey(\mu))$;

> **ComputeShortestPath()**
> while (U.TopKey() < CaculateKey($S_{start}$) or $rhs(s_{start}) \neq g(s_{start})$)
> > $k_{old} $ = U.TopKey();
> > $\mu$ = U.Pop();
> > if ($k_{old}$ < CaculateKey($\mu$))
> >
> > > U.Insert($\mu$, CaculateKey($\mu$));
>
> > elseif ($g(\mu) > rhs(\mu)$)
> >> $g(\mu) = rhs(\mu)$
> for all $s \in Pred(\mu)$, UpdateVertex(s);

>> else
>>> $g(\mu) = \infty$;
> for all $s \in Pred(\mu) \cup {\mu}$, UpdateVertex(s);

> **Main()**
> > $S_{last} = S_{start}$;
> > Initialize();
> > ComputeShortestPath();
> > while($S_{start} \neq S_{goal}$)
> > >>> /* if ($g(S_{start} =\infty)$) then there is no known path */
> > >>> $ S_{start} = arg \text{min}_{s^{'} \in Succ(\mu)}(c(\mu,s^{'})+g(s^{'})) $;
> > >>> Move to $ S_{start} $;
> > >>> Scan graph for changed edge costs;
> > >>> if any edge costs changed
> > >>>
> > >>> > $k_m = k_m + h(s_{last}, s_{start})$;
> > >>> $S_{last} = S_{start}$;
> > >>> for all directed edges $(u, v)$ with changed edge costs
> > >>> >> Update the edge cost $c(u, v)$;
> > >>> Update Vertex$(u)$;

>>>> Compute ShortestPath();

***
更详细的算法说明，请查阅有关文献资料。

<!-- more -->

# Linux系统简要说明
Linux是一套免费使用和自由传播的类Unix操作系统，是一个基于POSIX和UNIX的多用户、多任务、支持多线程和多CPU的操作系统。它能运行主要的UNIX工具软件、应用程序和网络协议。它支持32位和64位硬件。Linux继承了Unix以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。
在做算法程序开发之前，应对Linux系统基本操作有一定的了解，才能方便上手，在这里推荐一款教程：

> [鳥哥的 Linux 私房菜](http://linux.vbird.org)

该教程内容详实全面，是Linux入门的好材料。
这里用到一些Linux下的基本操作，客户端可以选择PUTTY，至少掌握：

> ls
> cd
> tar
> man
> gcc
> make
> vim
> nano

命令不能一一列举。

# Dstar Lite程序使用说明
该程序调用一些GNU库，请在类Unix系统下编译使用。
如果系统没有安装编译工具，则需要先安装 (Ubuntu)：

``` 
$ sudo apt-get install build-essential
```
[原文地址](http://blog.csdn.net/ayawaya/article/details/70155932)

CSDN下载：
[dstar.tar.gz](http://download.csdn.net/detail/ayawaya/9828088)

- - - ------------
下载后解压，进入解压后的目录：
``` 
$ cd dstar
$ ls
```
![这里写图片描述](http://img.blog.csdn.net/20170413133859594?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYXlhd2F5YQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后使用make编译
```
$ make
```
![这里写图片描述](http://img.blog.csdn.net/20170413133941198?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYXlhd2F5YQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

完毕，运行程序:
```
$ ./dstar
```
![仿真结果](http://img.blog.csdn.net/20170413134116089?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYXlhd2F5YQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

--------
**仿真程序操作命令：**

 - **[q/Q] - 退出**
 - **[r/R] - 再次规划路径**
 - **[a/A] - 切换自动规划**
 - **[c/C] - 清屏（重启）**
 - **鼠标左键 - 设置障碍物**
 - **鼠标中间 - 移动目标点**
 - **鼠标右键 - 移动起始点**

![这里写图片描述](http://img.blog.csdn.net/20170413140515101?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYXlhd2F5YQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

- - -
程序提供的Dstar类可以单独调用，使用vim编辑器编写程序：
```
$ vim DstarDraw.cpp
```
输入以下内容：
``` c
#include "Dstar.h"
int main() {
    Dstar *dstar = new Dstar();
    list<state> mypath;
    dstar->init(0,0,10,5);         // set start to (0,0) and goal to (10,5)
    dstar->updateCell(3,4,-1);     // set cell (3,4) to be non traversable
    dstar->updateCell(2,2,42.432); // set set (2,2) to have cost 42.432
    dstar->replan();               // plan a path
    mypath = dstar->getPath();     // retrieve path
    dstar->updateStart(10,2);      // move start to (10,2)
    dstar->replan();               // plan a path
    mypath = dstar->getPath();     // retrieve path
    dstar->updateGoal(0,1);        // move goal to (0,1)
    dstar->replan();               // plan a path
    mypath = dstar->getPath();     // retrieve path
    return 0;
}
```
* * *
该算法还有多种改进分支，在此基础上进一步研究。
https://blog.csdn.net/ayawaya/article/details/70155932

* * *
