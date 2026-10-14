---
date_created: 2026-08-18
tags:
  - dsa/problem
  - dsa/pattern/arrays_and_hashing
  - status/solved
platform: NeetCode
difficulty: Medium
pattern: Arrays & Hashing
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-18
next_review:
---

# [[Group Anagrams]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/anagram-groups/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Anagrams contain the same characters, so sorting each string gives every group of anagrams the same key. Use the sorted string as a hash map key and append each original string to its corresponding group.

---

## ⚙️ Approaches

### Approach 1: Sort Each String + Hash Map

- **Idea:** Sort every string to create a canonical key. Strings that are anagrams will produce the same sorted key, so group them together in a hash map.
- **Time Complexity:** O(n × k log k)
- **Space Complexity:** O(n × k)

Where:
- `n` = number of strings
- `k` = maximum length of a string

#### Code Implementation

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        d = defaultdict(list)

        for v in strs:
            sortedKey = ''.join(sorted(v))
            d[sortedKey].append(v)

        return list(d.values())
````

---

## ⚠️ Pitfalls & Edge Cases

-  Empty input → return an empty list
    
-  Empty strings → all empty strings belong to the same group
    
-  Single-character strings
    
-  Duplicate strings
    
-  Strings with different character frequencies must have different keys
    
-  `enumerate()` is unnecessary when the index isn't used
    
-  `defaultdict(list)` avoids manually checking whether a key already exists
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes / Reflections|
|---|---|---|---|
|**1st Pass**|2026-08-18|Y|Independently found the sorting + hash map approach. Used `dict.get()` initially, then improved the implementation to `defaultdict(list)` after a hint.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Arrays & Hashing]]
    
- Similar Problem: [[Valid Anagram]]
    
- Similar Problem: [[Contains Duplicate]]
    
- Similar Problem: [[Two Sum]]