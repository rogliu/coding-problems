# Subtree of Another Tree

### Problem

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

### Example 1

![subtree1-tree.jpeg](Subtree%20of%20Another%20Tree%2046feb3a2e8854258a88973fd10464f8b/subtree1-tree.jpeg)

```
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```

### Example 2

![subtree2-tree.jpeg](Subtree%20of%20Another%20Tree%2046feb3a2e8854258a88973fd10464f8b/subtree2-tree.jpeg)

```
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```

### Constraints

- The number of nodes in the `root` tree is in the range `[1, 2000]`.
- The number of nodes in the `subRoot` tree is in the range `[1, 1000]`.
- `104 <= root.val <= 104`
- `104 <= subRoot.val <= 104`

### Solution

- iterate through the root binary tree
- for each node, check if that tree is the same tree
- can iterate through the tree using breadth-first search
- check if the same tree using recursive depth-first search

### Complexity Analysis

- Time: O(m*n) where m is the number of nodes in the tree and n is the number of nodes in the subtree
- Space: O(height of tree * height of subtree)

### Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        def same_tree(node_one, node_two):
            if not node_one and not node_two:
                return True
            if not node_one and node_two:
                return False
            if node_one and not node_two:
                return False
            if node_one.val != node_two.val:
                return False
            
            return same_tree(node_one.left, node_two.left) and same_tree(node_one.right, node_two.right)
    
        
        queue = collections.deque([root])
        
        while queue:
            node = queue.popleft()
            if node.val == subRoot.val:
                if same_tree(node, subRoot):
                    return True
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        return False
```

```jsx
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        
        def same_tree(node_one, node_two):
            if not node_one and not node_two:
                return True
            if node_one and node_two and node_one.val == node_two.val:
                return same_tree(node_one.left, node_two.left) and same_tree(node_one.right, node_two.right)
            
            return False
        
        if not subRoot:
            return True
        if not root:
            return False
        
        if same_tree(root, subRoot):
            return True
        
        return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)
```