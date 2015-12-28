+++
title = "Exploring Algorithm: Palindrome Number"
date  = "2015-12-23T00:00:00-00:00"
tags  = ["algorithm", "math problem"]
slug  = "algorithm_math_palindrome_number"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/palindrome-number/).

```markdown
Determine whether an integer is a palindrome. Do this without extra space.
```

#### Solution
We have another problem to make an integer reversed in [here](http://blog.rdtr.net/post/algorithm/algorithm_math_reverse_integer/). We can utilize this solution directly in this problem. That is, we can get a reversed integer of a given one and if it's a palindrome, the reversed one should equal to the original one.

A code in Python:

```python
class Solution(object):
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        if x < 0: return False
        elif x >= 0 and x < 10: return True
        
        originalX = x
        reversedX = 0
        while x > 0:
            reversedX = reversedX * 10 + x % 10
            x /= 10
        return originalX == reversedX
```

A code in Java:

```java
public class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0) return false;
        else if (x >= 0 && x < 10) return true;
        
        int originalX = x;
        int reversedX = 0;
        while (x > 0) {
            reversedX = reversedX * 10 + x % 10;
            x /= 10;
        }
        return originalX == reversedX;
    }
}
```

Time complecity is `O(N)` while `N` is a length of digits of `x`. Also, it consumes `O(1)`.