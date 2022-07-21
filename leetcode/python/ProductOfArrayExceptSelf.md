# Product of Array Except Self

Category: Array
Date: 07/20/2022
Difficulty: 🟡 Medium
Link: https://leetcode.com/problems/product-of-array-except-self/
Status: 👏 Solved
Tags: Blind 50, Neetcode 150
Tips: Calculate numbers to the left and right of an index

### Problem

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

### Example 1

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

### Example 2

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

### Constraints

- `2 <= nums.length <= 105`
- `30 <= nums[i] <= 30`
- The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

### Solution

- calculate all the values to the left and the right of the current index
- one array stores all the values to the right of the element at that index, and vice versa
- to get the result, we can multiple the left and right values at that index to get the product of the array except that number

### Complexity Analysis

```
Time: O(N) since we are doing one pass through the loop three times.
Space: O(N) since we have to store three arrays of N elements.
```

### Code

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        # brute force would be to multiply all numbers within the array
        # then for each number, divide the total product by that number 
        # to get the product of array except self
        
        # optimized solution would be to use three for loops
        # one will store the products to the left of each element
        # one will store the products to the right of each element
        # the final result will multiply each of these numbers together
        
        length_of_input = len(nums)
        product_to_left = [0 for _ in range(length_of_input)]
        product_to_right = [0 for _ in range(length_of_input)]
        result = [0 for _ in range(length_of_input)]
        
        product_to_right[-1] = 1
        product_to_left[0] = 1
        
        # input = [-1,1,0,-3,3]
        # left = [1, -1, ...]
        for index in range(1, length_of_input):
            product_to_left[index] = product_to_left[index - 1] * nums[index - 1]
        
        for index in reversed(range(length_of_input - 1)):
            product_to_right[index] = product_to_right[index + 1] * nums[index + 1]
            
        for index in range(length_of_input):
            result[index] = product_to_left[index] * product_to_right[index]
        
        return result
```