# Same Tree

### Problem

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

### Example 1

```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

### Example 2

```
Input: p = [1,2], q = [1,null,2]
Output: false
```

### Example 3

```
Input: p = [1,2,1], q = [1,1,2]
Output: false
```

### Constraints

- The number of nodes in both trees is in the range `[0, 100]`.
- `104 <= Node.val <= 104`

### Solution

```
If they are both None, then return True
If one is None and the other isn’t then return False
If the values are different, return False
Call the function recursively on the left and right sides of the tree.
```

### Complexity Analysis

```
Time: O(N) time since it traverses all the nodes
Space: O(log(N)) if the tree is balanced or O(N) if the tree is unbalnaced 
i.e. unbalanced when there is no right nodes and the tree keeps branching outwards.
```

### Recursive

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True  
        if not p or not q:
            return False
        if p.val != q.val:
            return False
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

### Breadth First Search

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        queue = collections.deque([(p, q)])
        while queue:
            (p, q) = queue.popleft()
						# if both nodes exist and they have the same value, continue iterating
            if p and q and p.val == q.val: 
                queue.extend([(p.left, q.left), (p.right, q.right)])
					  # if only one node exists, return False
            elif p or q:
                return False
						# if both nodes don't exist, continue iterating
        
        return True
```