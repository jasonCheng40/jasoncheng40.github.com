---
layout: post

title:  "[LeetCode] Custom Sort String 自定义排序的字符串"

date:   2018-10-28 19:35:09 +0800

categories: c++

tags: algorithm

description: Custom Sort String
---

# Custom Sort String 自定义排序的字符串

## Problem Description

`S` and `T` are strings composed of lowercase letters. In `S`, no letter occurs more than once.

`S` was sorted in some custom order previously. We want to permute the characters of `T` so that they match the order that `S` was sorted. More specifically, if `x` occurs before `y` in `S`, then `x` should occur before `y` in the returned string.

Return any permutation of `T` (as a string) that satisfies this property.

```
Example :
Input: 
S = "cba"
T = "abcd"
Output: "cbad"
Explanation: 
"a", "b", "c" appear in S, so the order of "a", "b", "c" should be "c", "b", and "a". 
Since "d" does not appear in S, it can be at any position in T. "dcba", "cdba", "cbda" are also valid outputs.

```

 

Note:

- `S` has length at most `26`, and no character is repeated in `S`.
- `T` has length at most `200`.
- `S` and `T` consist of lowercase letters only.

---



## Algorithom

  * 解题思路:

    这道题给了我们两个字符串S和T，让我们将T按照S的顺序进行排序，就是说在S中如果字母x在字母y之前，那么排序后的T中字母x也要在y之前，其他S中未出现的字母位置无所谓。

    那么我们其实关心的是S中的字母，只要按S中的字母顺序遍历，对于遍历到的每个字母，如果T中有的话，就先排出来，这样等S中的字母遍历完了，再将T中剩下的字母加到后面即可。所以我们先用HashMap统计T中每个字母出现的次数，然后遍历S中的字母，只要T中有，就将该字母重复其出现次数个，加入结果res中，然后将该字母出现次数重置为0。之后再遍历一遍HashMap，将T中其他字母加入结果res中即可，参见代码如下：

    ​

* Code Implement:

  ```c++
  class Solution {
  public:
      string customSortString(string S, string T) {
          string res = "";
          unordered_map<char, int> m;
          for (char c : T) ++m[c];
          for (char c : S) {
              res += string(m[c], c);
              m[c] = 0;
          }
          for (auto a : m) {
              res += string(a.second, a.first);
          }
          return res;
      }
  };
  ```


​      