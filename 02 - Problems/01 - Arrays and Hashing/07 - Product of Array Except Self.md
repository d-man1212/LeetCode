---
date_created: 2026-08-19
tags:
  - dsa/problem
  - dsa/pattern/arrays_and_hashing
  - dsa/pattern/prefix_suffix
  - status/solved
platform: NeetCode
difficulty: Medium
pattern: Arrays & Hashing
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-19
next_review:
---

# [[Product of Array Except Self]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/products-of-array-discluding-self/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> For each index, the answer is the product of everything to its left multiplied by the product of everything to its right. Build prefix and suffix product arrays so that `answer[i] = prefix[i] * suffix[i]`.

---

## ⚙️ Approaches

### Approach 1: Brute Force

- **Idea:** For every index, loop through the entire array and multiply every element except the current one.
- **Time Complexity:** $O(n^2)$
- **Space Complexity:** $O(1)$ extra space, excluding the output array.

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        result = []

        for i in range(len(nums)):
            product = 1

            for j in range(len(nums)):
                if i != j:
                    product *= nums[j]

            result.append(product)

        return result
````

---

### Approach 2: Prefix + Suffix Products

- **Intuition / Pattern:** Build a `prefix` array where `prefix[i]` is the product of everything to the left of `nums[i]`. Build a `suffix` array where `suffix[i]` is the product of everything to the right. Multiply them together for each index.
    
- **Time Complexity:** $O(n)$
    
- **Space Complexity:** $O(n)$
    

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)

        prefix = [1] * n
        suffix = [1] * n

        # Product of everything to the left
        for i in range(1, n):
            prefix[i] = prefix[i - 1] * nums[i - 1]

        # Product of everything to the right
        for i in range(n - 2, -1, -1):
            suffix[i] = suffix[i + 1] * nums[i + 1]

        return [prefix[i] * suffix[i] for i in range(n)]
```

### Example

```text
nums   = [1,  2,  3, 4]

prefix = [1,  1,  2, 6]
suffix = [24, 12, 4, 1]

answer = [24, 12, 8, 6]
```

For index `2`:

```text
nums[2] = 3

product to the left  = 1 × 2 = 2
product to the right = 4

answer = 2 × 4 = 8
```

---

## 🧠 What I Learned

> `prefix[i]` means the product of everything before `nums[i]`.

> `suffix[i]` means the product of everything after `nums[i]`.

Therefore:

```text
answer[i] = prefix[i] × suffix[i]
```

This avoids division and handles zeros naturally.

---

## ⚠️ Pitfalls & Edge Cases

-  Do not include `nums[i]` in either its prefix or suffix product
    
-  Initialize prefix and suffix products with `1`
    
-  Zeros work naturally with the prefix/suffix approach
    
-  Negative numbers also work normally
    
-  Build the suffix array from right to left
    
-  Make sure prefix/suffix indices align directly with `nums`
    

---

## 🔁 Spaced Repetition Tracker

| Review       | Date       | Solved Without Hints? (Y/N) | Notes / Reflections                                                                                                                                                                                                                                     |
| ------------ | ---------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1st Pass** | 2026-08-19 | N                           | Independently identified division and brute-force approaches. Needed a hint for the prefix/suffix insight. After the concept was explained, independently built a working prefix/suffix solution, though the initial suffix indexing was less readable. |
| **2nd Pass** |            |                             |                                                                                                                                                                                                                                                         |
| **3rd Pass** |            |                             |                                                                                                                                                                                                                                                         |

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Arrays & Hashing]]
    
- Related Pattern: [[Pattern - Prefix and Suffix]]
    
- Similar Concept: [[Trapping Rain Water]]