# Validate Binary Search Tree


### Problem

Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

### Example 1

```
Input: root = [2,1,3]
Output: true
```

### Example 2

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

### Constraints

- The number of nodes in the tree is in the range `[1, 10^4]`.
- `231 <= Node.val <= 231 - 1`

### Notes

- node must strictly be greater or less than, cannot be equal to

### Solution: Depth-first Search

- each node by definition of a BST has a max value and min value
- traverse the tree
    - return true if node is None
    - return false if node is not within range
    - traverse left & right subtrees if node is within range
        - left subtree, the max value is the root value
        - right subtree, the min value is the root value

### Complexity Analysis

- Time: O(n) since it traverses all the nodes
- Space: O(h) if tree is balanced, could be O(n) in worst case (unbalanced tree)

### Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        lower, higher = float("-inf"), float("inf")
        
        def dfs(node, lower, higher):
            if node is None:
                return True
        
            if node.val <= lower or node.val >= higher:
                return False
            
            return dfs(node.left, lower, node.val) and dfs(node.right, node.val, higher)
        
        return dfs(root, lower, higher)
```