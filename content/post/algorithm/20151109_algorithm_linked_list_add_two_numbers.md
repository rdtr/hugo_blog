+++
title = "Exploring Algorithm: Add 2 numbers"
date  = "2015-11-09T00:00:00-00:00"
tags = ["algorithm", "linked list problem"]
slug= "algorithm_linked_list_add_two_numbers"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/add-two-numbers/).

```markdown
You are given two linked lists representing two non-negative numbers.
The digits are stored in reverse order and each of their nodes contain a single digit.
Add the two numbers and return it as a linked list.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
```

#### Solution
Just begin from a head of each linked list and sum up each element, then carry over `1` if the sum is greater than 9. When one of the lists reaches its tail, we can stop adding a value from the list. Then when both lists reach their end, we stop the process.

Note that when the last sum amount is greater than 9, we have to perform one more carry-over `1` to the tail of the result list.

A code in Python:

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        dummyHead = ListNode(0);
        curNode = dummyHead
        
        if not l1 and not l2: return None
        elif not l1:          return l2
        elif not l2:          return l1
        
        co = 0 # carry over from the previous node
        while l1 or l2:
            sum = 0
            if l1:
                sum += l1.val
                l1 = l1.next
            if l2:
                sum += l2.val
                l2 = l2.next
                
            sum += co
            co  = sum / 10
            sum = sum % 10
            
            curNode.next = ListNode(sum)
            curNode = curNode.next
        
        if co == 1:
            curNode.next = ListNode(1)
        
        return dummyHead.next
```

A code in Java:

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if (l1 == null && l2 == null) return null;
        else if (l1 == null) return l2;
        else if (l2 == null) return l1;
        
        ListNode dummyHead = new ListNode(0);
        ListNode curNode   = dummyHead;
        int co  = 0; // carry-over from previous nodes
        while (l1 != null || l2 != null) {
            int sum = 0;
            
            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }
            sum += co;
            
            co   = sum / 10;
            sum %= 10;
            
            curNode.next = new ListNode(sum);
            curNode = curNode.next;
        }
        
        if (co == 1) curNode.next = new ListNode(1);
        return dummyHead.next;
    }
}
```

Time complexicity: `O(N)`, space complexicity: `O(N)` while N is a length of the longer list between two given.  
To understand easily I store the result in a new linked-list, but actaully we don't need to use a new one, instead just store the result to `l1` or `l2` (That won't affect the result). Then, there is no extra space needed. 