# Longest Repeating Character Replacement

Approach: Sliding Window
Category: String
Date: 07/11/2022
Difficulty: 🟡 Medium
Link: https://leetcode.com/problems/longest-repeating-character-replacement/
Status: 👏 Solved

### Problem

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return *the length of the longest substring containing the same letter you can get after performing the above operations*.

### Constraints

- `1 <= s.length <= 105`
- `s` consists of only uppercase English letters.
- `0 <= k <= s.length`

### Example 1: Easy

```
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.
```

### Example 2: Complex

```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
```

### Notes

- problem: trying to find the longest substring with only *k* number replacements
- pattern: since the question is asking for the “longest substring”, we could use sliding window

### Brute Force Solution:

- calculate every possible substring (time: O(n^2))
    - need a way to check if a substring is valid
        - length of substring - frequency of most frequent character = number of changes needed
        - if number of changes needed ≤ k, then the substring is valid

### Complexity Analysis

- Time: O(n^2)
- Space: O(1)

### Optimized Solution:

- Expand the window when valid, and shrink (until valid) when invalid
    - need to store the most frequent character in a efficient way
    - if we look for the max each time it would be O(n)
    - instead, we could store a running variable of the most frequent character
        - do we need to know what the most frequent character is?
            - No, as long as we have the size of the window and the most frequent character, we don’t care what the actual characters are

### Complexity Analysis

- Time: O(n)
- Space: O*(1) since there cannot be ore than O(26) characters stored in the dictionary

### Code

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        character_freq = {}
        character_freq["most_freq_character"] = 0
        window_start = 0
        result = 0
        for window_end in range(len(s)):
            character_freq[s[window_end]] = character_freq.get(s[window_end], 0) + 1
            character_freq["most_freq_character"] = max(character_freq["most_freq_character"], character_freq[s[window_end]])
            while window_end - window_start + 1 - character_freq["most_freq_character"] > k:
                character_freq[s[window_start]] -= 1 
                window_start += 1 

            result = max(result, window_end - window_start + 1)

        return result
```

```jsx
def length_of_longest_substring(str1, k):
  window_start, max_length, max_repeat_letter_count = 0, 0, 0
  frequency_map = {}

  # Try to extend the range [window_start, window_end]
  for window_end in range(len(str1)):
    right_char = str1[window_end]
    if right_char not in frequency_map:
      frequency_map[right_char] = 0
    frequency_map[right_char] += 1
    
    # we don't need to place the maxRepeatLetterCount under the below 'if', see the 
    # explanation in the 'Solution' section above.
    max_repeat_letter_count = max(
      max_repeat_letter_count, frequency_map[right_char])

    # Current window size is from window_start to window_end, overall we have a letter which is
    # repeating 'max_repeat_letter_count' times, this means we can have a window which has one letter
    # repeating 'max_repeat_letter_count' times and the remaining letters we should replace.
    # if the remaining letters are more than 'k', it is the time to shrink the window as we
    # are not allowed to replace more than 'k' letters
    if (window_end - window_start + 1 - max_repeat_letter_count) > k:
      left_char = str1[window_start]
      frequency_map[left_char] -= 1
      window_start += 1
      
    max_length = max(max_length, window_end - window_start + 1)
  return max_length

def main():
  print(length_of_longest_substring("aaaabcdbb", 2))
  print(length_of_longest_substring("abbcb", 1))
  print(length_of_longest_substring("abccde", 1))

main()
```