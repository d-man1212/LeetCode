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

# [[Evaluate Reverse Polish Notation]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/evaluate-reverse-polish-notation/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Reverse Polish Notation can be evaluated naturally with a stack. Push numbers onto the stack, and whenever an operator appears, pop the two most recent numbers, perform the operation in the correct order, and push the result back.

---

## ⚙️ Approaches

### Approach 1: Stack

- **Idea:** Iterate through the tokens. Numbers are pushed onto the stack. When an operator appears, pop the right operand first and then the left operand, perform the operation, and push the result back.
- **Time Complexity:** O(n)
- **Space Complexity:** O(n)

#### Code Implementation

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        operations = {'+', '-', '*', '/'}

        for i in tokens:
            if i not in operations:
                stack.append(i)
                continue

            a = int(stack.pop())
            b = int(stack.pop())

            if i == '+':
                result = b + a
            elif i == '-':
                result = b - a
            elif i == '*':
                result = b * a
            else:
                result = int(b / a)

            stack.append(result)

        return int(stack[-1])
```

---

## ⚠️ Pitfalls & Edge Cases

- [x] Operand order matters → `a` is the right operand and `b` is the left operand
- [x] Subtraction must be `b - a`, not `a - b`
- [x] Division must truncate toward zero
- [x] Python `//` should not be used because it performs floor division
- [x] Negative numbers need to truncate toward zero
- [x] Operators should only be applied after popping two operands
- [x] Intermediate results should be pushed back onto the stack

### Important Python Detail

```python
7 // 3     # 2
-7 // 3    # -3  ← wrong for this problem
```

The problem requires truncation toward zero, so:

```python
int(-7 / 3)  # -2
```

---

## 🔁 Spaced Repetition Tracker

| Review | Date | Solved Without Hints? (Y/N) | Notes |
|---|---|---|---|
| **1st Pass** | 2026-08-20 | Y | Independently found the stack-based solution and correctly handled operand ordering. Needed a hint about Python division: `/` returns a float and `//` floors toward negative infinity, while the problem requires truncation toward zero. |
| **2nd Pass** | | | |
| **3rd Pass** | | | |

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Stack]]
- Similar Problem: [[Min Stack]]
- Similar Problem: [[Valid Parentheses]]
````