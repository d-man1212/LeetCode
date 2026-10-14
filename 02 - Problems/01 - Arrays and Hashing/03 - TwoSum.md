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

# [[Two Sum]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/two-integer-sum/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> For each number, calculate the complement needed to reach the target. Store previously seen numbers and their indices in a hash map. If the complement already exists, we have found the two indices.

---

## ⚙️ Approaches

### Approach 1: Brute Force

- **Idea:** Check every possible pair of numbers. If their sum equals the target, return their indices.
- **Time Complexity:** $O(n^2)$
- **Space Complexity:** $O(1)$

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)

        for i in range(n):
            for j in range(i + 1, n):
                if nums[i] + nums[j] == target:
                    return [i, j]

        return []
````

---

### Approach 2: Hash Map

- **Intuition / Pattern:** Instead of searching through the array for the second number, store numbers that have already been seen in a dictionary. For each current number, calculate the complement needed to reach `target` and check whether it already exists in the dictionary.
    
- **Time Complexity:** $O(n)$
    
- **Space Complexity:** $O(n)$
    

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)
        d = {}

        for i in range(n):
            if target - nums[i] in d:
                return [d[target - nums[i]], i]
            else:
                d[nums[i]] = i

        return []
```

### Readability Improvement

The same approach can be made slightly easier to read by storing the required complement:

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}

        for i, num in enumerate(nums):
            need = target - num

            if need in seen:
                return [seen[need], i]

            seen[num] = i

        return []
```

- **Time Complexity:** $O(n)$
    
- **Space Complexity:** $O(n)$
    

---

## ⚠️ Pitfalls & Edge Cases

-  Do not use the same element twice
    
-  Duplicate values can form the answer, e.g. `[3, 3]`, target `6`
    
-  Check for the complement **before** adding the current number
    
-  Negative numbers work with the same approach
    
-  Return indices, not the values themselves
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes / Reflections|
|---|---|---|---|
|**1st Pass**|2026-08-18|Y|Independently implemented both brute force and the optimal hash map approach. The only suggestion received was a readability improvement using a `need` variable.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Arrays & Hashing]]
    
- Similar Problem: [[Contains Duplicate]]
    
- Similar Problem: [[Valid Anagram]]
    
- Similar Problem: [[Group Anagrams]]
    

This one is a clean **independent solve**: you found both the brute-force and optimal hash map solution yourself. The `need` variable is only a readability refinement, not an algorithmic hint. 