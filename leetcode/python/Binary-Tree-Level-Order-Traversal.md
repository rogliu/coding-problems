# Binary Tree Level Order Traversal

### Problem

Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

### Constraints

- The number of nodes in the tree is in the range `[0, 2000]`.
- `1000 <= Node.val <= 1000`

### Example 1

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

### Example 2

```
Input: root = [1]
Output: [[1]]
```

### Example 3

```
Input: root = []
Output: []
```

### Notes

- problem: return the level order traversal of a binary tree list of lists, where each inner list is the nodes of a level
- return a list of

### Solution: Breadth-first Search

- do a breadth-first search since we want to go level by level
- get the number of nodes in current level (length of the queue)
- pop nodes for that length
- instantiate a list for each level
    - for each node
        - add it onto a list
        - add its children onto the queue

### Complexity Analysis

- Time: O(n) since it traverses all the nodes
- Space: O(n) since it stores all the nodes

### Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if root is None:
            return []
        queue = collections.deque([root])
        result = []
        while queue:
            # list to store nodes in level
            level = []
            nodes_in_level = len(queue)
            # only pops nodes for the current level
            for number_added in range(nodes_in_level):
                node = queue.popleft()
                # adds node to level
                level.append(node.val)
                # appends children onto queue if valid
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            # adds level onto result
            result.append(level)
        
        return result
```

### Solution: Recursive

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # list of lists
        # each index in result represents level
        result = []
        # edge case
        if root is None:
            return result
        # adds node by node and keeps track of level
        def helper(node, level):
            # adds onto result when there's a new level
            if level == len(result):
                result.append([])
            result[level].append(node.val)
            # appends nodes if present
            if node.left:
                helper(node.left, level + 1)
            if node.right:
                helper(node.right, level + 1)
        # start from level 0
        helper(root, 0)
        return result
```