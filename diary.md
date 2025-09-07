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
  - Hence, overall runtime is `O(n)`.
- Space Complexity: `O(n)`
  - HashSet takes `O(n)` space.
  - Integers take `O(1)` space.
  - Hence, overall space is `O(n)`.