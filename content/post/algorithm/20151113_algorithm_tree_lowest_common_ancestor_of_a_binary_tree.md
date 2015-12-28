+++
title = "Exploring Algorithm: Lowest Common Ancestor of a Binary Tree"
date  = "2015-11-13T00:00:00-00:00"
tags = ["algorithm", "tree problem"]
slug= "algorithm_tree_lowest_common_ancestor_of_a_binary_tree"
+++

#### Problem
This problem is borrowed from [LeetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/).

```markdown
Given a binary tree, find the lowest common ancestor (LCA)
of two given nodes in the tree.

According to the definition of LCA on Wikipedia:
"The lowest common ancestor is defined between two nodes
v and w as the lowest node in T that has both v and w as descendants
(where we allow a node to be a descendant of itself)."

        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
         
For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3.  
Another example is LCA of nodes 5 and 4 is 5, since a node can be a
descendant of itself according to the LCA definition.
```

#### Solution
Let's say one node is `P` and another is `Q`. To solve this problem, first we construct a map that has each node in a tree as a key, and its parent as a value. Then you can trace the tree from `P` to root, and `Q` to root. Then what we have to do is find the last common node between these two paths.
If we check each node in paths one by one, it takes `O(MN)`(`M`, `N` are a length of each path). To reduce the complexitiy, we can create a hashmap of one path and iterate another, while checking each node exists on the hashmap.

Python code:

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from collections import deque
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        node_parent = {} # key: node, value: node's parent
        queue = deque([root])
        # traverse all nodes by BST and construct node_parent map
        while queue:
            node = queue.popleft()
            if node.left:
                node_parent[node.left] = node
                queue.append(node.left)
            if node.right:
                node_parent[node.right] = node
                queue.append(node.right)
            
        # constcurt node existence on paths of p
        # In python, we can use set instead of dictionary
        # because it also takes O(1) to check value existence
        pParents = set()
        current = p
        while current in node_parent:
            pParents.add(current)
            current = node_parent[current]
        else:
            pParents.add(current)
        
        # find first common node on paths of p and q
        current = q
        while current not in pParents:
            current = node_parent[current]
        return current
```

Java code:

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        Map<TreeNode, TreeNode> parentMap = new HashMap<>();
        
        // store parentMap while traversing a tree in BST
        Deque<TreeNode> queue = new ArrayDeque();
        queue.offerLast(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.pollFirst();
            if (node.left != null) {
                queue.offerLast(node.left);
                parentMap.put(node.left, node);
            }
            if (node.right != null) {
                queue.offerLast(node.right);
                parentMap.put(node.right, node);
            }
        }
        
        // store nodes from p to root 
        Set<TreeNode> pNodes = new HashSet<>();
        TreeNode pNode = p;
        while (pNode != null) {
            pNodes.add(pNode);
            pNode = parentMap.get(pNode);
        }
        
        // find the first common node while traversing from q to root
        if (pNodes.contains(q)) return q;
        TreeNode qNode = q;
        while (true) {
            if (pNodes.contains(qNode)) return qNode;
            qNode = parentMap.get(qNode);
        }
    }
}
```

Time complexicity: `O(K)` while K is a number of elements in a tree given, because we have to check all nodes while constructing a node->parent hashmap.  
Space complexicity is also `O(K)` because we need as many space as a number of nodes to store node->parent hashmap.

It's also possible that we store all paths from root to `P` and root to `Q` as an array individually. In this case, we can iterate both arrays from the first element (which should be the root of the tree) and if an element becomes different between these 2 arrays, the lowest common ancestor would be exactly the previous element of it.

Python code: 

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from collections import deque

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root: return None
        if not p or not q: return None
        
        pQueue = []
        self.findPath(root, p, pQueue)
        qQueue = []
        self.findPath(root, q, qQueue)
        
        common = pQueue[0]
        pLen, qLen = len(pQueue), len(qQueue)
        i = 0
        
        # iterate from root to P, Q and return the last common node
        while True:
            pRoot = pQueue[i]
            qRoot = qQueue[i]
            if pRoot != qRoot:
                return common
            # when one is a parent of another
            if i == pLen - 1 or i == qLen - 1:
                return pRoot
            common = pRoot
            i += 1
        
    # returns a path from root as a queue
    def findPath(self, root, node, currentPath):
        if root == node:
            currentPath.append(root.val)
            return True
        if root.left:
            currentPath.append(root.val)
            if self.findPath(root.left, node, currentPath):
                return True
        if root.right:
            currentPath.append(root.val)
            if self.findPath(root.right, node, currentPath):
                return True
        currentPath.pop()
        return False
```

Java code:

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        Deque<TreeNode> pPath = new ArrayDeque<>();
        Deque<TreeNode> qPath = new ArrayDeque<>();
        Solution.constructPath(root, p, pPath);
        Solution.constructPath(root, q, qPath);
        
        TreeNode commonNode = root;
        TreeNode pNode;
        TreeNode qNode;
        
        while (true) { // find the last common node
            pNode = pPath.pollFirst();
            qNode = qPath.pollFirst();
            
            if (pPath.isEmpty()) return pNode;
            if (qPath.isEmpty()) return qNode;
            if (pNode != qNode) return commonNode;
            
            commonNode = pNode;
        }
    }
    
    public static boolean constructPath(TreeNode root, TreeNode target, Deque<TreeNode> path) {
        if (root == target) {
            path.offerLast(root);
            return true;
        };
        
        if (root.left != null) {
            path.offerLast(root.left);
            if (Solution.constructPath(root.left, target, path)) return true;
        }
        if (root.right != null) {
            path.offerLast(root.right);
            if (Solution.constructPath(root.right, target, path)) return true;
        }
        // if not found, reddress the state of path by deleting the adding one
        path.pollLast();
        return false;
    }
}
```

It's important to remove the last element at the last line of the recurrsive `findPath` / `constructPath` method. This restores elements of a path array when getting back from a wrong node.

 