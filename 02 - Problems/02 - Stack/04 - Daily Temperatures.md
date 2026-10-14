---
date_created: 2026-08-20
tags:
  - dsa/problem
  - dsa/pattern/stack
  - status/solved
platform: NeetCode
difficulty: Medium
pattern: Stack
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-20
next_review:
---

# [[Daily Temperatures]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/daily-temperatures/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Instead of looking forward from every day, keep a stack of indices for days that are still waiting for a warmer temperature. When a warmer temperature appears, pop the waiting indices and calculate how many days they had to wait.

---

## ⚙️ Approaches

### Approach 1: Brute Force

- **Idea:** For each temperature, scan forward until finding the first warmer temperature. The number of days waited is the difference between the two indices.
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n)

#### Code Implementation

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)

        result = []

        for i in range(n):
            value = 1
            j = i + 1

            while j < n:
                if temperatures[i] >= temperatures[j]:
                    value += 1
                else:
                    result.append(value)
                    break

                j += 1
            else:
                result.append(0)

        return result
```

---

### Approach 2: Monotonic Stack — Optimal

- **Intuition / Pattern:** Keep indices of temperatures that have not yet found a warmer day. When the current temperature is warmer than the temperature at the top index, that day has found its answer, so pop it and calculate `i - old`.
- **Time Complexity:** O(n)
- **Space Complexity:** O(n)

#### Code Implementation

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)

        result = [0 for _ in range(n)]
        stackIndices = []

        for i in range(n):
            while stackIndices and temperatures[i] > temperatures[stackIndices[-1]]:
                old = stackIndices.pop()
                result[old] = i - old

            stackIndices.append(i)

        return result
```

### Why This Is O(n)

Although there is a `while` loop inside the `for` loop, each index is:

1. Pushed onto the stack exactly once.
2. Popped from the stack at most once.

Therefore, the total number of stack operations is O(n), giving an overall **O(n)** time complexity.

---

## ⚠️ Pitfalls & Edge Cases

- [x] Empty input → return an empty result
- [x] Last day → no warmer day, so answer remains `0`
- [x] Monotonically decreasing temperatures → all answers remain `0`
- [x] Equal temperatures → equal temperature is not warmer, so do not pop
- [x] Use `>` rather than `>=`
- [x] Store indices, not just temperatures, because the index difference gives the number of days
- [x] Initialize the stack as empty rather than `[0]` to handle empty input

---

## 🔁 Spaced Repetition Tracker

| Review | Date | Solved Without Hints? (Y/N) | Notes |
|---|---|---|---|
| **1st Pass** | 2026-08-20 | Y | Independently implemented the brute-force solution, then derived the monotonic-stack solution after a hint to think about unresolved previous temperatures. Needed only a small edge-case correction to initialize the stack as empty. |
| **2nd Pass** | | | |
| **3rd Pass** | | | |

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Stack]]
- Similar Problem: [[Min Stack]]
- Similar Problem: [[Evaluate Reverse Polish Notation]]
- Related Pattern: [[Monotonic Stack]]
````