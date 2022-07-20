# Delete and Earn

### Problem

You are given an integer array `nums`. You want to maximize the number of points you get by performing the following operation any number of times:

- Pick any `nums[i]` and delete it to earn `nums[i]` points. Afterwards, you must delete **every** element equal to `nums[i] - 1` and **every** element equal to `nums[i] + 1`.

Return *the **maximum number of points** you can earn by applying the above operation some number of times*.

### Constraints

- `1 <= nums.length <= 2 * 104`
- `1 <= nums[i] <= 104`

### Example 1: Easy

```
Input: nums = [3,4,2]
Output: 6
Explanation: You can perform the following operations:
- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = [2].
- Delete 2 to earn 2 points. nums = [].
You earn a total of 6 points.
```

### Example 2: Complex

```
Input: nums = [2,2,3,3,3,4]
Output: 9
Explanation: You can perform the following operations:
- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = [3,3].
- Delete a 3 again to earn 3 points. nums = [3].
- Delete a 3 once more to earn 3 points. nums = [].
You earn a total of 9 points.
```

### Edge Cases

```
- lots of input 
- [1]
```

### Notes

- can simplify the problem
    - Intuition: if you want to take an element *i*, you’re going to want to take all of them
        - vice versa delete all of the i+1 and i-1 elements
    - you can get the frequencies of all the elements, and from there calculate its value
        - i.e. key*value or number*frequency
- state: maximum amount to earn at index i
- recurrence relation: either take the value from the element before or the current element + previous two element

### Brute Force Solution: Top-down Recursion

- base cases:
    - if i == 0: return 0
    - if i == 1: return frequency[1]
- if its a valid frequency, calculate max between current element + current -2th element and the previous element
- else return the top_down(previous element)

### Complexity Analysis

- Time: O(n+k) since for each call, we could call 2 more function calls
- Space: O(k) since there will never be more than n calls on the stack

### Code

```python
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        frequency = collections.Counter(nums)
        
				def top_down(i):
            if i == 0:
                return 0
            if i == 1:
                return frequency[1]
            
            if i in frequency:
                value = frequency[i]
                return max(top_down(i-1), i*frequency[i] + top_down(i-2))
            else:
                return top_down(i-1)

        return top_down(max(nums))
```

### Optimized Solution: Top-down Recursion with Memoization

- check if the value has been calculated
    - if not, calculate the value and store in memo
    - else return the value

### Complexity Analysis

- Time: O(n+k) where n is the length of nums and k is the max value
- Space: O(n+k) since we have to store all the input elements and also store the results from calculations 0-k

### Code

```python
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        frequency = collections.Counter(nums)
        memo = {}
            
        def top_down_memo(i):
            if i == 0:
                return 0
            if i == 1:
                return frequency[1]
            
            if i not in memo:
                if i in frequency:
                    value = frequency[i]
                    memo[i] = max(top_down_memo(i-1), i*frequency[i] + top_down_memo(i-2))
                else:
                    memo[i] = top_down_memo(i-1)
            
            return memo[i]

        return top_down_memo(max(nums))
```

### Optimized Solution: Bottom-up

- iterate from 0→max element
    - if the index is in the input
        - the maximum value at that index is either the previous index’s value or the current value * its frequency + the previous second element
    - else, the current indices max is the previous elements value

### Complexity Analysis

- Time: O(n+k) where n is the length of nums and k is the max value
- Space: O(n+k) since we have to store all the input elements and also store the results from calculations 0-k

### Code

```python
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        frequency = collections.Counter(nums)
        
        def bottom_up():
            dp = [0] * (max(nums) + 1)

            for key in range(len(dp)):
                if key in frequency:
                    value = frequency[key]
                    dp[key] = max(dp[key-1], key*value + dp[key-2])
                else:
                    dp[key] = dp[key-1]
            
            return dp[-1]

        return bottom_up()
```