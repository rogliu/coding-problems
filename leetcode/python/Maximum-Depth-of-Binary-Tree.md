# Maximum Depth of Binary Tree

### Problem

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

### Example 1

![tmp-tree.jpeg](Maximum%20Depth%20of%20Binary%20Tree%2099c6ba827af546fdbdb123d5a6baad0f/tmp-tree.jpeg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

### Example 2

```
Input: root = [1,null,2]
Output: 2
```

### Example 3

```
Input: root = []
Output: 0
```

### Example 1

```
Input: root = [0]
Output: 1
```

### Constraints

- The number of nodes in the tree is in the range `[0, 104]`.
- `100 <= Node.val <= 100`

### Solution

- Base case: if root is none, return 0
- call DFS on both sides
- Since we’re looking for the *maximum* depth, return the maximum of each side
- Add 1 to include the current valid node

```
Base case
If the root is None, return 0 
 Recursive case 
Return the maximum of the left and right children + 1
```

### Complexity Analysis

```
Time: O(N) since it traverses all of the nodes.
Space: O(N) recursive calls stored on the stack. at worst if it’s unbalanced, O(log(N)) average case if the tree is balanced
```

### Code: Recursive Depth-first Search

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
    
        if root is None:
            return 0
        
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```

### Code: Iterative Breadth-first Search

```jsx
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root is None:
            return 0
        
        from collections import deque
        
        queue = deque([(root, 1)])
        result = 0
        while queue:
            node, height = queue.popleft()
            result = max(result, height)
            if node.left:
                queue.append((node.left, height + 1))
            if node.right:
                queue.append((node.right, height + 1))
        
        return result
```
