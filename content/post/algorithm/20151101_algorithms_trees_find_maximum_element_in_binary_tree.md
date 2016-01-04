+++
title = "Exploring Algorithm: Finding the maximum element in binary tree"
date  = "2015-11-01T00:00:00-00:00"
tags = ["algorithm", "tree problem"]
slug= "algorithms_tree_find_maximum_element_in_binary_tree"
+++

#### Problem

```markdown
Find maximum element in binary tree.
```

#### Solution

This is a fairy simple and straigt-forward problem, because we eventually have to check all elements in the binary tree (Note: the tree is not a **binary search** tree)

So we can traverse all elements by BFS, compare value everytime we visit a node while storing the max value at that time, then finally return it.

The code in Python would be like:

```python
'''
The definition of tree node is like:
class Node():
    def __init__(self, value):
        self.value, self.left, self.right = value, None, None

    def setValue(self, value):
        self.value = value

    def setLeft(self, left):
        self.left = left

    def setRight(self, right):
        self.right = right
'''

def find_max(root):
    if not root:
        return None
    queue = [root]
    max_value = root.value

    while queue:
        new_queue = []
        for node in queue:
            if node.left:
                max_value = max(max_value, node.left.value)
                new_queue.append(node.left)
            if node.right:
                max_value = max(max_value, node.right.value)
                new_queue.append(node.right)
        queue = new_queue
    return max_value
```

The time complexicity is `O(N)` while N is a number of nodes of the tree, and the space complexicity is also O(N) (Imagine a case that root has all nodes as children).

Or we can use recurrsive function that searches the biggest number among the node itself, its left child, and right child from a given node.

```java
def find_max_value_from_parent_and_children(node):
    if not node:
        return None

    if node.left and node.right:
        return max(node.value, max(find_max_value_from_parent_and_children(node.left), \
                                   find_max_value_from_parent_and_children(node.right)))
    elif node.left:
        return max(node.value, find_max_value_from_parent_and_children(node.left))
    elif node.right:
        return max(node.value, find_max_value_from_parent_and_children(node.right))
    else:
        node.value

def find_max(root):
    return find_max_value_from_parent_and_children(root)
```

The time complexicity and the space complexicity are both `O(N)` in this case too.

Java version of the code above is written in below:

```java
/* The definition of a node of tree is like:
public class Node {
    public int value;
    public Node left;
    public Node right;

    public Node(int value) {
        this.value = value;
        left       = null;
        right      = null;
    }

    public setValue(int value) {
        this.value = value;
    }

    public setLeft(Node left) {
        this.left = left;
    }

    public setRight(Node right) {
        this.right = right;
    }
}
*/

public class Solution {
     public static int findMax(Node root){
        if (root == null) return Integer.MIN_VALUE;

        int maxValue = root.value;
        List<Node> lst = new ArrayList<Node>(root);
        while (!lst.isEmpty()) {
            List<Node> newLst = new ArrayList<Node>();

            if (lst.left != null) {
                newLst.add(lst.left);
                if (lst.left > maxValue) maxValue = lst.left;
            if (lst.right != null) {
                newLst.add(lst.right);
                if (lst.right > maxValue) maxValue = lst.right;
            }
            lst = newLst;
        }
        return maxValue;
     }
}
```

With a recurrsive function:

```java
public class Solution {
    public static int findMaxFromParentAndChildren(Node node) {
        if (node == null) return Integer.MIN_VALUE;

        if (node.left != null && node.right != null) {
            return Math.max(node.value,
                            Math.max(Solution.findMaxFromParentAndChildren(left),
                                     Solution.findMaxFromParentAndChildren(right)));
        } else if (node.left != null) {
            return Math.max(node.value,
                            Solution.findMaxFromParentAndChildren(left));
        } else if (node.right != null) {
            return Math.max(node.value,
                            Solution.findMaxFromParentAndChildren(right));
        } else {
            return node.value;
        }
    }

    public static int findMax(Node root){
        return Solution.findMaxFromParentAndChildren(root);    
    }
}
```
