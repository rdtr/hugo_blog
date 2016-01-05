+++
title = "Exploring Algorithm: Roman to Integer"
date  = "2016-01-05T00:00:00-00:00"
tags  = ["algorithm", "math problem"]
slug  = "algorithm_math_romanr_to_integer"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/roman-to-integer/).

```markdown
Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.
```

#### Solution
For me, an engineer from Japan, this problem is difficult rather because I am not so familiar with Roman Numeral than because it's really hard. The rule how we can convert an integer given to roman numeral is written in [Wikipedia](https://en.wikipedia.org/wiki/Roman_numerals).

Actually, we have a really simple rule as follows:

|Value|Symbol|
|:----|:----|
| 1   |I    |
| 4   |IV   |
| 5   |V    |
| 9   |IX   |
| 10  |X    |
| 50  |L    |
| 90  |XC   |
| 100 |C    |
| 400 |CD   |
| 500 |D    |
| 900 |CM   |
|1000 |M    |

Basically we can just check a character of a given string, and accumulate a corresponding value from a symbol character. For example `III` is `1 + 1 + 1 = 3` and `XVII` is `10 + 5 + 1 + 1 = 17`. Only thing we have to be careful is that there are some exceptions about `4` and `9` related symbols.

A code in Python:

```python
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        leng = len(s)

        s   = s + ' '
        i   = 0
        res = 0
        while i < leng:
            ch, nxCh = s[i], s[i+1]
            inc = 1
            if ch == 'M':
                res += 1000
            elif ch == 'C':
                if nxCh == 'M':
                    res += 900
                    inc = 2
                elif nxCh == 'D':
                    res += 400
                    inc = 2
                else:
                    res += 100
            elif ch == 'D':
                res += 500
            elif ch == 'X':
                if nxCh == 'C':
                    res += 90
                    inc = 2
                elif nxCh == 'L':
                    res += 40
                    inc = 2
                else:
                    res += 10
            elif ch == 'L':
                res += 50
            elif ch == 'I':
                if nxCh == 'X':
                    res += 9
                    inc = 2
                elif nxCh == 'V':
                    res += 4
                    inc = 2
                else:
                    res += 1
            else: # if ch == 'V'
                res += 5;
            i += inc
        return res
```

The code looks complicated but it's just checking each symbol of a string given, while handling `4` and `9` related ones. To avoid being annoyed by an "out of index" error when grabbing a character at a next index, I just append one extra character to the string given before begin to iterate it.

A code in Java:

```java
public class Solution {
    public int romanToInt(String s) {
        int len = s.length();

        s = s + " ";
        int res = 0;
        for (int i = 0; i < len; i++) {
            char ch   = s.charAt(i);
            char nxCh = s.charAt(i+1);

            if (ch == 'M') {
                res += 1000;
            } else if (ch == 'C') {
                if (nxCh == 'M') {
                    res += 900;
                    i++;
                } else if (nxCh == 'D') {
                    res += 400;
                    i++;
                } else {
                    res += 100;
                }
            } else if (ch == 'D') {
                res += 500;
            } else if (ch == 'X') {
                if (nxCh == 'C') {
                    res += 90;
                    i++;
                } else if (nxCh == 'L') {
                    res += 40;
                    i++;
                } else {
                    res += 10;
                }
            } else if (ch == 'L') {
                res += 50;
            } else if (ch == 'I') {
                if (nxCh == 'X') {
                    res += 9;
                    i++;
                } else if (nxCh == 'V') {
                    res += 4;
                    i++;
                } else {
                    res++;
                }
            } else { // if (ch == 'V')
                res += 5;
            }
        }
        return res;
    }
}
```

Time complexity is `O(N)` while `N` is a length of the string given. This solution also takes `O(1)` space.
