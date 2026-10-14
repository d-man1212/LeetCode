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

# [[Valid Sudoku]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/valid-sudoku/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Each number must be unique within three independent groups: its row, its column, and its 3×3 box. While traversing the board, track previously seen numbers using sets and return `False` immediately if a duplicate appears in any of these groups.

---

## ⚙️ Approaches

### Approach 1: Hash Sets for Rows, Columns, and Boxes

- **Idea:** Create sets to track values seen in each row and column. Use a dictionary of sets for the 3×3 boxes, where each box is identified by `(i // 3, j // 3)`.
- **Time Complexity:** $O(n^2)$ for a generalized `n × n` board
- **Space Complexity:** $O(n^2)$ in the generalized case

> For the fixed 9×9 Sudoku board, both are technically $O(1)$.

#### Code Implementation

```python
from collections import defaultdict

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        rowSets = [set() for _ in range(9)]
        colSets = [set() for _ in range(9)]
        boxSets = defaultdict(set)

        for i in range(9):
            for j in range(9):
                current = board[i][j]

                if current == '.':
                    continue

                if (
                    current in rowSets[i]
                    or current in colSets[j]
                    or current in boxSets[(i // 3, j // 3)]
                ):
                    return False

                rowSets[i].add(current)
                colSets[j].add(current)
                boxSets[(i // 3, j // 3)].add(current)

        return True
````

---

## ⚠️ Pitfalls & Edge Cases

-  Ignore empty cells represented by `.`
    
-  Check for duplicates before adding the current value
    
-  Track rows and columns independently
    
-  Correctly identify the 3×3 box using `(row // 3, col // 3)`
    
-  Return immediately when a duplicate is found
    
-  Do not confuse validating Sudoku with solving Sudoku
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes / Reflections|
|---|---|---|---|
|**1st Pass**|2026-08-18|N|Built the row/column/box tracking solution. Needed hints for using sets to track duplicates and for the 3×3 box mapping `(i // 3, j // 3)`.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Arrays & Hashing]]
    
- Similar Problem: [[Contains Duplicate]]
    
- Related Concept: [[Hash Set]]