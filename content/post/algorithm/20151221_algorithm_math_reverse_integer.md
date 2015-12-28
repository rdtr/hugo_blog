+++
title = "Exploring Algorithm: Reverse Integer"
date  = "2015-12-21T00:00:00-00:00"
tags = ["algorithm", "math problem"]
slug= "algorithm_math_reverse_integer"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/reverse-integer/).

```markdown
Reverse digits of an integer.

Example1: x = 123, return 321
Example2: x = -123, return -321
```

#### Solution
I believe we're expected to solve this problem without using extra space that takes `O(N)` space. Regarding an example of `123`, how we can extract the last digit number `3`? This is fairly easy, we can just calculate

```markdown
123 % 10 = 3
```

Then by dividing the number by `10` and calculate a remainder again, we can get the next digit number `2`. We need to continue this process until the number becomes zero.

```markdown
123 / 10 = 12
12 % 10 = 2 // 2nd digit

12 / 10 = 1
1 % 10 = 1 // 1st digit

1 / 10 = 0 // finish
```

Now we can extract each digit one by one like `3`, `2`, `1`. Then what we have to do is just like below:

```markdown
result = 3 // last digit
// result = 3

result = result * 10 + 2 // multiply by 10 and add 2nd digit
// result = 32

result = result * 10 + 1 // multiply by 10 and add 1st digit
// result = 321
```

That is to say, we iterate through all digits and add each digit and multiply * 10, repeatedly. Also, we have to consider a case the number is negative.  
Note: It seems LeetCode asks us to return `0` if `x >= 2^31` or `x < -(2^31)`.

A code in Python:

```python
class Solution(object):
    INTEGER_MAX = 2**31
    
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        isPositive = 1
        if x < 0:
            isPositive = -1
            x = -x
        
        sum = 0
        while x > 0:
            r = x % 10
            
            maxDiff = self.INTEGER_MAX - sum * 10
            if sum > self.INTEGER_MAX / 10 or r > maxDiff:
                return 0
            
            sum = sum * 10 + r
            x = x / 10
        return sum * isPositive
```

A code in Java:

```java
public class Solution {
    public int reverse(int x) {
        int isPositive = 1;
        if (x < 0) {
            isPositive = -1;
            x = -1 * x;
        }
        
        int sum  = 0;
        while (x > 0) {
            int r = x % 10;
            
            int maxDiff = Integer.MAX_VALUE - sum * 10;
            if (sum > Integer.MAX_VALUE / 10 || r > maxDiff) return 0;
            
            sum = sum * 10 + r;
            x /= 10;
        }
        return isPositive * sum;
    }
}
```

This takes `O(1)` space and time complexity is `O(N)` while `N` is a number of digits of a given integer.
