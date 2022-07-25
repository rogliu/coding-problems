# Invert Binary Tree

### Problem

- Given the `root`of a binary tree, invert the tree, and return *its root*.

### Example 1

![invert1-tree.jpeg](Invert%20Binary%20Tree%20854bc6dab1da4e12abe46413717853d9/invert1-tree.jpeg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

### Example 2

![invert2-tree.jpeg](Invert%20Binary%20Tree%20854bc6dab1da4e12abe46413717853d9/invert2-tree.jpeg)

```
Input: root = [2,1,3]
Output: [2,3,1]
```

### Example 3

```
Input: root = []
Output: []
```

### Constraints

- The number of nodes in the tree is in the range `[0, 100]`.
- `100 <= Node.val <= 100`

### Solution:

```
The base case is if the root is None, return None
Then we need to swap the nodes
After we need to call the function recursively on both sides 
then we return the root
```

### Complexity Analysis

```
Time: O(N) since we traverse the entire tree
Space: O(h) if the tree is balanced or O(N) if the tree is unbalanced
i.e. unbalanced if there are only right nodes in the tree
```

### Code: Recursive

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # base case
        if root is None:
            return None
        # swap nodes
        root.left, root.right = root.right, root.left
        # recursive step
        self.invertTree(root.left)
        self.invertTree(root.right)
        # return root
        return root
```

### Code: Iterative

```jsx
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        from collections import deque
        # if empty list, still enters the loop so you need this base case
        if root is None:
            return None
        
        queue = deque([root])
        
        while queue:
            current_node = queue.popleft()
            current_node.left, current_node.right = current_node.right, current_node.left
            
            if current_node.left:
                queue.append(current_node.left)
            
            if current_node.right:
                queue.append(current_node.right)
        
        return root
```