---
layout: post

title:  "[LeetCode] All Paths From Source to Target 从起点到目标点到所有路径"

date:   2018-10-14 22:30:10 +0800

categories: c++

tags: algorithm

description: All Paths From Source to Target
---

# All Paths From Source to Target 

## Problem Description

- Given a directed, acyclic graph of `N` nodes.  Find all possible paths from node `0` to node `N-1`, and return them in any order.

  The graph is given as follows:  the nodes are 0, 1, ..., graph.length - 1.  graph[i] is a list of all nodes j for which the edge (i, j) exists.

  ```
  Example:
  Input: [[1,2], [3], [3], []] 
  Output: [[0,1,3],[0,2,3]] 
  Explanation: The graph looks like this:
  0--->1
  |    |
  v    v
  2--->3
  There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.

  ```

  Note:

  - The number of nodes in the graph will be in the range `[2, 15]`.
  - You can print different paths in any order, but you should keep the order of nodes inside one path.

---



## Algorithom

  * 解题思路:

    这道题给了我们一个无回路有向图，包含N个结点，然后让我们找出所有可能的从结点0到结点N-1的路径。

    这个图的数据是通过一个类似邻接链表的二维数组给的，其实很简单，我们来看例子中的input，[[1,2], [3], [3], []]，这是一个二维数组，最外层的数组里面有四个小数组，每个小数组其实就是和当前结点相通的邻结点，由于是有向图，所以只能是当前结点到邻结点，反过来不一定行。那么结点0的邻结点就是结点1和2，结点1的邻结点就是结点3，结点2的邻结点也是3，结点3没有邻结点。

    那么其实这道题的本质就是遍历邻接链表，由于要列出所有路径情况，那么递归就是不二之选了。我们用cur来表示当前遍历到的结点，初始化为0，然后在递归函数中，先将其加入路径path，如果cur等于N-1了，那么说明到达结点N-1了，将path加入结果res。否则我们再遍历cur的邻接结点，调用递归函数即可，参见代码如下：

    ​

* Code Implement:

  ```c++
  class Solution {
  public:
      vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
          vector<vector<int>> res;
          helper(graph, 0, {}, res);
          return res;
      }
      void helper(vector<vector<int>>& graph, int cur, vector<int> path, vector<vector<int>>& res) {
          path.push_back(cur);
          if (cur == graph.size() - 1) res.push_back(path);
          else for (int neigh : graph[cur]) helper(graph, neigh, path, res);
      }
  };
  ```


​      