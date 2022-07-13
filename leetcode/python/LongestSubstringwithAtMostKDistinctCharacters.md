# Longest Substring with At Most K Distinct Characters

Tips: Think about when to expand and shrink the sliding window

### Problem

Given a string `s` and an integer `k`, return *the length of the longest substring of* `s`*that contains at most* `k` ***distinct** characters*.

### Constraints

- `1 <= s.length <= 5 * 104`
- `0 <= k <= 50`

### Example 1: Easy

```
Input: s = "eceba", k = 2
Output: 3
Explanation: The substring is "ece" with length 3.
```

### Example 2: Complex

```
Input: s = "aa", k = 1
Output: 2
Explanation: The substring is "aa" with length 2.
```

### Brute Force Solution:

- calculate the unique characters of all valid substrings and return the longest one

### Complexity Analysis

- Time: O(n^2)
- Space: O(n)

### Optimized Solution: Sliding Window

- We can use a dictionary to remember the frequency of each character we have processed
1. Insert characters into the dictionary until we have K distinct characters
2. These constitute our sliding window and we’re asked to find the longest window such that there are no more than K distinct characters
3. After this, keep adding one character in the sliding window or “slide the window ahead”
4. In each step, try to shrink the window from the beginning if the count of distinct characters is larger than K
    1. Shrink the window until we have no more than K distinct characters in the dictionary
5. While shrinking, decrement the character’s frequency going out of the window and remove it from the dictionary if it becomes zero
6. At the end of each step, check if the current window length is the longest so far
- Visual
    
    ![Screen Shot 2022-07-06 at 5.17.59 PM.png](Longest%20Substring%20with%20At%20Most%20K%20Distinct%20Characte%20f5687edb5a0d4a17b42a5fceaa8addbc/Screen_Shot_2022-07-06_at_5.17.59_PM.png)
    

### Complexity Analysis

- Time: O(n) where n is the number of characters in the string
    - outer for loop runs through all the characters
    - inner for loop processes each character only once
    - O(n+n) → O(n)
- Space: O(k) since we are storing a maximum of O(K+1) characters in the dictionary

### Code

```python
class Solution:
    def lengthOfLongestSubstringKDistinct(self, s: str, k: int) -> int:
        window_start = 0
        result = 0
        character_frequencies = {}
        for window_end in range(len(s)):
            character = s[window_end]
            character_frequencies[character] = character_frequencies.get(character, 0) + 1
            # this code is incorrect because its at maximum k, not equal to k 
            # if len(character_frequencies) == k:
            #     result = max(result, window_end-window_start+1)
            while window_start <= window_end and len(character_frequencies) > k:
                window_start_character = s[window_start]
                character_frequencies[window_start_character] -= 1
                if character_frequencies[window_start_character] == 0:
                    del character_frequencies[window_start_character]
                window_start += 1
            
            # should be here, since at this point we can assume the window is valid 
            result = max(result, window_end-window_start+1)
        return result
```

## Mistakes

- Solution was invalid, tried to shrink while valid which gives us the minimum → must expand while valid and shrink while invalid (unique characters > k)
- Expand the window, once its valid → not sure whether to expand or shrink
    - must expand since we are trying to find the longest
    - if the window is greater than K, shrink
    - You were trying to shrink while valid, which makes no sense since you have to expand while valid to find the maximum
    - You can’t shrink when the window is smaller, only when its greater