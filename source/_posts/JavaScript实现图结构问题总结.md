---
title: JavaScript实现图结构问题总结
date: 2021-07-12 21:50:55
categories: 
- 数据结构与算法JavaScript描述中部分问题
tags:
- hexo
- Gitalk
---
# 一  邻接表问题

书中的邻接表是这样定义的，在node v14中实现后发现有问题，下面是书中实现邻接表的代码。
   
    for (let i = 0; i < this.vertices; i++) {
        this.adj[i] = [];
        this.adj[i].push(''); //去掉这行可以正常运行
        }

虽然说直接去掉push加默认空字符串的代码后就可以正常运行，但是还要知其所以然，让我们一起来看看为什么加上这一行就会导致错误呢。报错信息是TypeError: this.adj[v] is not iterable，下面是完整的graph类代码
//图类的构建(邻接表法表示图的边（图的顶点列表构成数组，对应数组存着相连的顶点），属性，方法，深度优先，广度优先
    function Graph(v) {
    this.vertices = v; //图的顶点个数
    this.edges = 0; //图的边
    ////构建邻接表
    this.adj = [];
    for (let i = 0; i < this.vertices; i++) {
        this.adj[i] = [];
        this.adj[i].push(''); //这个默认值影响到dfs的递归了，查下怎么回事,原书问题还是有些的，
        // 解释：加入字符的数据typeof g.adj[0] Object，对象没有iterator接口，所以for of用不了，解决办法有一些
        //可以自己在Symbol.iterator上部署，或者使用别的数据结构set，或者 Object.keys迭代属性，
        // 但是本文加入是number类型就能成功遍历，上面是思路，稍后个人博客会实现验证下
        // this.adj[i] = []; //邻接表初始化
    }
    this.dfs = dfs;
    this.bfs = bfs;
    //深度优先搜索的标记访问与否的数组
    this.marked = [];
    for (let i = 0; i < this.vertices; i++) {
        this.marked[i] = false;
    }
    this.addEdge = addEdge; //添加边
    this.showGraph = showGraph; //展示图
    }

    //添加边，参数是两个顶点，传入会互相添加到对应的邻接表中，然后边到数量加一
    function addEdge(v, w) {
    this.adj[v].push(w);
    this.adj[w].push(v);
    this.edges++;
    }

    //展示图打印所有顶点和相邻顶点列表
    function showGraph() {
    for (let i = 0; i < this.vertices; i++) {
        console.log(i + '=>');
        for (let j = 0; j < this.vertices; j++) {
        if (this.adj[i][j] !== undefined) {
            console.log(this.adj[i][j] + ' ');
        }
        }
    }
    }

    //深度优先搜索，深搜先从一条起始顶点开始，到达最后一个顶点，然后回溯，继续下一个路径；直到最后，没有路径位置。
    // 实现的时候访问当前没有访问过的顶点，然后标记为已访问，再递归的访问初始顶点邻接表中其它没有访问的
    function dfs(v) {
    this.marked[v] = true; //访问过的节点标记true
    if (this.adj[v]) {
        console.log(this.adj[v], 'visit vertex:', v);
    }
    // this.adj[v].forEach(w=>{
    //   if (!this.marked[w]) { //没有访问过的话，就递归访问
    //     this.dfs(w);
    //   }
    //   })
    for (let w of this.adj[v]) { //遍历邻接表中的图节点
        if (!this.marked[w]) { //没有访问过的话，就递归访问
        this.dfs(w);
        }
    }
    }
一开始以为是因为this.adj[v]是Object，对象没有iterator接口，所以for of用不了，但是，我又发现去掉后的也是object，但是邻接表中没有第一个的空字符串后，都是number就可以遍历，