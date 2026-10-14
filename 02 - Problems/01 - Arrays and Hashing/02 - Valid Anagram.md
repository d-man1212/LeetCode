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

# [[Valid Anagram]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/is-anagram/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Anagrams must have the same length and the same frequency of every character. Use a hash map: increment the count for characters in `s` and decrement it for characters in `t`. If every final count is `0`, the strings are anagrams.

---

## ⚙️ Approaches

### Approach 1: Hash Map / Frequency Counter

- **Idea:** First check if both strings have the same length. Iterate through both strings simultaneously, adding `1` for each character in `s` and subtracting `1` for each character in `t`. At the end, every character frequency must be `0`.
- **Time Complexity:** $O(n)$
- **Space Complexity:** $O(k)$, where $k$ is the number of unique characters.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        m, n = len(s), len(t)

        if m != n:
            return False

        mapping = {}

        for i in range(m):

            if s[i] in mapping:
                mapping[s[i]] += 1
            else:
                mapping[s[i]] = 1

            if t[i] in mapping:
                mapping[t[i]] -= 1
            else:
                mapping[t[i]] = -1

        return all(value == 0 for value in mapping.values())
````

### Approach 2: Simplified Hash Map Implementation

> **Further refinement:** The same approach can be written more cleanly using `dict.get()`.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        mapping = {}

        for i in range(len(s)):
            mapping[s[i]] = mapping.get(s[i], 0) + 1
            mapping[t[i]] = mapping.get(t[i], 0) - 1

        return all(value == 0 for value in mapping.values())
```

- **Time Complexity:** $O(n)$
    
- **Space Complexity:** $O(k)$
    

---

## ⚠️ Pitfalls & Edge Cases

-  Different string lengths → immediately return `False`
    
-  Empty strings → both are valid anagrams
    
-  Same characters with different frequencies → return `False`
    
-  Character case matters unless specified otherwise
    
-  Special characters and spaces count as characters unless specified otherwise
    

---

## 🔁 Spaced Repetition Tracker

| Review       | Date       | Solved Without Hints? (Y/N) | Notes / Reflections                                                                                                                                                                          |
| ------------ | ---------- | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1st Pass** | 2026-08-18 | Y                           | Independently came up with the single hash map approach: increment characters from `s` and decrement characters from `t`. Needed a small hint for simplifying the final check using `all()`. |
| **2nd Pass** |            |                             |                                                                                                                                                                                              |
| **3rd Pass** |            |                             |                                                                                                                                                                                              |

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Arrays & Hashing]]
    
- Similar Problem: [[Contains Duplicate]]
    
- Similar Problem: [[Group Anagrams]]
    
- Similar Problem: [[Ransom Note]]
    

One important distinction I captured: **you independently found the actual algorithm**. The hint was only for the Pythonic `all()` cleanup, so this absolutely counts as solving the problem yourself. 
