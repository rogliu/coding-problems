# Longest Substring Without Repeating Characters

Tips: for sliding window, think about when to shrink the window until, not just when to shrink the window

### Problem

Given a string `s`, find the length of the **longest substring** without repeating characters.

### Constraints

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

### Example 1: Easy

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

### Example 2: Complex

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

### Edge Cases

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

### Notes

- longest substring with distinct characters
- Clarifying question:
    - if the string has ‘a’ and ‘A’, does ‘A’ count as a repeat of ‘a’ or are they unique?
- Patterns: seems like it could be a sliding window problem since we are trying to find the longest contiguous substring in the input
    - sliding window:
        - when do we expand → when valid
        - when do we shrink → when invalid
            - have to shrink until its valid again

### Brute Force Solution:

- for each character, iterate through the input until it isn’t a distinct character
- compare its length with the current longest substring

### Complexity Analysis

- Time: O(n^2)
- Space: O(1)

### Optimized Solution:

- How do we determine when a substring is valid?
    - for each character, add it into a set
        - if its not in the set, add the new element and check if its the longest substring
        - if its in the set, check if the current length is the longest substring and then remove elements until the current element isn’t in the set

### Complexity Analysis

- Time: O(n)
- Space: O(k) is the number of distinct characters in the input string
    - aka k ≤ n, since in the worst case, the string might not have any duplicate characters
    - since we can expect a fixed set of characters in the input, i.e. 26 for English letters, we can say the algorithm runs in fixed space O(1)
    - could use a fixed-size array instead of a HashMap

### Code: Sliding Window

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        characters = set()
        window_start = 0
        longest_substring = 0
        
        for window_end_character in s:
            if window_end_character not in characters:
                characters.add(window_end_character)
                longest_substring = max(longest_substring, len(characters))
            else:
                while window_end_character in characters:
                    characters.remove(s[window_start])
                    window_start += 1
                characters.add(window_end_character)
        
        return longest_substring
```

### Code: Sliding Window Optimized

```jsx
def non_repeat_substring(str1):
  window_start = 0
  max_length = 0
  char_index_map = {}

  # try to extend the range [windowStart, windowEnd]
  for window_end in range(len(str1)):
    right_char = str1[window_end]
    # if the map already contains the 'right_char', shrink the window from the beginning so that
    # we have only one occurrence of 'right_char'
    if right_char in char_index_map:
      # this is tricky; in the current window, we will not have any 'right_char' after its previous index
      # and if 'window_start' is already ahead of the last index of 'right_char', we'll keep 'window_start'
      window_start = max(window_start, char_index_map[right_char] + 1)
    # insert the 'right_char' into the map
    char_index_map[right_char] = window_end
    # remember the maximum length so far
    max_length = max(max_length, window_end - window_start + 1)
  return max_length

def main():
  print("Length of the longest substring: " + str(non_repeat_substring("aabccbb")))
  print("Length of the longest substring: " + str(non_repeat_substring("abbbb")))
  print("Length of the longest substring: " + str(non_repeat_substring("abccde")))

main()
```

- use a HashMap to remember the last index of each character we have processed
- whenever we get a duplicate character, we shrink our sliding window to ensure we always have distinct characters in the sliding window

## Mistakes

- For the sliding window approach
    - I only considered *when* to shrink the window, not when to shrink the window *until*
        - aka you can’t just blindly shrink the window by one, you have to shrink it, in this case, until the element on the end of the window isn’t in the set
        - then you have to add the element to the set at the end