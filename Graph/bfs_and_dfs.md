# 深度优先搜索(dfs)和广度优先搜(bfs)索理论基础

## 区别

深度优先搜索(Depth First Search)：一直往下搜索，指导一个尽头

广度优先搜索(Broad First Search)：优先遍历离自己最近的节点

## DFS

两个关键点:

1. 搜索方向，认准一个方向搜索，直到 **碰壁/达到目标** 后我们就更换方向

2. 换方向的本质就是**撤销原路径**，改为节点链接的下一个路径，本质是一个**回溯**的过程

### 代码框架

因为dfs的过程中需要用到**回溯**，因此使用**递归**来实现实际上是最方便的。

```cpp
void dfs(/* parameter */) {
  if (/* terminate condition */) {
    /* store result */

    return;
  }

  for (/* selection: other nodes connected to this node */) {
    /* process node */;

    dfs(/* graph */, /* selected node */ ); // recursion

    /* backtrace, recall processed result */
  }
}
```

### 深度优先三部曲

1. 确认递归函数，参数

`void dfs(/* parameter */)`

一般情况下，dfs需要二维数组表示**所有路径**，以及一维数组表示**单一路径**。\
通常情况下，我们使用全局变量来表示他们。

```cpp
vector<vector<int>> result; // 保存符合条件的所有路径
vector<int> path;           // 起点到终点的路径
void dfs(/* graph */, /* current searching node */)
```

上述代码不强求
2. 确认终止条件

```cpp
if (/* terminate condition */) {
  /* store result */

  return;
}
```

终止条件特别重要。

终止条件不仅是吧**结束本层递归**，同时也是得到**结果**的时候
3. 处理目前搜索节点的出发路径
一般来说，这里就是一个`for`循环解决，遍历目前**搜索节点**能够到的所有节点

```cpp
  for (/* selection: other nodes connected to this node */) {
    /* process node */;

    dfs(/* graph */, /* selected node */ ); // recursion

    /* backtrace, recall processed result */
  }
```

## BFS

### 广搜的使用场景

1. 两个点最短距离
2. 岛屿问题(bfs,dfs)

### bfs过程

[bfs](https://code-thinking-1253855093.file.myqcloud.com/pics/20220825103900.png)
只要是bfs，就一定是最短距离

### 代码模版(队列)

```cpp
int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 表示四个方向
// grid 是地图，也就是一个二维数组
// visited标记访问过的节点，不要重复访问
// x,y 表示开始搜索节点的下标
void bfs(vector<vector<char>>& grid, \
         vector<vector<bool>>& visited, \
         int x, int y) {
    queue<pair<int, int>> que; // 定义队列
    que.push({x, y}); // 起始节点加入队列
    visited[x][y] = true; // 只要加入队列，立刻标记为访问过的节点
    while(!que.empty()) { // 开始遍历队列里的元素
        pair<int ,int> cur = que.front(); que.pop(); // 从队列取元素
        int curx = cur.first;
        int cury = cur.second; // 当前节点坐标
        for (int i = 0; i < 4; i++) { // 开始想当前节点的四个方向左右上下去遍历
            int nextx = curx + dir[i][0];
            int nexty = cury + dir[i][1]; // 获取周边四个方向的坐标
            if (nextx < 0 || nextx >= grid.size() \
        || nexty < 0 || nexty >= grid[0].size()) continue;  // 坐标越界了，直接跳过
            if (!visited[nextx][nexty]) { // 如果节点没被访问过
                que.push({nextx, nexty});  // 队列添加该节点为下一轮要遍历的节点
                visited[nextx][nexty] = true; // 只要加入队列立刻标记，避免重复访问
            }
        }
    }

}
```
