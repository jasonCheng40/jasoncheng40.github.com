---
layout: post

title:  "[LeetCode] Number of Subarrays with Bounded Maximum 有界限最大值的子数组数量"

date:   2018-10-21 22:40:10 +0800

categories: c++

tags: algorithm

description: Number of Subarrays with Bounded Maximum
---

# Number of Subarrays with Bounded Maximum

## Problem Description

We are given an array `A` of positive integers, and two positive integers `L` and `R` (`L <= R`).

Return the number of (contiguous, non-empty) subarrays such that the value of the maximum array element in that subarray is at least `L` and at most `R`.

```
Example :
Input: 
A = [2, 1, 4, 3]
L = 2
R = 3
Output: 3
Explanation: There are three subarrays that meet the requirements: [2], [2, 1], [3].

```

Note:

- L, R  and `A[i]` will be an integer in the range `[0, 10^9]`.
- The length of `A` will be in the range of `[1, 50000]`.

Given a list of query words, return the number of words that are stretchy. 

```
Example:
Input: 
S = "heeellooo"
words = ["hello", "hi", "helo"]
Output: 1
Explanation: 
We can extend "e" and "o" in the word "hello" to get "heeellooo".
We can't extend "helo" to get "heeellooo" because the group "ll" is not extended.

```

Notes:

- `0 <= len(S) <= 100`.
- `0 <= len(words) <= 100`.
- `0 <= len(words[i]) <= 100`.
- `S` and all words in `words` consist only of lowercase letters

---



## Algorithom

  * 解题思路:

    这道题给了我们一个数组，又给了我们两个数字L和R，表示一个区间范围，让我们求有多少个这样的子数组，使得其最大值在[L, R]区间的范围内。

    既然是求子数组的问题，那么最直接，最暴力的方法就是遍历所有的子数组，然后维护一个当前的最大值，只要这个最大值在[L, R]区间的范围内，结果res自增1即可。但是这种最原始，最粗犷的暴力搜索法，OJ不答应。但是其实我们略作优化，就可以通过了。

    优化的方法是，首先，如果当前数字大于R了，那么其实后面就不用再遍历了，不管当前这个数字是不是最大值，它都已经大于R了，那么最大值可能会更大，所以没有必要再继续遍历下去了。同样的剪枝也要加在内层循环中加，当curMax大于R时，直接break掉内层循环即可，参见代码如下：

    ​

* Code Implement:

  ```c++
  class Solution {
  public:
      int numSubarrayBoundedMax(vector<int>& A, int L, int R) {
          int res = 0, n = A.size();
          for (int i = 0; i < n; ++i) {
              if (A[i] > R) continue;
              int curMax = INT_MIN;
              for (int j = i; j < n; ++j) {
                  curMax = max(curMax, A[j]);
                  if (curMax > R) break;
                  if (curMax >= L) ++res;
              }
          }
          return res;
      }
  };
  ```


​      

* Optimization:

  虽然上面的方法做了剪枝后能通过OJ，但是我们能不能在线性的时间复杂度内完成呢。答案是肯定的，优化方法是用一个子函数来算出最大值在[-∞, x]范围内的子数组的个数，而这种区间只需要一个循环就够了，为啥呢？

  我们来看数组[2, 1, 4, 3]的最大值在[-∞, 4]范围内的子数组的个数。当遍历到2时，只有一个子数组[2]，遍历到1时，有三个子数组，[2], [1], [2,1]。当遍历到4时，有六个子数组，[2], [1], [4], [2,1], [1,4], [2,1,4]。当遍历到3时，有十个子数组。其实如果长度为n的数组的最大值在范围[-∞, x]内的话，其所有子数组都是符合题意的，而长度为n的数组共有n(n+1)/2个子数组，刚好是等差数列的求和公式。

  所以我们在遍历数组的时候，如果当前数组小于等于x，则cur自增1，然后将cur加到结果res中；如果大于x，则cur重置为0。这样我们可以正确求出最大值在[-∞, x]范围内的子数组的个数。而要求最大值在[L, R]范围内的子数组的个数，只需要用最大值在[-∞, R]范围内的子数组的个数，减去最大值在[-∞, L-1]范围内的子数组的个数即可，参见代码如下：

  ```c++
  class Solution {
  public:
      int numSubarrayBoundedMax(vector<int>& A, int L, int R) {
          return count(A, R) - count(A, L - 1);
      }
      int count(vector<int>& A, int bound) {
          int res = 0, cur = 0;
          for (int x : A) {
              cur = (x <= bound) ? cur + 1 : 0;
              res += cur;
          }
          return res;
      }
  };
  ```

  ​