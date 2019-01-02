---
layout: post

title:  "[LeetCode] Find Eventual Safe States 找到最终的安全状态"

date:   2018-09-30 19:40:10 +0800

categories: c++

tags: algorithm

description: Find Eventual Safe States
---

# Find Eventual Safe States 找到最终的安全状态

## Problem Description

In a directed graph, we start at some node and every turn, walk along a directed edge of the graph.  If we reach a node that is terminal (that is, it has no outgoing directed edges), we stop.

Now, say our starting node is *eventually safe *if and only if we must eventually walk to a terminal node.  More specifically, there exists a natural number `K` so that for any choice of where to walk, we must have stopped at a terminal node in less than `K` steps.

Which nodes are eventually safe?  Return them as an array in sorted order.

The directed graph has `N` nodes with labels `0, 1, ..., N-1`, where `N` is the length of `graph`.  The graph is given in the following form: `graph[i]` is a list of labels `j` such that `(i, j)` is a directed edge of the graph.

```
Example:
Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]
Output: [2,4,5,6]
Here is a diagram of the above graph.


```

![Illustration of graph](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

Note:

- `graph` will have length at most `10000`.
- The number of edges in the graph will not exceed `32000`.
- Each `graph[i]` will be a sorted list of different integers, chosen within the range `[0, graph.length - 1]`.



---



## Algorithom

  * 解题思路:

    这道题给了我们一个有向图，然后定义了一种最终安全状态的结点，就是说该结点要在自然数K步内停止，所谓停止的意思，就是再没有向外的边，即没有出度，像上面例子中的结点5和6就是出度为0，因为graph[5]和graph[6]均为空。

    那么我们分析题目中的例子，除了没有出度的结点5和6之外，结点2和4也是安全状态结点，为啥呢，我们发现结点2和4都只能到达结点5，而结点5本身就是安全状态点，所以2和4也就是安全状态点了，所以我们可以得出的结论是，若某结点唯一能到达的结点是安全状态结点的话，那么该结点也同样是安全状态结点。那么我们就可以从没有出度的安全状态往回推，比如结点5，往回推可以到达结点4和2，先看结点4，此时我们先回推到结点4，然后将这条边断开，那么此时结点4出度为0，则标记结点4也为安全状态结点，同理，回推到结点2，断开边，此时结点2虽然入度仍为2，但是出度为0了，标记结点2也为安全状态结点。

    分析到这里，思路应该比较明朗了，由于我们需要回推边，所以需要建立逆向边，用一个集合数组来存，由于题目要求返回的结点有序，我们可以利用集合TreeSet的自动排序的特性，由于需要断开边，为了不修改输入数据，所以我们干脆再建一个顺向边得了，即跟输入数据相同。还需要一个safe数组，布尔型的，来标记哪些结点是安全状态结点。在遍历结点的时候，直接先将出度为0的安全状态结点找出来，排入一个队列queue中，方便后续的处理。后续的处理就有些类似BFS的操作了，我们循环非空queue，取出队首元素，标记safe中该结点为安全状态结点，然后遍历其逆向边的结点，即可以到达当前队首结点的所有结点，我们在正向边集合中删除对应的边，如果此时结点出度为0了，将其加入队列queue中等待下一步处理，这样while循环退出后，所有的安全状态结点都已经标记好了，我们直接遍历safe数组，将其存入结果res中即可，参见代码如下：

    ​

* Code Implement:

  ```c++
  class Solution {
  public:
      vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
          vector<int> res;
          int n = graph.size();
          vector<bool> safe(n, false);
          vector<set<int>> g(n, set<int>()), revg = g;
          queue<int> q;
          for (int i = 0; i < n; ++i) {
              if (graph[i].empty()) q.push(i);
              for (int j : graph[i]) {
                  g[i].insert(j);
                  revg[j].insert(i);
              }
          }
          while (!q.empty()) {
              auto t = q.front(); q.pop();
              safe[t] = true;
              for (int i : revg[t]) {
                  g[i].erase(t);
                  if (g[i].empty()) q.push(i);
              }
          }
          for (int i = 0; i < n; ++i) {
              if (safe[i]) res.push_back(i);
          }
          return res;
      }
  };
  ```


​      