# Two Sum

Approach: Dictionary
Category: Array
Date: 06/22/2022
Difficulty: 🟢 Easy
Link: https://leetcode.com/problems/two-sum/
Status: 👏 Solved
Tags: Blind 50
Tips: Hashmap to store values to check later

### Problem

<aside>
💡 Problem: Given an array of integers, “nums” and an integer “target”, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

</aside>

### Example 1

```
Input: nums = [2,7,11,15], target = 9
Output:  [0,1] 
nums[0] + nums[1] == 9, we return [0,1]
```

### Example 2

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

### Example 3

```
Input: nums = [3,3], target = 6
Output: [0,1]
```

### Constraints

- `2 <= nums.length <= 104`
- `109 <= nums[i] <= 109`
- `109 <= target <= 109`
- **Only one valid answer exists.**

### Solution: Brute Force Approach

- For each element
    - iterate through input and check if the pairs equals the target

### Code

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        
        for index_one in range(len(nums)):
            for index_two in range(index_one + 1, len(nums)):
                element_one = nums[index_one]
                element_two = nums[index_two]
                if element_one + element_two == target:
                    return index_one, index_two
```

### Complexity Anaylsis:

- TIme: O(n^2) since for each element we are iterating through the input
- Space: O(1)

### Solution: Finding the Complement

1. For each element
    1. return index and the complements index if its complement is in the dictionary 
        1. complement: target - current element
    2. if not, store the current element and its current index into the array

### Complexity Analysis

- Time: O(n) since only one pass through the array
- Space: O(n) since we could store all the elements at the worst case

### Code: Complements using Dictionary

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:

        complements = {}
        
        for index, number in enumerate(nums):
            complement = target - number
            if complement in complements:
                return index, complements[complement]
            
            complements[number] = index
```