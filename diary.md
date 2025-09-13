# Date: 2025-09-07 SUN
## Problem: [128] Longest Consecutive Sequence
- **Topics:** Array, Hash Table
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/longest-consecutive-sequence/description/
### Problem Summary
- **Requirement:** Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.
- **Constraint:** Algorithm must run in $O(n)$ time.
### Key Insight
- Hash Set
  - stores unique elements
  - allows $O(1)$ lookups
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
- Time Complexity: $O(n)$
  - The nested loops might initially appear to be of $O(n^2)$ complexity. Upon closer inspection, it is revealed to be $O(n)$. Because the inner loop is only reached when `num` is the start of a sequence.
  - All other computations occur in $O(1)$.
- Space Complexity: $O(n)$
  - HashSet takes $O(n)$ space.
  - Integers take $O(1)$ space.
## Problem: [1] Two Sum
- **Topics:** Array, Hash Table
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/two-sum/description/
### Problem Summary
- **Requirement:** Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.
- **Assumption:** Each input would have exactly one solution.
- **Constraint:** May not use the same element twice.
### Key Insight
- Hash Table
  - allows $O(1)$ lookups
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
- Time Complexity: $O(n)$
  - The array of size $n$ is traversed through once.
  - Each lookup in the table costs $O(1)$.
- Space Complexity: $O(n)$
  - The hash table can store at most $n$ elements.
# Date: 2025-09-08 MON
## Problem: [3] Longest Substring Without Repeating Characters
- **Topics:** String, Hash Table, Sliding Window
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/longest-substring-without-repeating-characters/description/
### Problem Summary
- **Requirement:** Given a string `s`, find the length of the longest substring without duplicate characters.
### Key Insight
- Hash Table
  - maps character to index of last occurrence
- Sliding Window
  - avoids listing all possible substrings
### Solution
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        longest = 0
        table = {}
        left = 0

        # the right index iterates through the string
        for right in range(len(s)):
            # if repeat char found, move left index one past last occurrence
            if s[right] in table:
                # "max()" prevents moving backwards
                left = max(table[s[right]] + 1, left)

            # update longest counter, update table with new char
            longest = max(longest, right - left + 1)
            table[s[right]] = right

        return longest
```
### Complexity Analysis
- Time Complexity: $O(n)$
  - Index $right$ will iterate $n$ times.
- Space Complexity: $O(min(m, n))$
  - We need $O(k)$ space for the sliding window, where $k$ is the size of the hash table. The size of the table is upper bounded by the size of the string $n$ and the size of the charset/alphabet $m$.
## Problem: [5] Longest Palindromic Substring
- **Topics:** String, Dynamic Programming, Two Pointers
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/longest-palindromic-substring/description/
### Problem Summary
- **Requirement:** Given a string `s`, return the longest palindromic substring in `s`.
### Key Insight
- Dynamic Programming
  - solves complex problems by breaking them down into simpler subproblems, solving each subproblem once, and storing their results to avoid redundant work
  - "Remember and Reuse" instead of "Repeat and Regret" (brute force)
  - especially useful for 2 scenarios:
    - overlapping subproblems
    - optimal substructure
  - 2 common approaches:
    - Top-down (Memoization) -> recursive
    - Bottom-up (Tabulation) -> iterative
- "Expand From Centers"
  - "expand" at each single character or pair of identical connected characters, expand as long as the palindrome condition is met
- Manacher's Algorithm **[BEYOND THE SCOPE]**
  - finds the longest palindromic substring in $O(n)$ time and space.
### Solution
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        left = 0
        right = 0

        # expand to find longest palindromic substring with l, r as center
        def expand(l, r):
            # bounds & palindrome check
            while l >= 0 and r < len(s) and s[l] == s[r]:
                l -= 1
                r += 1

            # return the last eligible indices
            return [l + 1, r - 1]

        # iterate through all "centers"
        for i in range(len(s)):
            # odd centers
            l, r = expand(i, i)
            # update left, right if new longest palindrome found
            if r - l > right - left:
                left = l
                right = r

            # even centers
            l, r = expand(i, i + 1)
            # update left, right if new longest palindrome found
            if r - l > right - left:
                left = l
                right = r

        # return the substring ("+1" for proper indexing)
        return s[left : right + 1]
```
### Complexity Analysis
- Time Complexity: $O(n^2)$
  - There are $2n − 1 = O(n)$ centers. For each center, we call expand, which costs up to $O(n)$.
- Space Complexity: $O(1)$
  - No extra space used except for a few integers.
# Date: 2025-09-13 SAT
## Problem: [133] Clone Graph
- **Topics:** Graph, DFS, BFS, Hash Table
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/clone-graph/description/
### Problem Summary
- **Requirement:** Given a reference of a node in a connected undirected graph. Return a deep copy (clone) of the graph.
- **Clarification:** Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.

  ```Java
  class Node {
      public int val;
      public List<Node> neighbors;
  }
  ```
### Key Insight
- Depth First Search
- Breadth First Search
- Hash Table
  - quick reference from original node to cloned node
### Solution
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

from typing import Optional
class Solution:
    def __init__(self):
        # keep track of visited nodes & help avoid cycles
        # key: original node, val: cloned node
        self.visited = {}

    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        # if input graph is empty
        if not node:
            return node

        # if node already visited, return clone
        if node in self.visited:
            return self.visited[node]

        # create clone_node with identical val & empty neighbors
        clone_node = Node(node.val, [])

        # add clone_node to visited
        # NOTE: objects are passed by reference in Python
        self.visited[node] = clone_node

        # if node has neighbors
        if node.neighbors:
            # recursively clone node.neighbors & update clone_node.neighbors
            clone_node.neighbors = [self.cloneGraph(n) for n in node.neighbors]
            # pass by reference -> update self.visited[node].neighbors

        return clone_node
```
### Complexity Analysis
- Time Complexity: $O(n + m)$
  - $n$ is the number of nodes and $m$ is the number of edges.
- Space Complexity: $O(n)$
  - The space is occupied by the `visited` hash map and the recursion stack. `visited` takes up $O(n)$ space. The space occupied by the recursion stack is $O(h)$ where $h$ is the height of the graph. $h$ is bounded by $n$ in the worst case. Hence, the recursion stack also takes up $O(n)$ space.