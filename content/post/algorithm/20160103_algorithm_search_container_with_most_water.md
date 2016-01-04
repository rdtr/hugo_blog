+++
title = "Exploring Algorithm: Container With Most Water"
date  = "2016-01-03T00:00:00-00:00"
tags  = ["algorithm", "search problem"]
slug  = "algorithm_search_container_with_most_water"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/container-with-most-water/).

```markdown
Given n non-negative integers a1, a2, ..., an, where each represents
a point at coordinate (i, ai). n vertical lines are drawn such that
the two endpoints of line i is at (i, ai) and (i, 0).

Find two lines, which together with x-axis forms a container,
such that the container contains the most water.
```

#### Solution
We can solve this by brute-force search, pick all combinations of `i` and `j` while `i ≠ j and 1 <= i, j <= n`, check capacity of water for two lines of `ai` and `aj`, and return the maximum one.  

The brute-force search, however, takes `O(N^2)` time complexity. Is there any way to shorten time to take? Let's investigate the condition deeper.

![example1](http://f.st-hatena.com/images/fotolife/r/rdtr/20160103/20160103182657.png?1451874433)

See an example above. We first start from picking the most left side `A1` and the most right side `An`. The capacity of a container with these two lines are `(n-1)*an`, because `a1 > an`.

![example2](http://f.st-hatena.com/images/fotolife/r/rdtr/20160103/20160103183608.png?1451874978)

This container has the greatest width `n-1` so if we want to have a bigger container, we have to find  a higher one, obviously. At this time, it's important to notice that as long as you pick `An`, you can never find such containers. Because whatever another point you pick from `A2` to `An-1`, a height of a container is capped to `an`. So we can save time by not checking a container through `(A2, An)` to `(An-1, An)` and instead checking `(A1, An-1)` which could be a higher container than `(A1, An)`.

Summarizing the above, we can start from `A1` and `An` and check a container's capacity, then if `a1 > an` then pick `an-1` instead of `an` and check a new container's capacity, otherwise pick `a2` instead of `a1` as a new one. Then we would continue this process until two points will indicate the same one.

A code in Python:

```python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        leng = len(height)
        if leng <= 1: 0

        res = 0
        left, right = 0, leng - 1
        while left != right:
            hl = height[left]
            hr = height[right]

            if hl < hr: h = hl
            else: h = hr

            v  = h * (right - left)
            if v > res: res = v

            if hl < hr:
                left += 1
            else:
                right -= 1
        return res
```

A code in Java:

```java
public class Solution {
    public int maxArea(int[] height) {
        int len = height.length;

        if (len <= 1) return 0;
        int left   = 0;
        int right  = len - 1;

        int res = 0;
        while (left != right) {
            int hl = height[left];
            int hr = height[right];

            int h = (hl < hr)? hl : hr;
            int v = h * (right - left);

            res = (v > res)? v : res;

            if (hl < hr) left++;
            else right--;
        }
        return res;
    }
}
```

Note: It's little unclear but this problem on LeetCode asks us return a maximum capacity of possible containers.  
Time complexity is `O(N)` and it doesn't cost any extra time space other than the array passed as an argument.
