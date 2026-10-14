---
date_created: 2026-08-20
tags:
  - dsa/problem
  - dsa/pattern/stack
  - status/solved
platform: NeetCode
difficulty: Easy
pattern: Stack
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-20
next_review:
---

# [[Valid Parentheses]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/validate-parentheses/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Use a stack because brackets must be matched in reverse order of when their opening brackets appeared. Push opening brackets onto the stack, and when a closing bracket appears, it must match the most recently opened bracket.

---

## ⚙️ Approaches

### Approach 1: Stack + Hash Map

- **Idea:** Store opening brackets in a stack. Use a mapping from each closing bracket to its corresponding opening bracket. When a closing bracket appears, check whether it matches the top of the stack.
- **Time Complexity:** O(n)
- **Space Complexity:** O(n)

#### Code Implementation

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []

        leftFacing = {'(', '[', '{'}
        mapping = {
            ')': '(',
            ']': '[',
            '}': '{'
        }

        for i in s:
            if i in leftFacing:
                stack.append(i)
            else:
                if not stack or stack[-1] != mapping[i]:
                    return False

                stack.pop()

        return not stack
````

### Simplified Implementation

The separate `leftFacing` set isn't necessary because `mapping` already contains every closing bracket. We can use membership in `mapping` to distinguish closing brackets from opening brackets.

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []

        mapping = {
            ')': '(',
            ']': '[',
            '}': '{'
        }

        for char in s:
            if char not in mapping:
                stack.append(char)
            else:
                if not stack or stack[-1] != mapping[char]:
                    return False

                stack.pop()

        return not stack
```

---

## ⚠️ Pitfalls & Edge Cases

-  Empty string → valid
    
-  Single opening bracket → invalid
    
-  Closing bracket with an empty stack → invalid
    
-  Mismatched bracket types → invalid
    
-  Correct pairs in the wrong order → invalid
    
-  Nested brackets → stack handles naturally
    
-  All opening brackets with no closing brackets → invalid
    
-  The stack must be empty at the end
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes / Reflections|
|---|---|---|---|
|**1st Pass**|2026-08-20|Y|Independently recognized the stack approach and implemented the closing-to-opening bracket mapping. Received only a small readability hint to remove the separate `leftFacing` set.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Stack]]
    
- Similar Problem: [[Min Stack]]
    
- Similar Problem: [[Evaluate Reverse Polish Notation]]
    

```

 
```