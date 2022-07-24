# Total Appeal of A String

### Problem

The **appeal** of a string is the number of **distinct** characters found in the string.

- For example, the appeal of `"abbca"` is `3` because it has `3` distinct characters: `'a'`, `'b'`, and `'c'`.

Given a string `s`, return *the **total appeal of all of its substrings.***

A **substring** is a contiguous sequence of characters within a string.

### Constraints

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.

### Example 1: Easy

```
Input: s = "abbca"
Output: 28
Explanation: The following are the substrings of "abbca":
- Substrings of length 1: "a", "b", "b", "c", "a" have an appeal of 1, 1, 1, 1, and 1 respectively. The sum is 5.
- Substrings of length 2: "ab", "bb", "bc", "ca" have an appeal of 2, 1, 2, and 2 respectively. The sum is 7.
- Substrings of length 3: "abb", "bbc", "bca" have an appeal of 2, 2, and 3 respectively. The sum is 7.
- Substrings of length 4: "abbc", "bbca" have an appeal of 3 and 3 respectively. The sum is 6.
- Substrings of length 5: "abbca" has an appeal of 3. The sum is 3.
The total sum is 5 + 7 + 7 + 6 + 3 = 28.
```

### Example 2: Complex

```
Input: s = "code"
Output: 20
Explanation: The following are the substrings of "code":
- Substrings of length 1: "c", "o", "d", "e" have an appeal of 1, 1, 1, and 1 respectively. The sum is 4.
- Substrings of length 2: "co", "od", "de" have an appeal of 2, 2, and 2 respectively. The sum is 6.
- Substrings of length 3: "cod", "ode" have an appeal of 3 and 3 respectively. The sum is 6.
- Substrings of length 4: "code" has an appeal of 4. The sum is 4.
The total sum is 4 + 6 + 6 + 4 = 20.
```

### Notes

- Consider the set of substrings that end at a certain index i (1). Then, consider a specific alphabetic character. How do you count the number of substrings ending at index i that contain that character (2)?
    1. 
        - i.e. s = “code”
            - set of substrings that end at index 1 → {’c’}
            - set of substrings that end at index 2 → {’o’, ’co’}
            - set of substrings that end at index 3 → {’d’, ‘cod’, ‘od’}
            - set of substrings that end at index 4 → {’e’, ‘code’, ‘ode’, ‘de’}
    2. 
        - The number of substrings that contain the alphabetic character is equivalent to 1 plus the index of the last occurrence of the character before index i + 1.
            - i.e. {’c’},
            - i.e. {’c’}, {’o’, ‘co} for ‘o’, index = 1
            - index = 1 + 1 = 2
                - last
                - index i + 1 =
    - The total appeal of all substrings ending at index i is the total sum of the number of substrings that contain each alphabetic character.
    - To find the total appeal of all substrings, we simply sum up the total appeal for each index.

### Brute Force Solution:

1. Generate all possible substrings 
    1. For each substring, iterate through its characters
        1. if a character hasn’t been seen, add it to a set
        2. if seen, continue
        3. add on the length of the set to our result 

### Complexity Analysis

- Time: O(n^2)
    - Generating each substring is O(n^2)
- Space: O(n)

### Code

```python
class Solution:
    def appealSum(self, s: str) -> int:
        result = 0
        for i in range(len(s)):
            seen = set()
            for j in range(i, len(s)):
                if s[j] not in seen:
                    seen.add(s[j])
                result += len(seen)
                
        return result
```

### Optimized Solution:

s = ‘ababba’

| Index I | Possible substrings at this point | Appeal points for each level |
| --- | --- | --- |
| 0 a  | ‘a’-1 = 1 | 1 |
| 1 b  | ‘ab’-2, ‘b’-1 = 3 | 1+2 = 3 |
| 2 a  | ‘aba’-2, ‘ba’-2, ‘a’-1 = 5 | 3+2 = 5  |
| 3 b  | ‘abab’-2, ‘bab’-2, ‘ab’-2, ‘b’-1 = 7 | 5+3-1=7 |
| 4 b  | ‘ababb’-2, ‘babb’-2, ‘abb’-2, ’bb’-1, ’b’-1 = 8 | 7+4-3=8 |
| 5 a | ‘ababba’-2, ‘babba’-2, ‘abba’-2, ‘bba’-2’ ‘ba’-2, ‘a’-1 =11 | 8+5-2 = 11 |
| Total |  | 35 |
- What is the pattern?
    - if the current character has been seen before
        - appeal points for the level = previous appeal + current index - last seen index
    - if not seen
        - appeal points for the level = previous appeal + current index + 1
- To store the previously seen character’s index, we can use a dictionary
- previous points + (number of possible substrings with current index - index last character was shown)

### Complexity Analysis

- Time: O(26n) → O(n)
- Space: O(26)

### Code

```python
class Solution:
    def appealSum(self, s: str) -> int:
        # stores last seen index of a character
        seen = {}
        # used to calculate current appeal
        previous_appeal = 0
        current_appeal = 0 
        result = 0 
        for index, character in enumerate(s):
            if character in seen:
                current_appeal = previous_appeal + index - seen[character]
            else:
                current_appeal = previous_appeal + index + 1
            # update the last seen index
            seen[character] = index
            # save the current appeal
            previous_appeal = current_appeal
            # add on the total appeal at index
            result += current_appeal
        
        return result
```