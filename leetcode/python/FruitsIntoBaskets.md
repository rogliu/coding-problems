# Fruits Into Baskets

### Problem

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the **type** of fruit the `ith` tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

- You only have **two** baskets, and each basket can only hold a **single type** of fruit. There is no limit on the amount of fruit each basket can hold.
- Starting from any tree of your choice, you must pick **exactly one fruit** from **every** tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
- Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array `fruits`, return *the **maximum** number of fruits you can pick*.

### Constraints

- `1 <= fruits.length <= 105`
- `0 <= fruits[i] < fruits.length`

### Example 1: Easy

```
Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.
```

### Example 2: Complex

```
Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].
```

### Edge Cases

```
Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].
```

### Notes

- I am looking for the longest subarray of at most 2 distinct elements

### Brute Force Solution:

- for each element, iterate through the input and add until there 3 unique elements, then update the result

### Complexity Analysis

- Time: O(n^2)
- Space: O(1)

### Optimized Solution: Sliding Window

- Expand the sliding window until we have more than 2 distinct elements
    - When we have more than 2 distinct elements, we have to shrink the window until its valid again (only 2 distinct elements)
- Shrinking means moving the left pointer and subtracting the frequency
    - delete the element from the dictionary if 0

### Complexity Analysis

- Time: O(n)
- Space: O(1)

### Code

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        counts = {}
        most_fruits = 0
        window_start = 0
        for window_end in range(len(fruits)):
            counts[fruits[window_end]] = counts.get(fruits[window_end], 0) + 1
            # don't need to check if window_start <= window_end since its impossible to have window_start == window_end with there being more than 2 distinct characters
            # shrink the sliding window, until we are left with '2' fruits in the fruit frequency dictionary
            while len(counts) > 2:
                counts[fruits[window_start]] -= 1 
                if counts[fruits[window_start]] == 0:
                    del counts[fruits[window_start]]
                window_start += 1 
            most_fruits = max(most_fruits, window_end - window_start + 1)
        
        return most_fruits
  
  return most_fruits
```