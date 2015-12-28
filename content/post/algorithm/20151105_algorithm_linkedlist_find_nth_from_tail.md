+++
title = "Exploring Algorithm: Find the Nth element from the tail of the linked list"
date  = "2015-11-05T00:00:00-00:00"
tags = ["algorithm", "linked list problem"]
slug= "algorithm_linkedlist_find_nth_from_tail"
+++

#### Problem

```markdown
Return N th node from the end of a linked list. 
If not found, return NULL.
```

#### Solution

One easy solution you may first think up is just go throuth the list to the end once, so you can get the length of the list M, so now you see that the element is `(N - M + 1) th` element from the head of the list.

A code in Python:

```python
# Class of linked list Node.
class Node():
    def __init__(self, val):
        self.next = None
        self.value = val
    
    def next(self):
        return self.next
    
    def setNext(self, nextNode):
        self.next = nextNode
        

def findElement(head, N):
    if head == None:
        return None
    
    curNode = head
    num = 1 # number of elements in list
    while curNode.next:
        num += 1
        curNode = curNode.next
    
    if num < N or N <= 0: # list is too short
        return None
    
    # get num - N + 1 node
    curNode = head
    curNum  = 1
    while curNum < num - N + 1:
        curNum += 1
        curNode = curNode.next
    return curNode.value
```

Time complexicity: `O(M)`, space complexicity: `O(1)`

A code in Java:

```java
// Class of linked list node
class Node {
    public int  value;
    public Node next;
    
    public Node(int value) {
        this.value = value;
        this.next = null;
    }
    
    public void setNext(Node nextNode) {
        this.next = nextNode;
    }
}

class Solution
{
	public static Node findNthNodeFromTail(Node head, int N) {
	    if (head == null) return null;
	    
        int length   = 1;
        Node curNode = head;
        while (curNode.next != null) {
            length++;
            curNode = curNode.next;
        } // now length = length of the given list
        
        if (length <= 0 || length < N) return null;
        
        int curLength = 1;
        curNode       = head;
        while (curLength < length - N + 1) {
            curNode = curNode.next;
            curLength++;
        }
        return curNode;
	}
}
```

This is one way to solve the problem, but we have to go through the same list twice.  
To avoid this, we can use two pointers p1 and p2. First we let p1 proceed to Mth point from the head, then set p2 to the head and let p1 and p2 one by one, until p1 reaches to the tail.  
At this time, p2 is on the Nth point from the tail.

A code in Python :

```python
# Class of linked list Node.
def findElement(head, N):
    if head == None:
        return None
    
    p1 = head
    num = 1
    while num < N:
        num += 1
        p1 = p1.next
        if p1 is None:
            return None
    
    # get num - N + 1 node
    p2 = head
    while not p1.next is None:
        p1 = p1.next
        p2 = p2.next
    return p2
```

This code searches through the given list only once.
Time complexicity: `O(M)`, space complexicity: `O(1)`

A code in Java:

```java
class Solution
{
	public static Node findNthNodeFromTail(Node head, int N) {
	    if (head == null) return null;
	    
        int length   = 1;
        Node p1 = head;
        while (length < N) {
            length++;
            p1 = p1.next;
            
            if (p1 == null) return null;
        } // now p1 is on the Nth node from the head
        
        Node p2 = head;
        while (p1.next != null) {
            p1 = p1.next;
            p2 = p2.next;
        }
        return p2;
	}
}
```

Like this problem, using two pointers and let them run through the same list with a different pace or position is sometime useful way to solve the problem.