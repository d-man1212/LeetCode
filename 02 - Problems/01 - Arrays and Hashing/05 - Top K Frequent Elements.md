---
date_created: 2026-08-19
tags:
  - dsa/problem
  - dsa/pattern/arrays_and_hashing
  - dsa/pattern/bucket-sort
  - status/solved
platform: NeetCode
difficulty: Medium
pattern: Arrays & Hashing
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-19
next_review:
---

# [[Top K Frequent Elements]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/top-k-elements-in-list/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Count how frequently each number appears, then use the frequency itself as the bucket index. Starting from the highest-frequency bucket lets us collect the top `k` elements without sorting.

---

## ⚙️ Approaches

### Approach 1: Frequency Map + Sorting

- **Idea:** Count the frequency of each number, then sort the unique numbers by their frequency in descending order and return the first `k`.
- **Time Complexity:** O(n + u log u)
- **Space Complexity:** O(u)

Where `u` is the number of unique values.

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:

        numberToFrequency = defaultdict(int)

        for i in nums:
            numberToFrequency[i] += 1

        sortedFrequency = sorted(
            set(nums),
            key=lambda x: numberToFrequency[x],
            reverse=True
        )

        return [sortedFrequency[i] for i in range(k)]
````

**Limitation:** We don't actually need to completely sort all unique numbers when we only want the top `k`.

---

### Approach 2: Bucket Sort

- **Intuition / Pattern:** A number can appear at most `n` times, where `n = len(nums)`. Create buckets where the index represents frequency. For example, `buckets[3]` contains all numbers that occur exactly 3 times. Then iterate from the highest frequency down and collect numbers until we have `k`.
    
- **Time Complexity:** O(n)
    
- **Space Complexity:** O(n)
    

#### Code Implementation

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:

        numberToFrequency = defaultdict(int)

        for num in nums:
            numberToFrequency[num] += 1

        n = len(nums)
        buckets = [[] for _ in range(n + 1)]

        for num, freq in numberToFrequency.items():
            buckets[freq].append(num)

        result = []
        i = n

        while len(result) < k:
            result.extend(buckets[i][:k - len(result)])
            i -= 1

        return result
```

#### Alternate Code Implementation

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        numberToFrequency = defaultdict(int)
        frequencyToNumber = defaultdict(list)

        for num in nums:
            numberToFrequency[num] += 1

        n = len(nums)

        for num, freq in numberToFrequency.items():
            frequencyToNumber[freq].append(num)

        result = []
        i = n

        while len(result) < k:
            result.extend(frequencyToNumber[i][:k - len(result)])
            i -= 1

        return result
```

---

## 🧠 What I Learned

### Why Buckets Work

If:

```text
nums = [1,1,1,2,2,3]
```

The frequency map is:

```text
1 → 3
2 → 2
3 → 1
```

The buckets become:

```text
frequency 1 → [3]
frequency 2 → [2]
frequency 3 → [1]
```

Starting from the highest frequency gives:

```text
[1] → [1, 2] → ...
```

So for `k = 2`, the answer is `[1, 2]`.

### Important Insight

> We don't need to sort the numbers by frequency. Since frequency is bounded between `1` and `n`, we can use the frequency as an array index.

---

## ⚠️ Pitfalls & Edge Cases

-  A number can appear at most `n` times
    
-  `buckets[frequency]` stores all numbers with that frequency
    
-  Iterate from highest frequency to lowest
    
-  A bucket can contain multiple numbers
    
-  Don't add more than `k` elements to the result
    
-  `buckets[i][:k - len(result)]` prevents returning more than `k` elements
    
-  Empty buckets should simply be skipped
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes / Reflections|
|---|---|---|---|
|**1st Pass**|2026-08-19|N|Independently implemented the frequency-map + sorting solution. Needed hints to recognize and construct the bucket-sort approach. After understanding the bucket idea, implemented it independently.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Arrays & Hashing]]
    
- Pattern: [[Pattern - Bucket Sort]]
    
- Similar Problem: [[Group Anagrams]]
    
- Similar Problem: [[Top K Frequent Words]]