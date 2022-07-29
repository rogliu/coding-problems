# Minimum Size Subarray Sum

### Problem

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a **contiguous subarray** `[numsl, numsl+1, ..., numsr-1, numsr]` of which the sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

### Constraints

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

### Example 1: Easy

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

### Example 2: Complex

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

### Edge Cases

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

### Brute Force Solution:

- calculate every possible subarray and its sum for the input array
- maintain the minimum length for each valid subarray and return it

### Complexity Analysis

- Time: O(N^2) since we have to iterate through the input for each element
- Space: O(1)

### Code

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        minimum_size = float('inf')
        for i in range(len(nums)):
            running_sum = 0
            for j in range(i, len(nums)):
                running_sum += nums[j]
                if running_sum >= target:
                    minimum_size = min(minimum_size, j - i + 1)
        return minimum_size if minimum_size != float('inf') else 0
```

### Optimized Solution: Sliding Window

- Problem follows the Sliding Window problem
    - Can use a similar strategy to Maximum Sum Subarray of Size K, except the sliding window size is not fixed
1. Add up elements from beginning of array until their sum becomes greater than or equal to the traget
2. These elements make up our sliding window
    1. Asked to find the smallest such window having a greater than or equal to the target
    2. Remember the length of this window as the smallest window so far
3. After this, keep adding one element in the sliding window in a stepwise fashion
4. In each step, try to shrink the window from the beginning
5. Shrink the window until the window’s sum is smaller than ‘S’ again, this is needed as we intend to find the smallest window
6. Shrinking happens in multiple steps, in each step
    1. check if the current window length is the smallest so far, and if so remember the length
    2. subtract the first element of the window from the running sum to shrink the sliding window

### Complexity Analysis

- Time: O(N)
    - the outer for loop runs for all elements
    - the inner while loop processes each element only once
    - O(N + N) → O(N)
- Space: O(1)

### Code

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        # Time Complexity O(n) 
        # Space Complexity O(1) 
        smallest_length = float('inf')
        window_sum = 0
        window_start = 0 
        # expand window
        for window_end in range(len(nums)):
            window_sum += nums[window_end] # add next element
            # shrink the window as small as possible until window_sum < s
            while window_start <= window_end and window_sum >= target:
                smallest_length = min(smallest_length, window_end - window_start + 1)
                window_sum -= nums[window_start]
                window_start += 1 

        # returns 0 if no valid length found
        return smallest_length if smallest_length != float('inf') else 0
```