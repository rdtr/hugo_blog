+++
title = "Exploring Algorithm: Longest Common Prefix"
date  = "2016-01-06T00:00:00-00:00"
tags  = ["algorithm", "string problem"]
slug  = "algorithm_string_longest_common_prefix"
+++

#### Problem
This problem is quoted from [LeetCode](https://leetcode.com/problems/longest-common-prefix/).

```markdown
Write a function to find the longest common prefix string amongst an array of strings.
```

#### Solution

```markdown
check a character at [i] of all strings
              |
              v
        +---+---+---+---+---+--+
strs[0] | A | B |   |   |   |
        +---+---+---+---+---+--+

        +---+---+---+---+-+
strs[1] | A | B |   |   |
        +---+---+---+---+-+

        +---+---+-+
strs[2] | A | B |
        +---+---+-+
```

We have to check each index of all strings in a given array to determine whether a common prefix ends at that index or not.

When we should stop this iteration? Because there can be no common prefix which is longer than the shortest string, we can find it and its length in advance and just continue until the index we're checking reaches the last index of the shortest string.  
If not all characters of the same index aren't equal during the iteration, that means the longest common prefix ends at that point.

A code in Python:

```python
class Solution(object):
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """

        strsNum = len(strs)
        if   strsNum == 0: return ''
        elif strsNum == 1: return strs[0]

        # check the minimum length among all strings
        minLen, minStr = len(strs[0]), strs[0]
        for st in strs:
            if len(st) < minLen:
                minLen, minStr = len(st), st


        # check all string's character at the same index
        for i in range(minLen):
            c = strs[0][i]
            for j in range(strsNum):
                _c = strs[j][i]
                if c != _c:
                    return minStr[0:i]
        return minStr # the shortest string itself is the longest common prefix
```

Let's say the length of a given array with strings = `N`, and the length of the shortest string among them is `M`.  
Checking the string which has minimum length takes `O(N)` time complexity and the second iteration checking each character of each index until the end of the shortest string takes `O(MN)`.

Therefore, this solution takes `O(MN)` time complexity as a whole. Space complexity is constant, which is `O(1)` except for the array given.


A code in Java:

```java
public class Solution {
    public String longestCommonPrefix(String[] strs) {
        int leng = strs.length;
        if (leng == 0) return "";
        else if (leng == 1) return strs[0];

        // check the minimum length among all strings
        int minLen    = strs[0].length();
        String minStr = strs[0];
        for (int i = 1; i < leng; i++) {
            if (strs[i].length() < minLen) {
                minLen = strs[i].length();
                minStr = strs[i];
            }
        }

        for (int j = 0; j < minLen; j++) {
            char ch = strs[0].charAt(j);
            for (int k = 1; k < leng; k++) {
                char _ch = strs[k].charAt(j);
                if (ch != _ch) {
                    return minStr.substring(0, j);
                }
            }
        }
        return minStr;
    }
}
```
