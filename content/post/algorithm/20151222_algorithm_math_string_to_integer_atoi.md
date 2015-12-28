+++
title = "Exploring Algorithm: String to Integer (atoi)"
date  = "2015-12-22T00:00:00-00:00"
tags = ["algorithm", "math problem"]
slug= "algorithm_math_string_to_integer_atoi"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/string-to-integer-atoi/).

```markdown
Implement atoi to convert a string to an integer.
```

#### Solution
It's little annoying because this problem intentionally hides a detailed requirement to let us do some try and error. If this is an interview question, such kind of conversation  is really necessary. But in this case, to avoid confusion, I'm writing the requirement below:

```markdown
The function first discards as many whitespace characters as necessary  
until the first non-whitespace character is found.  
Then, starting from this character, takes an optional initial plus or minus sign  
followed by as many numerical digits as possible, and interprets them  
as a numerical value.

The string can contain additional characters after those that form the integral number,   which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number,  
or if no such sequence exists because either str is empty or  
it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.  
If the correct value is out of the range of representable values,  
INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.
```

When we know the detail, this problem can be solved in a similar manner as [Reverse Integer](http://blog.rdtr.net/post/algorithm/algorithm_math_reverse_integer/) problem. We can extract a digit one by one, sum it up, and multiply by a base(= `10`).  

A code in Python:

```python
class Solution(object):
    def myAtoi(self, str):
        str  = str.strip()
        
        negative = 1
        res      = 0
        for i, ch in enumerate(str):
            if i == 0 and ch == '-':
                negative = -1
                continue
            elif i == 0 and ch == '+':
                continue
            
            if ch.isdigit():
                res *= 10
                res += ord(ch) - ord('0')
            else:
                break
        
        if negative == 1 and res > 2147483647:
            return 2147483647
        elif negative == -1 and res > 2147483648:
            return -2147483648
        return negative * res
```

A code in Java:

```java
public class Solution {
    public int myAtoi(String str) {
        int start = 0;
        while (start < str.length() && str.charAt(start) == ' ') ++start;
        if (start == str.length()) return 0;
        
        int f = 1; // = 1 if the result >= 0, otherwise -1
        if (str.charAt(start) == '-') {
            isPositive = -1;
            ++start;
        } else if (str.charAt(start) == '+') ++start;
        
        int res = 0;
        for (int i = start; i < str.length(); i++) {
            char ch = str.charAt(i);
            if (ch >= '0' && ch <= '9') {
                int digit = ch - '0';
                if (f == 1 &&
                    (res > Integer.MAX_VALUE/10 || digit > Integer.MAX_VALUE-res*10)) {
                    return 2147483647;
                } else if (f == -1 &&
                (-1*res < Integer.MIN_VALUE/10 || -1*digit < (Integer.MIN_VALUE+res*10))){
                    return -2147483648;
                }
                res = res * 10 + digit;
            } else break;
        }
        return res * f;
    }
}
```

Time complecity is `O(N)` while `N` is a length of `str`. Also, it consumes `O(1)` space.