+++
title = "Exploring Alrorithm: Longest common subsequence"
date  = "2015-11-03T00:00:00-00:00"
tags = ["algorithm", "string problem", "dynamic programming"]
slug= "algorithms_dp_longest_common_subsequence"
+++

#### Problem

```
The longest common subsequence of string S1 and S2 is the longest group of
character elements from S1 and S2 that are common between the two groups and
in the same order in each group.
For example, the sequences "1224533324" and "69283746" have an LCS of "234".
```

#### Solution
This is also a popular DP algorithm problem.
It is similar as [Longest Common Substring](http://blog.rdtr.net/post/algorithm/algorithms_dp_longest_common_substring/) problem, but unlike substrings, **subsequences are not required to occupy consecutive positions within the original sequences**.

Let's consider the example cale of the problem. Now we have a table below, `S1` string is on the row and `S2` is on  the column:

![example1](http://f.st-hatena.com/images/fotolife/r/rdtr/20151103/20151103225640.png?1446620237)

For instance, an orange cell `Table[4][2]` represents the length of LCS of `S1.subString(0, 5) = "12245"` and `S2.subString(0, 3) = "692"`. Our task is fill this table with their each own LCS length.

![example2](http://f.st-hatena.com/images/fotolife/r/rdtr/20151103/20151103230039.png?1446620452)

We can start with the 0th row and 0th column. Regarding the 0th row, this is a problem asking `Does "1224533324" include a character "6"? Mark "0" in the cell until "6" is found and mark "1" after found`. Unfortunately we cannot found `"6"` in this case, then all cells filled with `"0"`. It's the same case in the 0 th column.

![example3](http://f.st-hatena.com/images/fotolife/r/rdtr/20151103/20151103233524.png?1446622533)

Now we proceed to `Table[1][1]`. In this case, `S1[1] != S2[1]` so we have to pick the longest LCS amoung the previous ones, that is `Table[0][1]`, `Table[1][0]`, and `Table[0][0]`. But we can say `Table[0][1] >= Table[0][0]` and `Table[1][0] >= Table[0][0]` because when `S1[1] == S2[0]` or `S1[0] == S2[1]`, `Table[0][1]` or `Table[1][0]` is incremented from `Table[0][0]`. So anyway we can ignore `Table[0][0]` and just check the left cell and the upper cell, then pick the larger one.

![example4](http://f.st-hatena.com/images/fotolife/r/rdtr/20151103/20151103235353.png?1446623644)

Next, `Table[1][2]`. Now `S1[1] == S2[2]` so we can increment the length of the LCS, but at the same time we notice that **we can only jump diagonally from Table[0][1]** because as for `Table[0][2]` and `Table[1][1]`, `S1[1]` or `S2[2]` (= `"2"`) is already taken.

We can generalize the rule as follows:

```
Table[i][j] =
	Table[i-1][j-1] + 1 if S1[i] == S2[j]
	else
		Max(Table[i][j-1], Table[i-1][j])
```

By following the rule above, we can fill in the table as below:

![example5](http://f.st-hatena.com/images/fotolife/r/rdtr/20151104/20151104000153.png?1446624166)

Okay we can get the LCS length, but how we can print the LCS itself? Now you know the LCS length is incremented when we jump diagonally. In other words, to collect LCS characters, we have to collect points we have to jump  when creating a table above and reaches to the final LCS.

![example6](http://f.st-hatena.com/images/fotolife/r/rdtr/20151104/20151104013104.png?1446629490)

To check them, we can start with the very bottom right of the table. **At a point which we have to jump to, `S1[i] == S2[j]` should suffice**. Until reach such point, we would just go upper or left cell which have the same LCS value. Then if we find the point, we collect the character on the cell and jump to upper left cell. We continue this procedure until we reach the boundary of the table.

In the example above, `"4"`, `"3"`, `"2"` are collected, this is the LCS in reversed order.


In the code below, I use `m[i][j]` as a memorization of `Table[i][j]`.

```python

def LCS(s1, s2):
    l1, l2 = len(s1), len(s2)
    if l1 == 0 or l2 == 0: return ''
        
    # memorization of m[l1][l2]
    m = []
    for x in range(l1):
        m.append([0]*(l2))
    
    # Fill 0th row
    isSameFound = False
    for i in range(l1):
        if isSameFound or s1[i] == s2[0]:
            m[i][0] = 1
            isSameFound = True
    
    # Fill 0th column
    isSameFound = False
    for j in range(l2):
        if isSameFound or s2[j] == s1[0]:
            m[0][j] = 1
            isSameFound = True
        
    max_len = 0
    # m[i][j] stores the maximum length of subsequence of s1[:i+1], s2[:j+1]
    for i in range(0, l1-1):
        for j in range(0, l2-1):
            if s1[i+1] == s2[j+1]:
                m[i+1][j+1] = m[i][j] + 1
                max_len = max(m[i][j], max_len)
            else:
                m[i+1][j+1] = max(m[i][j+1], m[i+1][j])
    
    #If you want to know just the length of the lcs, return maxLen.
    #Here we'll try to print the lcs.
    lcs_str = ''
    i, j = l1-1, l2-1
    while i >= 0 and j >= 0:
        if s1[i] == s2[j]:
            lcs_str += s1[i] #or s2[j-1]
            i -= 1
            j -= 1
        else:
            if m[i-1][j] > m[i][j-1]:
                i -= 1
            else:
                j -= 1

    return lcs_str[::-1]
```

In this case, the time complexity is `O(MN) + O(M+N)` = `O(MN)`,  
the space complexity is `O(MN)`. (at most the size of `m[M][N]`).

Jave version is below:

```java
import java.lang.Math;

public class Solution {
  public static String LCS(String s1, String s2) {
    if (s1.isEmpty() || s2.isEmpty()) {
      return "";
    }
    int l1 = s1.length();
    int l2 = s2.length();

    // memorization
    int[][] m = new int[l1][l2];
	int maxLen = 0;
	
	// fill the 0th row
	boolean isFound = false;
	for (int i = 0; i < l1; i++) {
		if (isFound || s1.charAt(i) == s2.charAt(0)) {
		    m[i][0] = 1;
		    isFound = true;
		} else {
		    m[i][0] = 0;
		}
	}
	
	// fill the 0th column
    isFound = false;
	for (int j = 0; j < l2; j++) {
		if (isFound || s2.charAt(j) == s1.charAt(0)) {
		    m[0][j] = 1;
		    isFound = true;
		} else {
		    m[0][j] = 0;
		}
	}
    
    // m[i][j] stores the maximum length of subsequence until s1.subString(0, i+1), s2.subString(0, j+1)
    for (int i = 1; i < l1; i++) {
        for (int j = 1; j < l2; j++) {
            if (s1.charAt(i) == s2.charAt(j)) {
                m[i][j] = m[i-1][j-1] + 1;
                maxLen = Math.max(m[i][j], maxLen);
        }   else {
                m[i][j] = Math.max(m[i-1][j], m[i][j-1]);
            }
        }
    }
    
    // If you want to know just the length of the lcs, return maxLen.
    // Here we'll try to print the lcs.
    StringBuilder sb = new StringBuilder();
    int i = l1 - 1, j = l2 - 1;
    while (i > 0 && j > 0) {
      if (s1.charAt(i) != s2.charAt(j)) {
        if (m[i-1][j] > m[i][j-1]) i--;
        else j--;
      } else {
        sb.append(s1.charAt(i)); // or s2.charAt(j-1)
        i--; j--;
      }
    }
    
    return sb.reverse().toString();
  }
}
```