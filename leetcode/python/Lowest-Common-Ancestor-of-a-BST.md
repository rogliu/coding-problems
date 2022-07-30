# Lowest Common Ancestor of a BST

### Problem

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

### Constraints

- The number of nodes in the tree is in the range `[2, 105]`.
- `109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the BST.

### Example 1

![binarysearchtree_improved.png](Lowest%20Common%20Ancestor%20of%20a%20BST%20a8c10e2428a84985b24de7c17e4b9386/binarysearchtree_improved.png)

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

### Example 2

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

### Example 3

```
Input: root = [2,1], p = 2, q = 1
Output: 2
```

### Notes

- need a way to traverse through the tree -> recursive depth-first search

### Solution

- when the root is greater than/equal to the smaller value and smaller than/equal to the greater value, its a valid ancestor
- when its smaller than the smaller value, search the right subtree
- when its greater than the greatest value, search the left subtree

### Complexity Analysis

- Time: O(h) if the tree is balanced, else O(n)
- Space: O(h) if the tree is balanced, else O(n)

### Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        """
        problem: find the lowest common ancestor of two given nodes in a BST
        binary search tree: a tree where all nodes to the left are smaller and all nodes to the right are greater
        
        need a way to traverse through the tree -> recursive dfs
            base cases
                when its greater than/equal to the smaller value and smaller than/equal to the greater value, its a valid ancestor
                when its smaller than the smaller value, search the right subtree
                when its greater than the greatest value, search the left subtree
            recursive statement
        
        note: utilize the fact that this is a binary search tree
        """
        
        lower = min(p.val, q.val)
        upper = max(p.val, q.val)
        if root.val >= lower and root.val <= upper:
            return root
        if root.val < lower:
            return self.lowestCommonAncestor(root.right, p, q)
        if root.val > upper:
            return self.lowestCommonAncestor(root.left, p, q)
```

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        cur = root
        while cur:
            if cur.val < p.val and cur.val < q.val:
                cur = cur.right
            elif cur.val > p.val and cur.val > q.val:
                cur = cur.left
            else:
                return cur
```