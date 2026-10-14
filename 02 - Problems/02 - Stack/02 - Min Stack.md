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

# [[Min Stack]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/minimum-stack/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> A normal stack can find the minimum with `min()`, but that takes O(n). Maintain a second stack that tracks the minimum value at each relevant point, so `getMin()` can return the current minimum in O(1).

---

## ⚙️ Approaches

### Approach 1: Brute Force

- **Idea:** Use a normal stack for all values and call `min()` on the stack whenever `getMin()` is called.
- **Time Complexity:** O(n) for `getMin()`
- **Space Complexity:** O(n)

#### Code Implementation

```python
class MinStack:

    def __init__(self):
        self.stack = []

    def push(self, value: int) -> None:
        self.stack.append(value)

    def pop(self) -> None:
        self.stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return min(self.stack)
````

---

### Approach 2: Two Stacks — Optimal

- **Intuition / Pattern:** Maintain a second stack containing the current minimum values. Whenever a value is pushed that is smaller than or equal to the current minimum, also push it onto `minStack`. When that minimum is removed from the main stack, remove it from `minStack` as well.
    
- **Time Complexity:** O(1) for every operation
    
- **Space Complexity:** O(n)
    

#### Code Implementation

```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.minStack = []

    def push(self, value: int) -> None:
        if not self.minStack or self.minStack[-1] >= value:
            self.minStack.append(value)

        self.stack.append(value)

    def pop(self) -> None:
        if self.stack.pop() == self.minStack[-1]:
            self.minStack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.minStack[-1]
```

---

## ⚠️ Pitfalls & Edge Cases

-  Duplicate minimum values → use `>=` when pushing to `minStack`
    
-  Popping the current minimum → also remove it from `minStack`
    
-  Popping a non-minimum → leave `minStack` unchanged
    
-  `top()` should only inspect the normal stack
    
-  `getMin()` should return `minStack[-1]`, not scan the normal stack
    
-  Negative values work normally
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes|
|---|---|---|---|
|**1st Pass**|2026-08-20|Y|Independently implemented the brute-force solution, then independently found the two-stack solution. No solution hint was needed.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Stack]]
    
- Similar Problem: [[Valid Parentheses]]
    
- Similar Problem: [[Evaluate Reverse Polish Notation]]