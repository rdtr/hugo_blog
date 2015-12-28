+++
title = "Exploring Algorithm: Longest common substrings"
date  = "2015-11-02T00:00:00-00:00"
tags = ["algorithm", "string problem", "dynamic programming"]
slug= "algorithms_dp_longest_common_substring"
+++

#### Problem
```markdown
Given two strings, return a length of the longest substring.  
e.g) If "abcdefg" and "cdeg" are given, the longest substring is "cde", therefore return 3.   
```

#### Solution
This is a popular DP algorithm problem.

Let's say strings are `S1` and `S2`, a length of each string is `M` and `N`.
We can just check all substrings of `S1` and check if each substring exist in `S2`, and return the longest existing substring, but this takes `O(M^2*N)` time complexitiry.

To reduce the complexity, we store some information in `m[M][N]` while `m[i][j]` stores which index of a common substring `S1[i]` and `S2[j]` are on. If they are not on any common substring, just store 0.

![example](http://f.st-hatena.com/images/fotolife/r/rdtr/20151102/20151102165321.png?1446512129)

On the example above, `m[3][1] = 2` because `S1[3] = S2[1] = 'd'` and `m[2][0] = 1`, that is to say, `S1[3]` and `S2[1]` is the 2nd index of the common substring.  
In the same way, we can see `m[i][j]` would be `m[i-1][j-1]+1` if `S1[i] == S2[j]`, otherwise `0`.

This is the botto-up approach of dynamic programming. A code in python is written as below:

```python
def LCS(s1, s2):
    if not s1 or not s2:
        return 0

    len1, len2 = len(s1), len(s2)

    # memorization
    m = []
    for i in range(len1):
        m.append([0]*len2) #m[len1][len2]

    lcs = 0

    # set 1st index value
    for i, ch in enumerate(s1):
        if ch == s2[0]:
            m[i][0] = 1
            lcs = 1
    for i, ch in enumerate(s2):
        if ch == s1[0]:
            m[0][i] = 1
            lcs = 1

    # m[i][j] stands for s1[i] and s2[j]
    # is the m[i][j] th bit of a common substring
    for i in range(1, len1):
        for j in range(1, len2):
            if s1[i] == s2[j]:
                m[i][j] = m[i-1][j-1] + 1
                lcs = max(m[i][j], lcs)
            else:
                m[i][j] = 0
    return lcs
```

In this case, the time and space complexity are both `O(MN)`

Jave version is below:

```java
public class Solution
{
  public static int LCS(String s1, String s2) {
    int l1 = s1.length();
    int l2 = s2.length();
    if (l1 == 0 || l2 == 0) return 0; // no common strings

    // memorization
    int[][] m = new int[l1][l2];
    int maxLen = 0;

    // set 1st index value
    for (int i = 0; i < l1; i++) {
      if (s1.charAt(i) == s2.charAt(0)) {
        m[i][0] = 1;
        maxLen = 1;
      } else m[i][0] = 0;
    }
    for (int i = 0; i < l2; i++) {
      if (s2.charAt(i) == s1.charAt(0)) {
        m[0][i] = 1;
        maxLen = 1;
      } else m[0][i] = 0;
    }

    // m[i][j] stands for s1.charAt(i) and s2.charAt(j)
    // is the m[i][j] th bit of a common substring
    for (int i = 1; i < l1; i++) {
      for (int j = 1; j < l2; j++) {
        if (s1.charAt(i) != s2.charAt(j)) m[i][j] = 0;
        else {
          m[i][j] = m[i-1][j-1] + 1;
          maxLen = (m[i][j] > maxLen)? m[i][j] : maxLen;
        }
      }
    }
    return maxLen;
  }
}
```