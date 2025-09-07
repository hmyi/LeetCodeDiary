# Date: 2025-09-07 SUN
## Problem: [128] Longest Consecutive Sequence
- **Topic:** Array
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/longest-consecutive-sequence/description/
### Problem Summary
- **Requirement:** Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.
- **Constraint:** Algorithm must run in `O(n)` time.
### Key Insight
- HashSet
  - stores unique elements
  - allows `O(1)` lookups
### Solution
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        longest_streak = 0
        num_set = set(nums)

        # iterate through set
        for num in num_set:
            # ensure `num` is start of sequence
            if num - 1 not in num_set:
                current_num = num
                current_streak = 1

                # find the rest of the sequence
                while current_num + 1 in num_set:
                    current_num += 1
                    current_streak += 1

                # update streak counter
                longest_streak = max(longest_streak, current_streak)

        return longest_streak
```
### Complexity Analysis
- Time Complexity: `O(n)`
  - The nested loops might initially appear to be of `O(n^2)` complexity. Upon closer inspection, it is revealed to be `O(n)`. Because the inner loop is only reached when `num` is the start of a sequence.
  - All other computations occur in `O(1)`.
- Space Complexity: `O(n)`
  - HashSet takes `O(n)` space.
  - Integers take `O(1)` space.
## Problem: [1] Two Sum
- **Topic:** Array
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/two-sum/description/
### Problem Summary
- **Requirement:** Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.
- **Assumption:** Each input would have exactly one solution.
- **Constraint:** May not use the same element twice.
### Key Insight
- Hash Table
  - allows `O(1)` lookups
### Solution
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        comp = {}

        # iterate through the array
        for i, num in enumerate(nums):
            # if complement of current `num` found, return indices
            if target - num in comp:
                return [comp[target - num], i]
            # else add current `num` to hash table
            comp[num] = i
        # return empty list if no valid pair found
        return []
```
### Complexity Analysis
- Time Complexity: `O(n)`
  - The array of size `n` is traversed through once.
  - Each lookup in the table costs `O(1)`.
- Space Complexity: `O(n)`
  - The hash table can store at most `n` elements.