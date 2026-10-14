---
date_created: 2026-08-19
tags:
  - dsa/problem
  - dsa/pattern/arrays_and_hashing
  - dsa/pattern/string_encoding
  - status/solved
platform: NeetCode
difficulty: Medium
pattern: Arrays & Hashing
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-19
next_review:
---

# [[Encode and Decode Strings]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/string-encode-and-decode/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Store each string as `length#string`. During decoding, first read the length before `#`, then take exactly that many characters as the next string. The original string can contain `#` because the length tells us exactly where the string ends.

### Mental Model

> **Read length → skip `#` → take exactly `length` characters → repeat.**

Example:

```text
["cat", "hello"]
        ↓
3#cat5#hello
````

---

## ⚙️ Approaches

### Approach 1: Length Prefix Encoding

- **Idea:** Prefix every string with its length followed by `#`. While decoding, find the `#` to determine the length, then use that length to extract the exact number of characters belonging to the original string.
    
- **Time Complexity:** O(n), where `n` is the total number of characters.
    
- **Space Complexity:** O(n)
    

### Code Implementation

```python
class Solution:

    def encode(self, strs: List[str]) -> str:
        finalStr = ''

        for string in strs:
            finalStr += str(len(string)) + '#' + string

        return finalStr


    def decode(self, s: str) -> List[str]:
        i = 0
        array = []

        while i < len(s):
            foundHash = s.find('#', i)
            length = int(s[i:foundHash])

            start = foundHash + 1
            end = start + length

            array.append(s[start:end])
            i = end

        return array
```

---

## 🧠 How Decoding Works

Suppose:

```python
["1#", "2#"]
```

Encoded:

```text
2#1#2#2#
```

The decoder does **not** use every `#` as a separator between strings.

Instead:

```text
2#1#2#2#
↑
```

1. Read `2` until the first `#`
    
2. Length = `2`
    
3. Take the next 2 characters → `"1#"`
    
4. Move the pointer forward
    
5. Repeat
    

The important point:

> The `#` is only used to mark the end of the length prefix. Once the length is known, we take exactly that many characters, even if those characters contain `#`.

---

## ⚠️ Pitfalls & Edge Cases

-  Empty input → `[]`
    
-  Empty strings → encoded as `0#`
    
-  Strings containing `#`
    
-  Strings containing numbers
    
-  Multi-digit lengths such as `12#hello world!`
    
-  Move the pointer to the end of the decoded string before processing the next one
    
-  Do not rely on splitting by `#`, since `#` can appear inside an original string
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes / Reflections|
|---|---|---|---|
|**1st Pass**|2026-08-19|N|Got stuck at the starting point and needed help understanding the length-prefix idea. After understanding `length#string`, independently implemented both `encode()` and `decode()`.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Arrays & Hashing]]
    
- Related Pattern: [[Pattern - String Encoding]]
    
- Similar Concept: [[Serialize and Deserialize Binary Tree]]