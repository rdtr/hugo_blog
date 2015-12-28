+++
title = "Exploring Algorithm: Longest Substring Without Repeating Characters"
date  = "2015-11-10T00:00:00-00:00"
tags = ["algorithm", "string problem"]
slug= "algorithm_string_longest_substring_without_repeating_characters"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/).

```markdown
Given a string, find the length of the longest substring without repeating characters.
For example, the longest substring without repeating letters for "abcabcbb" is "abc",
which the length is 3. For "bbbbb" the longest substring is "b", with the length of 1.
```

#### Solution
We can solve this problem just one iteration through the string given.  
We use HashMap to check if a character repeats in a previous sequence.

![example](http://f.st-hatena.com/images/fotolife/r/rdtr/20151111/20151111133323.png?1447277616)

An example is shown at above. Until we find a character appears twice we can just increment the current length, then if at index `j` we detect the character is already apperaed at index `i`, the longest substring without repeating characters is `S[i+1]` to `S[j]`.

And in an iteration after this point, we can consider only from at index `i+1`. So we have to store this index as `start_index` of the current longest substring, and if the character appeeared at `k` such that `k < start_index`, we can just ignore that.

A code in Python:

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        leng = len(s)
        if leng == 0: return 0
        
        m = {}
        
        maxLen = 0
        curLen = 0
        startIdx = 0
        for curIdx, ch in enumerate(s):
            if ch in m and m[ch] >= startIdx:
                startIdx = m[ch] + 1
                m[ch] = curIdx
                curLen = curIdx - startIdx + 1                
            else:
                m[ch] = curIdx
                curLen += 1
                if curLen > maxLen: maxLen = curLen
        return maxLen
```

A code in Java:

```java

public class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> m = new HashMap<>();
        int curLength  = 0;
        int maxLength  = 0;
        int startIndex = 0;
        
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (m.containsKey(ch) && m.get(ch) >= startIndex) {
                startIndex = m.get(ch) + 1;
                curLength  = i - startIndex + 1;
            } else if (++curLength > maxLength) maxLength = curLength;
            m.put(ch, i);
        }
        return maxLength;
    }
}
```

Time complexicity: `O(N)`, space complexicity: `O(N)` while N is a length of the string given.