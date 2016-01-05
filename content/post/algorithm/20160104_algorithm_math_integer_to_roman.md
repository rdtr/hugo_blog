+++
title = "Exploring Algorithm: Integer to Roman"
date  = "2016-01-04T00:00:00-00:00"
tags  = ["algorithm", "math problem"]
slug  = "algorithm_math_integer_to_roman"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/integer-to-roman/).

```markdown
Given an integer, convert it to a roman numeral.
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

So what we need to do is just check each digit and convert an integer value to a corresponding roman numeric  symbol. I store all possible values for each base `1`, `10`, `100`, `1000`, but there can be multiple ways to represent a mapping from values to symbols.

A code in Python:

```python
class Solution(object):
    def intToRoman(self, num):
        """
        :type num: int
        :rtype: str
        """

        # mp = {scale: {number : roman}}
        mp = {1:    ['', 'I', 'II', 'III', 'IV', 'V', 'VI', 'VII', 'VIII', 'IX'],
              10:   ['', 'X', 'XX', 'XXX', 'XL', 'L', 'LX', 'LXX', 'LXXX', 'XC'],
              100:  ['', 'C', 'CC', 'CCC', 'CD', 'D', 'DC', 'DCC', 'DCCC', 'CM'],
              1000: ['', 'M', 'MM', 'MMM']}

        res = ''
        base = 1000
        while num > 0:
            digit = num / base
            res += mp[base][digit]

            num -= digit * base
            base /= 10
        return res
```

A code in Java:

```java
public class Solution {
    public String intToRoman(int num) {
        // store a mapping table as a 2-dementional array
        String mapping[][] = {
            {"", "M", "MM", "MMM"},
            {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"},
            {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"},
            {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"},
        };

        StringBuilder res = new StringBuilder();
        int base   = 1000;
        int mapIdx = 0;
        while (num > 0) {
            int d = num / base;
            res.append(mapping[mapIdx][d]);

            mapIdx += 1;
            num    %= base;
            base   /= 10;
        }
        return res.toString();
    }
}
```

 This problem limits an argument up to `3999`, so I'd say time complexity is constant = `O(1)`. But more precisely, a number of iterations is proportional to a number of digits of an integer given, that is `O(N)`. Space complexity is also constant.
