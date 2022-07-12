# Max Consecutive Ones III

### Problem

Given a binary array `nums` and an integer `k`, return *the maximum number of consecutive* `1`*'s in the array if you can flip at most* `k` `0`'s.

### Constraints

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.
- `0 <= k <= nums.length`

### Example 1: Easy

```
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

### Example 2: Complex

```
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

### Notes

- problem: longest subarray with 1s and number of 0s ≤ k (number of replacements)
- pattern: since the question is asking for the *longest contiguous subarray*, we could use sliding window

### Brute Force Solution:

- calculate each possible subarray and return the longest subarray that is valid
    - How do we know if a subarray is valid?
        - if the length of the subarray - number of 1’s ≤ k
    - How do we know if a subarray is invalid?
        - the number of 0’s exceeds k

### Complexity Analysis

- Time: O(n^2)
- Space: O(1)

### Optimized Solution: Sliding Window

- When do we expand?
    - since we are trying to find the longest, we expand when its valid
- When do we shrink?
    - when the subarray is invalid → when the number of 0’s exceeds k
- How do we shrink?
    - if the element at the start of the window is a 0, decrement the count of 0’s
    - increment the left pointer
    - shrink until number of zeros ≤ k

### Complexity Analysis

- Time: O(n)
- Space: O(1)

### Code

```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        num_zeros = 0
        longest = 0
        window_start = 0
        for window_end in range(len(nums)):
            if nums[window_end] == 0: num_zeros += 1 

            while num_zeros > k:
                if nums[window_start] == 0: num_zeros -= 1 
                window_start += 1 

            longest = max(longest, window_end - window_start + 1)
        
        return longest
```