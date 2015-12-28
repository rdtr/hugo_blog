+++
title = "Exploring Algorithm: 2 Sum"
date  = "2015-11-08T00:00:00-00:00"
tags = ["algorithm", "search problem"]
slug= "algorithm_searching_two_sum"
+++

#### Problem
This problem is quoted from [LeetCode](https://leetcode.com/problems/two-sum/).

```markdown
Given an array of integers, find 2 numbers such that
they add up to a specific target number.
You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: [1, 2]
```

#### Solution
We can solve this problem by using brote-force method, just check all combination of two elements in a given array, then check their sum. But this takes `O(N^2)` in terms of time complexity.

Instead, while iterating through the array `A`, we can store each element in a hash map as a key. Because finally we have to output an index of each integer, it's natural to store an index as value to the map.  
We can also check if a value such that `value = target - A[i]` exists in the precious indexes by using the map. Then we can iterate through the array only once and can stop searching as soon as we find the solution.

A code in Python:

```python
class Solution(object):
    def twoSum(self, nums, target):
        m = {} # store existence of each character
        for i, num in enumerate(nums):
            v = target - num
            if v not in m:
                m[num] = i + 1
            else:
                return [m[v], i+1]
```

Time complexicity: `O(N)`, space complexicity: `O(N)` while length of the given array = `N`.

A code in Java:

```java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> m = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++) {
            int num = nums[i];
            int diff = target - num;
            if (m.containsKey(diff)) {
                return new int[] {m.get(diff), i+1};
            }
            m.put(num, i+1);
        }
        return new int[2];
    }
}
```