+++
title = "Exploring Algorithm: Longest Palindromic Substring"
date  = "2015-12-12T00:00:00-00:00"
tags = ["algorithm", "string problem"]
slug= "algorithm_string_longest_palindromic_substring"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/longest-palindromic-substring/).

```markdown
Given a string S, find the longest palindromic substring in S.
You may assume that the maximum length of S is 1000,
and there exists one unique longest palindromic substring.
```

#### Solution
A palindrome is a string that reads the same backward or forward, like
`CABAC` and `CABBAC`. `CABAC` has an odd length and the center is `B` `CABBAC` has an even length and the center is `BB`. One important fact about a palindrom is, that has line symmetry where the line is a center character (or 2 character when it has an even length).

So, we can iterate each element `S[i]` of the given string, and regard `S[i]` as a center of some palindrome, then we can expand the palindrome one by one, as long as **the left side element and the right side one has the same character** as below.

```markdown
  <-------------------> step3. length = 5
      <----------->     step2. length = 3
          <--->         step1. length = 1

+-+---+---+---+---+---+-+
  |   |   |   |   |   |
  | Y | X |   | X | Y |
  |   |   |   |   |   |
+-+---+---+---+---+---+-+
```

We have to consider also the case a palindrome has an even length. So in the same way, we can regard `S[i]` and `S[i+1]` as a center of a palindrome and try expanding it as below. 

```markdown
     <----------------------> step3. length = 6
         <-------------->     step2. length = 4
             <------->        step1. length = 2
-+---+---+---+---+---+---+---+-
 |   |   |   |   |   |   |   |
 |   | Y | X |   |   | X | Y |
 |   |   |   |   |   |   |   |
-+---+---+---+---+---+---+---+-
```

Note: **S[i] should equal to S[i+1] in this case.**

A code in Python:

```python
class Solution(object):
    def longestPalindrome(self, s):
        ml, mr, mlen = 0, 0, 0
        ls = len(s)
        for i in range(ls):
            if 2*(ls-i-1)+1 < mlen: break
            
            s1l, s1r = self.extendStr(s, i, i)
            s2l, s2r = self.extendStr(s, i, i + 1)
            
            if (s1r-s1l+1) > mlen: ml, mr, mlen = s1l, s1r, s1r-s1l+1
            if (s2r-s2l+1) > mlen: ml, mr, mlen = s2l, s2r, s2r-s2l+1
        return s[ml:mr+1]
    
    # return maximum lpos, rpos
    def extendStr(self, s, lpos, rpos):
        leng = len(s)
            
        while lpos >= 0 and rpos <= leng - 1 and s[lpos] == s[rpos]:
            lpos -= 1
            rpos += 1

```

To iterate each element, we should take `O(N)` time comlexity. And each expansion trial takes also `O(N)` at most, so as a whole, time complexicy is `O(N^2)`.  

To make it little more efficient, I set a break condition at the first line in the iteration loop and prevent keep searching a palindrome if the possible longest palindrome with a center `S[i]` can never be longer than the current max palindrome.

The space complexity is `O(1)` since we don't use extra space.

A code in Java:

```java
public class Solution {
    public String longestPalindrome(String s) {
        String res = "";
        int len    = s.length();
        
        for (int i = 0; i < len; i++) {
            if (2 * (len - i - 1) + 1 < res.length()) break;
            
            String res1 = extendStr(s, i, i);
            String res2 = extendStr(s, i, i+1);
            
            if (res1.length() > res.length()) res = res1;
            if (res2.length() > res.length()) res = res2;
        }
        return res;
    }
    
    public String extendStr(String s, int leftPos, int rightPos) {
        int len = s.length();
        
        while (leftPos >= 0 && rightPos <= len-1 && s.charAt(leftPos) == s.charAt(rightPos)) {
            --leftPos;
            ++rightPos;
        }
        return s.substring(leftPos+1, rightPos);
    }
}
```

On this java code, the space complexity is `O(N)` because I use String to hold the current maximum palindrome during an iteration.