+++
title = "Exploring Algorithm: ZigZag Conversion"
date  = "2015-11-07T00:00:00-00:00"
tags = ["algorithm", "string problem"]
slug= "algorithm_string_zigzag_conversion"
+++

#### Problem
This problem is quoated from [LeetCode](https://leetcode.com/problems/zigzag-conversion/).

```markdown
The string "PAYPALISHIRING" is written in a zigzag pattern
on a given number of rows like this:

P   A   H   N
A P L S I I G
Y   I   R

And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this
conversion given a number of rows:

string convert(string text, int nRows);

convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".
```

#### Solution
![example1](http://f.st-hatena.com/images/fotolife/r/rdtr/20151108/20151108232110.png?1447053680)

Let's assign a variable name `t0`, `t1`, `t2` to each string like above, then we immediately see that we need to store each character in a given string in zigzag order (`t0, t1, t2, t1, t0, t1, ...)`.

Python code:

```python
class Solution(object):
    def convert(self, s, numRows):
        leng = len(s)
        if leng == 0:      return ''
        if numRows <= 0:   return ''
        elif numRows == 1: return s
        
        resRows = [''] * numRows
        
        i = 0
        resRowNum = 0
        step = 1
        while i < leng:
            resRows[resRowNum] += s[i]
            
            if (step == 1 and resRowNum == numRows - 1) or (step == -1 and resRowNum == 0):
                step = -1 * step
            
            resRowNum += step
            i += 1
        
        return ''.join(resRows)
```

Time complexicity: `O(M)`, space complexicity: `O(M)` while length of the given string = M.

Java code:

```java
public class Solution {
    public String convert(String s, int numRows) {
        int len = s.length();
        if (len == 0 || numRows == 0) return "";
        if (numRows == 1) return s;
        
        StringBuilder[] resRows = new StringBuilder[numRows];
        for (int i = 0; i < numRows; i++) {
            resRows[i] = new StringBuilder();
        }
        
        int rowIdx = 0;
        int step = 1;
        for (int i = 0; i < len; i++) {
            resRows[rowIdx].append(s.charAt(i));
            if ((step == 1 && rowIdx == numRows - 1) || (step == -1 && rowIdx == 0)) {
                step = -1 * step;
            }
            rowIdx += step;
        }
        
        StringBuilder sb = new StringBuilder(resRows[0]);
        for (int i = 1; i < numRows; i++) {
            sb.append(resRows[i]);
        }
        
        return sb.toString();
    }
}
```

To make the code efficient, I use StringBuilder on each string concatnation.