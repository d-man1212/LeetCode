---
date_created: 2026-08-18
tags:
  - dsa/problem
  - dsa/pattern/arrays_and_hashing
  - status/solved
platform: NeetCode
difficulty: Easy
pattern: Arrays & Hashing
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-18
next_review:
---

# [Contains Duplicate](<Contains Duplicate>)

- **Problem Link:** [LeetCode Link](https://neetcode.io/problems/duplicate-integer/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> A brute-force approach compares every number with every other number, resulting in O(n²) time. A hash set stores only unique values, so if the size of the set is smaller than the original array, a duplicate exists.

---

## ⚙️ Approaches

### Approach 1: Brute Force (Initial Thought)

- **Idea:** Compare every element with every other element. If two elements at different indices have the same value, return `True`.
- **Time Complexity:** O(n²)
- **Space Complexity:** O(1)

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] == nums[j]:
                    return True

        return False
````

> **Note:** My original brute-force solution compared each pair twice (`i → j` and `j → i`), which caused unnecessary work and resulted in Time Limit Exceeded.

---

### Approach 2: Optimal Solution

- **Intuition / Pattern:** A set automatically removes duplicate values. If the length of the set is smaller than the length of the original array, at least one duplicate exists.
    
- **Time Complexity:** O(n)
    
- **Space Complexity:** O(n)
    

#### Code Implementation

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        return len(set(nums)) < len(nums)
```

### Alternative Interview-Friendly Implementation

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        seen = set()

        for num in nums:
            if num in seen:
                return True

            seen.add(num)

        return False
```

---

## ⚠️ Pitfalls & Edge Cases

-  Empty input → No duplicates
    
-  Single element → No duplicates
    
-  Negative numbers → Set handles them normally
    
-  Zero → Set handles it normally
    
-  Duplicate values → Return `True`
    
-  Multiple duplicates → Return `True` as soon as one is found
    
-  Avoid comparing the same pair twice in the brute-force approach
    

---

## 🔁 Spaced Repetition Tracker

| Review       | Date       | Solved Without Hints? (Y/N) | Notes / Reflections                                                            |
| ------------ | ---------- | --------------------------- | ------------------------------------------------------------------------------ |
| **1st Pass** | 2026-08-18 | Y                           | Started with brute force, then independently found the O(n) hash set solution. |
| **2nd Pass** |            |                             |                                                                                |
| **3rd Pass** |            |                             |                                                                                |

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Arrays & Hashing]]
    
- Similar Problems:
    
    - [[Valid Anagram]]
        
    - [[Two Sum]]
        
    - [[Group Anagrams]]
        

One small recommendation: for **your personal notes**, keep the sentence about your original mistake:

> **My original brute force compared every pair twice, causing unnecessary work.**

That kind of reflection is more valuable later than generic explanations, because when you review this problem in a month, you'll remember **what you specifically got wrong**.