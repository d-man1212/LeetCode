---
date_created: 2026-08-23
tags:
  - dsa/problem
  - dsa/pattern/two_pointers
  - status/solved
platform: NeetCode
difficulty: Easy
pattern: Two Pointers
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-23
next_review:
---

# [[Valid Palindrome]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/is-palindrome/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> A palindrome reads the same forwards and backwards. Use two pointers starting from both ends of the original string, skip non-alphanumeric characters, and compare the valid characters while moving both pointers inward.

> We don't need to create a cleaned version of the string first. We can skip irrelevant characters in-place while scanning from both ends.

---

## ⚙️ Approaches

### Approach 1: Clean String + Two-Pointer Comparison

- **Idea:** First create a new string containing only alphanumeric characters, convert it to lowercase, and then compare characters from opposite ends.
- **Time Complexity:** $O(n)$
- **Space Complexity:** $O(n)$

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        newStr = ''

        for i in range(len(s)):
            if (('z' >= s[i] >= 'a') or
                ('Z' >= s[i] >= 'A') or
                ('9' >= s[i] >= '0')):
                newStr += s[i]

        newStr = newStr.lower()

        for i in range(len(newStr) // 2):
            if newStr[i] != newStr[-1 - i]:
                return False

        return True
````

### Notes

This approach works, but constructing `newStr` requires additional memory.

Also, repeatedly doing:

```python
newStr += s[i]
```

can be less efficient because Python strings are immutable.

---

### Approach 2: Two Pointers — Optimal

- **Idea:** Keep two pointers directly on the original string. Skip non-alphanumeric characters from either side, then compare the characters at `left` and `right`. If they match, move both pointers inward.
    
- **Time Complexity:** $O(n)$
    
- **Space Complexity:** $O(n)$ in the submitted version because `s.lower()` creates a new string.
    

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1

        s = s.lower()

        while left < right:

            if not s[left].isalnum():
                left += 1
                continue

            if not s[right].isalnum():
                right -= 1
                continue

            if s[left] != s[right]:
                return False

            left += 1
            right -= 1

        return True
```

### Why the `continue` matters

When the left character isn't relevant:

```python
if not s[left].isalnum():
    left += 1
    continue
```

we immediately restart the loop.

This prevents us from comparing the left character until it points to a valid alphanumeric character.

The same applies to the right pointer.

### Example

For:

```text
"A man, a plan, a canal: Panama"
```

The pointers effectively compare:

```text
A ↔ a
m ↔ m
a ↔ a
n ↔ n
...
```

while skipping:

```text
spaces
,
:
```

The string is therefore a palindrome.

---

## Approach 3: Two Pointers with O(1) Extra Space

The only extra space in Approach 2 comes from:

```python
s = s.lower()
```

Instead, the case conversion can be performed only when comparing the two valid characters.

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1

        while left < right:

            if not s[left].isalnum():
                left += 1
                continue

            if not s[right].isalnum():
                right -= 1
                continue

            if s[left].lower() != s[right].lower():
                return False

            left += 1
            right -= 1

        return True
```

- **Time Complexity:** $O(n)$
    
- **Space Complexity:** $O(1)$
    

This is the strictest optimal version.

---

## 🧠 Pattern Recognition

This is a classic **Two Pointers** problem.

When you see:

> "Check something from both ends"

immediately consider:

```text
left  → → → 
        ← ← ← right
```

Instead of creating a reversed string or another processed version, ask:

> Can I compare both ends directly while moving the pointers inward?

For this problem, the extra complication is that some characters should be ignored.

So the pointer flow becomes:

```text
1. Start left at beginning
2. Start right at end
3. Skip invalid character on left
4. Skip invalid character on right
5. Compare valid characters
6. Move both inward
7. Repeat
```

---

## ⚠️ Pitfalls & Edge Cases

-  Ignore spaces
    
-  Ignore punctuation
    
-  Ignore other non-alphanumeric characters
    
-  Comparison is case-insensitive
    
-  Empty string → `True`
    
-  Single character → `True`
    
-  String containing only non-alphanumeric characters → `True`
    
-  Stop when `left >= right`
    
-  Don't move both pointers when only one side contains an invalid character
    
-  Compare only after both pointers point to valid characters
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes / Reflections|
|---|---|---|---|
|**1st Pass**|2026-08-23|Y|Initially implemented a cleaned-string solution independently. Then independently implemented the two-pointer solution after being prompted to consider avoiding `newStr`.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Two Pointers]]
    
- Similar Problem: [[Two Sum II - Input Array Is Sorted]]
    
- Similar Problem: [[Container With Most Water]]
    
- Similar Problem: [[3Sum]]