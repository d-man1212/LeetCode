---
date_created: 2026-08-20
tags:
  - dsa/problem
  - dsa/pattern/arrays_and_hashing
  - status/solved
platform: NeetCode
difficulty: Medium
pattern: Arrays & Hashing
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-20
next_review:
---

# [[Longest Consecutive Sequence]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/longest-consecutive-sequence/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Put all numbers into a set for O(1) average-time lookups. Only start building a sequence when `num - 1` is not in the set, because that means `num` is the beginning of a sequence. Then walk forward while consecutive numbers exist.

---

## ⚙️ Approaches

### Approach 1: Sorting

- **Idea:** Sort the numbers, then scan from left to right while tracking the current consecutive sequence length.
- **Time Complexity:** O(n log n)
- **Space Complexity:** O(1) or O(n), depending on the sorting implementation.

> This is a valid approach, but it does not meet the problem's desired O(n) average-time complexity.

---

### Approach 2: Hash Set — Optimal

- **Intuition / Pattern:** Use a set to quickly determine whether neighboring numbers exist. A number should only start a search if its predecessor (`num - 1`) does not exist. This prevents repeatedly traversing the same sequence.

- **Time Complexity:** O(n) average
- **Space Complexity:** O(n)

#### Code Implementation

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numSet = set(nums)
        longest = 0

        for num in numSet:
            if num - 1 not in numSet:
                current = num
                length = 1

                while current + 1 in numSet:
                    length += 1
                    current += 1

                longest = max(longest, length)

        return longest
````

### Why the nested loop is still O(n)

Although there is a `for` loop containing a `while` loop, the algorithm is not O(n²).

The `while` loop only runs when `num` is the beginning of a sequence:

```python
if num - 1 not in numSet:
```

For a sequence such as:

```text
1 → 2 → 3 → 4 → 5
```

only `1` starts the traversal.

When the outer loop later reaches `2`, `3`, `4`, or `5`, each already has a predecessor, so the `while` loop is skipped.

Therefore, across the entire algorithm, the consecutive-sequence traversal processes each number at most once.

---

## ⚠️ Pitfalls & Edge Cases

-  Empty array → return `0`
    
-  Single number → return `1`
    
-  Duplicate numbers → `set()` removes duplicates
    
-  Negative numbers work normally
    
-  Sequence can contain negative and positive values
    
-  Only start searching from numbers where `num - 1` does not exist
    
-  Do not start a forward search from every number, or the approach can become O(n²)
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes / Reflections|
|---|---|---|---|
|**1st Pass**|2026-08-20|N|Initially tried a defaultdict approach. Needed hints to recognize that a set should be used and that only numbers with no predecessor should start a sequence. Implemented the final O(n) solution independently.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Arrays & Hashing]]
    
- Similar Problem: [[Contains Duplicate]]
    
- Similar Problem: [[Two Sum]]
    
- Similar Problem: [[Group Anagrams]]