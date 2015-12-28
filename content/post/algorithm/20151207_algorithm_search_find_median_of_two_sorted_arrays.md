+++
title = "Exploring Algorithm: Median of Two Sorted Arrays"
date  = "2015-12-07T00:00:00-00:00"
tags = ["algorithm", "search problem"]
slug= "algorithm_search_median_of_two_sorted_arrays"
+++

#### Problem
This problem is quoted from [LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/).

```markdown
There are two sorted arrays nums1 and nums2 of size m and n respectively.
Find the median of the two sorted arrays.
The overall run time complexity should be O(log(m+n)).
```

#### Solution
When we see the description of the problem that the run time can be `logX`, we can easily assume this problem requires us to do some binary-search variant task. But that is actually the most difficult part of this problem to think up how to apply binary-search.

Let's start with a definition of the median. Suppose `L` is the length of some array `A` and if `N % 2 = 0`, then the median would be `(A[L/2] + A[L/2 - 1]) / 2`, otherwise the median is just `A[L/2]`.

```markdown
+---+---+---+---+        +---+---+---+
| 0 | 1 | 2 | 3 |        | 0 | 1 | 2 |
+---+-+-+-+-+---+        +---+-+-+---+
      ^   ^                    ^
      |   |                    |
median = (A[1] + A[2])/2   median = A[1]
```

Then our goal is to **find A[k] while A is an sorted single array of all elements both in nums1 and nums2 and k = L / 2 and (L / 2) - 1**. How we can do this?

![example1](http://f.st-hatena.com/images/fotolife/r/rdtr/20151207/20151207181843.png?1449542061)

By following the basic principle of binary search, let's divide nums1 and nums2 while choosing an element which is at its center position as a pivot(`a` and `b`). As described in the example above, when we say nums1 has length `N` and nums2 has `M`, a number of elements on the left side of pivot is at `N / 2` and `M / 2`, and on the right side, `N - N/2 - 1` and `M - M/2 - 1`.

If you aren't sure this is correct, you can check this by example above. If `N = 8`, the pivot = A[4] then the left side has 4 elements (from A[0] to A[3]) and the right side has 3 elements (from A[5] to A[7]). If `M = 5`, the pivot = B[2] and the left side has 2 elements and the right side also has 2 elements. We can say our equation holds in both cases when M and N is an even number, and an odd one.

Imagine now we have one array `A` which has all elements appear both in nums1 and nums2. Let's assume `b` is greater than `a`.

![example2](http://f.st-hatena.com/images/fotolife/r/rdtr/20151208/20151208012310.png?1449566637)

We don't know at which exact position do two pivots(a and b) we chose are. But we can certainly say that

- `N / 2` elements are on the left side of (a)        ... (1)
- `N - N/2 - 1` elements are on the right side of (a) ... (2)
- `M / 2` elements are on the left side of (b)        ... (3)
- `M - M/2 - 1` elements are on the right side of (b) ... (4)

If we have to find an element of an index `k` from this array, we need to think about

> Which part among those 4 parts is the one `k` can never be contained?.

For example, on the left side of `b` in the graph above, there are at least `N / 2 + M / 2 + 1` elements because (1), (3), and `a` are surely contained in that part. In other words, **if k <= (N/2 + M/2), we are sure that A[k] is at the left side of `b`. Therefore, A[k] is never be in (4)**.

In the same way, on the right side of `a`, there are at least `(N - N/2 - 1) + (M - M/2 - 1) + 1 = N - N/2 + M - M/2 - 1` elements because (2), (4), and `b` are surely in there. So, what is the possible maximum index of `a` in the arary `A`? That is `(N + M) - (N - N/2 + M - M/2 - 1) - 1 = N/2 + M/2`(*1).
Therefore, **if k > (N/2 + M/2), we are sure that A[k] is at the right side of `a` thus never be contained in (1)**. Note that in this case, we discard the part which contains numbers which are all smaller than `A[k]`. Then we have to set a new `k` as `k = k - N/2` because `N/2` elements are discarded.

So our strategy is 

```
if k <= (N/2 + M/2) then discard (4) and go back to the first step,
choose a new pivot from nums1 and nums2, and search k th element.
otherwise (k > N/2 + M/2) discard (1) and go back to the first step,
find A[k - N/2].
```

If `a` is greater than `b`, we can just think in the opposite way. That is how we apply binary search to this problem. Regarding the base case, when either nums1 or nums2 becomes empty as a result of discard, we can specify `k`th index of non-empty ones.

In the following code, I achieve that by adjusting an index of start and end on each recurrsion.

Python code:

```python
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        len1, len2 = len(nums1), len(nums2)
        leng = len1 + len2
        mid  = leng / 2
        res = self.findKth(nums1, 0, len1-1, nums2, 0, len2-1, mid)
        if leng % 2 == 0:
            res = (res + self.findKth(nums1, 0, len1-1, nums2, 0, len2-1, mid - 1)) / 2.0
        return res
        
    def findKth(self, nums1, start1, end1, nums2, start2, end2, k):
        len1, len2 = end1 - start1 + 1, end2 - start2 + 1
        if len1 == 0: return nums2[start2+k]
        if len2 == 0: return nums1[start1+k]
        
        mid1, mid2 = start1 + len1/2, start2 + len2/2
        dis1, dis2 = mid1 - start1 + 1, mid2 - start2 + 1

        if k <= len1/2 + len2/2:
            if nums1[mid1] >= nums2[mid2]:
                return self.findKth(nums1, start1, mid1-1, nums2, start2, end2, k)
            else: return self.findKth(nums1, start1, end1, nums2, start2, mid2-1, k)
        else:
            if nums1[mid1] >= nums2[mid2]:
                return self.findKth(nums1, start1, end1, nums2, mid2+1, end2, k-dis2)
            else: return self.findKth(nums1, mid1+1, end1, nums2, start2, end2, k-dis1)
```

Java code:

```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len1 = nums1.length;
        int len2 = nums2.length;
        int mid = (len1 + len2) / 2;
        
        double res = 0.0;
        res = 1.0 * findKthSmallest(nums1, 0, len1-1, nums2, 0, len2-1, mid);
        if ((len1+len2) % 2 == 0) {
            double res2 = 1.0 * findKthSmallest(nums1, 0, len1-1, nums2, 0, len2-1, mid-1);
            res += res2;
            res /= 2.0;
        }
        return res;
    }
    
    public int findKthSmallest(int[] nums1, int start1, int end1, int[] nums2, int start2, int end2, int k) {
        int len1 = end1 - start1 + 1;
        int len2 = end2 - start2 + 1;
        int mid1 = start1 + len1 / 2;
        int mid2 = start2 + len2 / 2;
        int dis1 = mid1 - start1 + 1;
        int dis2 = mid2 - start2 + 1;
        
        if (len1 == 0) return nums2[start2+k];
        if (len2 == 0) return nums1[start1+k];
        
        if (k <= len1 / 2 + len2 / 2) {
            if (nums1[mid1] >= nums2[mid2]) {
                return findKthSmallest(nums1, start1, mid1-1, nums2, start2, end2, k);
            } else {
                return findKthSmallest(nums1, start1, end1, nums2, start2, mid2-1, k);
            }
        } else {
            if (nums1[mid1] >= nums2[mid2]) {
                return findKthSmallest(nums1, start1, end1, nums2, mid2+1, end2, k-dis2);
            } else {
                return findKthSmallest(nums1, mid1+1, end1, nums2, start2, end2, k-dis1);
            }
        }
    } 
}
```

On each recurrsion, we reduce the number of total elements by `N/2` or `M/2`. Therefore, time complexity is `O(logM +logN)`. Spece complexity is the same because we have to call the function recurrsively at the same number of time.

You might think "Hey, the problem requires `O(log(M+N))`, it's different from yours". Yes, but 

```
logM + logN = log(MN)
           <= log(M^2 + N^2 + 2MN) (∵ M >= 0, N>= 0)
            = log((M+N)^2)
            = 2log(M+N)
```

So we can say `O(logM + logN) <= O(log(M+N))`, which shows our algorithm meets the requirement.

This problem is quite difficult in my opinion!

---
(*1)  
If you don't understand this equation, imagine you have an array with length 10 and some element X in it. If there're 6 elements on the right side of X, what is the position of X? That is 10 - 6 - 1 = 3.