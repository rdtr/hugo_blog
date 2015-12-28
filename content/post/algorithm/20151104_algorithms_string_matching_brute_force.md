+++
title = "Exploring Algorithm: String matching: brute force"
date  = "2015-11-04T00:00:00-00:00"
tags = ["algorithm", "string problem"]
slug= "algorithms_string_matching_brute_force"
+++

#### Problem

```markdown
Check whether string T is a substring of another string S.  
If true, return the first index of where it matches, otherwise return -1.
Example: If S = "ABCDE" and T = "CD", a return value is 2.
```

#### Solution
We can solve this in more efficient way, but first let me write about the basic way, brute force. Do you think it's trivial? Then you can skip this article:)

![example1](http://f.st-hatena.com/images/fotolife/r/rdtr/20151106/20151106184126.png?1446864100)

We will just loop through each index in S, and every time we reach one index i,   
we check whether `S[i] == T[0], S[i+1] == T[1], ...` until the end of T.
If we can reach to the end of T, that means we find T in S, so we return i. Otherwise proceed to the next index. A python code is below:

```python
def index_of(string, target):
    leng_s = len(string)
    leng_t = len(target)
    if leng_s == 0 or leng_t == 0:
        return -1
    
    for i in range(leng_s):
        j = 0
        if i + leng_t > leng_s:
            return -1
        while j < leng_t:
            if string[i+j] == target[j]:
                j += 1
            else:
                break
        if j == leng_t:
            return i
```

Time complexicity is `O(MN)` while M and N is the length of each string.
Space complexicity is `O(1)`.

A code in Java is below:

```java
class Solution
{
	public static int indexOf(String s, String t) {
	    int s_length = s.length();
	    int t_length = t.length();
	    if (s_length == 0 || t_length == 0) return -1;
	    
	    for (int i = 0; i < s_length; i++) {
	        if (i + t_length > s_length) return -1;
	        
	        int j = 0;
	        while (j < t_length) {
	            if (s.charAt(i+j) == t.charAt(j)) {
	                j++;
	                continue;
	            }
	            break;
	        }
	        if (j == t_length) return i;
	    }
	    return -1;
	}
}
```